---
title: Mosn 启动流程v1.6.0
linkTitle: Mosn 启动流程v1.6.0
date: 2023-12-17
aliases: "/blog/posts/mosn-startup/v1.6.0"
weigth: 2
author: "[wp500](https://github.com/wang55www)"
description: MOSN 源码解析基于 Mosn v1.6.0版本
---

mosn利用cli组件(github.com/urfave/cli)来作为实现命令行的控制。默认是cmdStart，之后就进入下面的启动逻辑。
程序入口 control.go cmdStart.Action方法用来做程序调用的入口。
mosn对象创建，首先调用NewMosn()这个方法只是返回了一个没有完全被赋值的Mosn对象。对象定义如下： 
```go
type Mosn struct {
	isFromUpgrade  bool // hot upgrade from old MOSN
	Upgrade        UpgradeData
	Clustermanager types.ClusterManager
	RouterManager  types.RouterManager
	Config         *v2.MOSNConfig
	// internal data
	servers   []server.Server
	xdsClient *istio.ADSClient
}
```

接下来就是创建另外一个叫StageManager的对象，这个对象是用来管理Mosn的生命周期，定义如下：
 
```go
// stagemanagr/stage_manager.go
// StageManager is used to controls service life stages.
type StageManager struct {
	lock                    sync.Mutex
	state                   State
	exitCode                int
	stopAction              StopAction
	data                    Data  //保存了app不同生命周期阶段所使用的数据
	app                     Application // Application interface app其实就是mosn
	wg                      sync.WaitGroup
    //以下是不同生命周期对应的处理函数 -- start
	paramsStages            []func(*cli.Context)  //参数准备阶段
	initStages              []func(*v2.MOSNConfig) //初始化场景
	preStartStages          []func(Application)  //启动之前
	startupStages           []func(Application)  //启动
	afterStartStages        []func(Application)  //启动之后
	beforeStopStages        []func(StopAction, Application) error //停止之前
	gracefulStopStages      []func(Application) error  //优雅停止
	afterStopStages         []func(Application) //停止之后处理
    //生命周期阶段 --- end  
	onStateChangedCallbacks []func(State)
	upgradeHandler          func() error // old server: send listener/config/old connections to new server
	newServerC              chan bool
}

// Data contains objects used in stages
type Data struct {
	// ctx contains the start parameters
	ctx *cli.Context
	// config path represents the config file path,
	// will create basic config from it and if auto config dump is set,
	// new config data will write into this path
	configPath string
	// basic config, created after parameters parsed stage
	config *v2.MOSNConfig
}


```

   - 可以看到stage_manager的生命周期包含 参数准备、初始化、启动之前处理、启动、启动之后处理、停止之前处理、优雅停止、停止之后处理等几个阶段。每个阶段对应的是一个函数的数组，也就是说每个阶段的处理可以有多个处理函数。
   - 这里可以好好的看一下stage_manager.go的源码，里面定义了11种场景的状态和2种额外的场景(注释里把场景的含义描述的比较清楚)，那么当状态发生变化的时候就会调用上面说的回调函数，同时还定义了一个Application的接口。这个接口的实现就是Mosn对象，Application被stageManager所管理，Application本身没有那么多的场景,应用的生命周期(application不同的生命周期，会触发stage场景的切换)只有初始化，启动，停止。那么stageManager根据这些周期的变化，同时切换场景。
- 阶段总结，到这里先不着急看启动的逻辑，我们先总结一下目前掌握的内容。
   - ![图片.png](https://cdn.nlark.com/yuque/0/2023/png/2687467/1699761896610-4049c15c-2b10-4ec6-b252-a43f318046c9.png#averageHue=%23f5f5f5&clientId=u9da8dcf3-6aba-4&from=paste&height=460&id=u0ad72238&originHeight=920&originWidth=924&originalType=binary&ratio=2&rotation=0&showTitle=false&size=105239&status=done&style=none&taskId=ua875a477-84a3-440e-8278-b146d32a98f&title=&width=462)<br />如上图所示，Application应用其实就是Mosn本身包含若干方法：初始化、启动、停止这些方法可以调用让应用进入不同的生命周期，那么对应场景对应应用的生命周期状态也一直发生改变，二者是有关联的。那么先不看源码只凭借猜测，到底是先让应用的生命周期发生变化后触发场景改变状态；还是先触发场景改变状态进而触发应用来改变生命周期呢？这个之前我们分析代码的时候说过，场景StageManager用来管理应用生命周期，那么应该是先由场景来触发状态改变，再调用应用对应的方法来改变生命周期。其实因为场景的状态有11个粒度要比应用的生命周期粒度更细，从这点上来看只可能场景状态细粒度切换同时调用应用的方法切换生命周期，而反之行不通(粒度大的一方无法识别什么时候调用小粒度一方)。
   - 既然我猜测是StageManager场景来控制切换，那么肯定有对应的方法提供场景切换。这个时候我们再来看代码，发现确实存在这样的方法。如下：<br />![图片.png](https://cdn.nlark.com/yuque/0/2023/png/2687467/1699761318557-a0006d6e-836b-4d7d-8a7b-5a5459c6c898.png#averageHue=%23434343&clientId=u0fbda8d3-50b4-4&from=paste&height=356&id=u1fed9754&originHeight=712&originWidth=628&originalType=binary&ratio=2&rotation=0&showTitle=false&size=103210&status=done&style=none&taskId=u2e7cfc4e-c0db-48c9-b96a-0974ca98d2a&title=&width=314)
- 我们继续看一下Run()方法，可以清楚的看到里面执调用了不同子阶段，每个阶段都会有切换状态的逻辑，在最后的逻辑也能看到设置状态为Running。
```go
// Run until the application is started
func (stm *StageManager) Run() {
	// 1: parser params
	stm.runParamsParsedStage()
	// 2: init
	stm.runInitStage()
	// 3: pre start
	stm.runPreStartStage()
	// 4: run
	stm.runStartStage()
	// 5: after start
	stm.runAfterStartStage()

	stm.SetState(Running)
}
```

- 可以看到这个Run() 方法执行了参数解析、初始化、启动前处理、启动、启动后处理等方法。同样我们猜测每个方法一定是执行前面说的每个阶段对应的回调函数数组。那我们找一个方法看一下，果然如猜测一样。
```go
func (stm *StageManager) runParamsParsedStage() {
	st := time.Now()
    //设置场景状态
	stm.SetState(ParamsParsed) 
    //果然如猜测，遍历回调函数的数组并调用
	for _, f := range stm.paramsStages {
		f(stm.data.ctx)
	}
	// after all registered stages are completed
	stm.data.config = configmanager.Load(stm.data.configPath)

	log.StartLogger.Infof("parameters parsed stage cost: %v", time.Since(st))
}

```

- 可以说Run()方法作用就是场景管理器stageManager来启动应用Application，那这个Run()方法在什么逻辑里被调用呢。梳理一下代码，调用链路是：control.go cmdStart.Action(就是前面提到的程序启动的入口)->stm.RunAll()->stm.Run()。
- stm.RunAll()是顺序的启动了场景管理器中所有的场景,其中Run是启动，之后就等待server停止，最后是Stop场景
```go
// run all stages
func (stm *StageManager) RunAll() {
	// start to work
	stm.Run()
	// wait server finished
	stm.WaitFinish()
	// stop working
	stm.Stop()
}
```

- 好了现在我们回到最开始的control.go cmdStart.Action逻辑。首先line 3 创建应用 line 5 创建场景管理器，line 32启动场景管理器触发执行启动、等待停止、停止。而中间(第6行到第29行)的一堆逻辑其实就是在设置StageManager(场景管理器)，会设置每个状态的回调函数，因为后面的调用场景切换的时候其实就是调用这里设置的回调函数。
```go
		Action: func(c *cli.Context) error {
            // 创建Application
			app := mosn.NewMosn()
            // 创建stagemanager场景管理器
			stm := stagemanager.InitStageManager(c, c.String("config"), app)
			// if needs featuregate init in parameter stage or init stage
			// append a new stage and called featuregate.ExecuteInitFunc(keys...)
			// parameter parsed registered
			stm.AppendParamsParsedStage(ExtensionsRegister)
			stm.AppendParamsParsedStage(DefaultParamsParsed)
			// initial registered
			stm.AppendInitStage(func(cfg *v2.MOSNConfig) {
				drainTime := c.Int("drain-time-s")
				server.SetDrainTime(time.Duration(drainTime) * time.Second)
				// istio parameters
				serviceCluster := c.String("service-cluster")
				serviceNode := c.String("service-node")
				serviceType := c.String("service-type")
				serviceMeta := c.StringSlice("service-meta")
				metaLabels := c.StringSlice("service-lables")
				clusterDomain := c.String("cluster-domain")
				podName := c.String("pod-name")
				podNamespace := c.String("pod-namespace")
				podIp := c.String("pod-ip")

				if serviceNode != "" {
					istio1106.InitXdsInfo(cfg, serviceCluster, serviceNode, serviceMeta, metaLabels)
				} else {
					if istio1106.IsApplicationNodeType(serviceType) {
						sn := podName + "." + podNamespace
						serviceNode = serviceType + "~" + podIp + "~" + sn + "~" + clusterDomain
						istio1106.InitXdsInfo(cfg, serviceCluster, serviceNode, serviceMeta, metaLabels)
					} else {
						log.StartLogger.Infof("[mosn] [start] xds service type is not router/sidecar, use config only")
						istio1106.InitXdsInfo(cfg, "", "", nil, nil)
					}
				}
			})
			stm.AppendInitStage(mosn.DefaultInitStage)
			stm.AppendInitStage(func(_ *v2.MOSNConfig) {
				// set version and go version
				metrics.SetVersion(Version)
				metrics.SetGoVersion(runtime.Version())
				admin.SetVersion(Version)
			})
			stm.AppendInitStage(holmes.Register)
			// pre-startup
			stm.AppendPreStartStage(mosn.DefaultPreStartStage) // called finally stage by default
			// startup
			stm.AppendStartStage(mosn.DefaultStartStage)
			// after-stop
			stm.AppendAfterStopStage(holmes.Stop)
            //执行所有场景
			// execute all stages
			stm.RunAll()
			return nil
		},

```

- **ParamsParsed 阶段解析**：(stm *StageManager) AppendParamsParsedStage(f func(*cli.Context)) 首先第一阶段注册了两个函数 ExtensionsRegister、DefaultParamsParsed 这两个函数作用：
   - ExtensionsRegister：主要用来初始化一些扩展的组件。这块我理解mosn在落地的时候会根据具体场景来接入一些特定的组件。这些组件如果需要初始化，可以在这个方法里初始化。为什么要在这个函数里初始化呢？一个是函数的参数是cli.Context命令行的封装可以方便获取命令行参数来初始化组件，另外这个函数执行也是整个生命周期最开始执行，如果需要mosn启动首先初始化的组件可以在这里来实现。而v1.6.0版本里实现，该函数主要来初始化一些链路追踪的配置，以及网络协议编解码的设置。这里就不展开分析了。
   - DefaultParamsParsed：这里就是mosn默认的在命令解析阶段进行初始化的内容一般不用修改。目前作用是用来设置日志级别及从命令行里解析各种开关。
- **InitStage阶段：** (stm *StageManager) AppendInitStage(f func(*v2.MOSNConfig)) *StageManager 这个是第二个阶段，这个阶段也是用来初始化，只不过初始化的来源是通过MOSN的配置文件。
   - line12-line38这部分主要是从Config中获取运行环境的相关元数据，用这些数据来初始化xds客户端。xds客户端使用xds协议与控制面组件进行交互。
   - line39 调用了mosn的默认初始化，这部分逻辑非常的多。里面又细分很多步骤。
```go
func DefaultInitStage(c *v2.MOSNConfig) {
	InitDefaultPath(c) //初始化mosn需要的相关运行时的目录,比如：日志，存储mosn进程ID的文件等
	InitDebugServe(c)   //启动mosn的debug信息查看服务，可以查看pprof信息
	InitializePidFile(c)  //初始化mosn pid持久化的文件
	InitializeTracing(c)   //初始化mosn 链路追踪的组件，根据配置文件中链路相关的配置。在之前的分析ParamParsed阶段会维护一些mosn支持的链路追踪的驱动列表，然后在这个阶段里会选择一个具体的组件作为链路追踪的实现。
	InitializePlugin(c) //这个阶段是初始化一下插件，不过看了实现其实就是初始化log配置
	InitializeWasm(c) //初始化web assembly 环境
	InitializeThirdPartCodec(c)  //初始化第三方的编解码的配置，这块我也不太理解，需要后面继续研究一下。个人感觉是加载动态代码，目前支持wasm和goPlugin
}
```

   - line 40-45 这部分比较简单，AppendInitStage 这部分就是初始化metrics初始化统计的组件
   - 接下来 stm.AppendInitStage(holmes.Register) 这句初始化一个蚂蚁开源的可观测性组件holmes。
- **PreStartStage阶段：**这个阶段逻辑并不复杂，主要是启动xds客户端。
```go
// Default Pre-start Stage wrappers
func DefaultPreStartStage(mosn stagemanager.Application) {
	m := mosn.(*Mosn)

	// start xds client
	_ = m.StartXdsClient()
	featuregate.FinallyInitFunc()  //初始化配置中指定的Feature
	m.HandleExtendConfig()  //将配置中扩展配置信息转换成对象
}
```

- **StartStage阶段**：这个阶段启动了mosn的管理服务，通过配置文件进行启动。
- **AfterStopStage阶段**：这个阶段是在mosn服务关闭后调用，这里就直接调用holmes.Stop关闭holmes组件。