---
title: "MOSN 作为 Istio 的数据平面"
linkTitle: "集成 Istio"
date: 2022-03-25
aliases: "/zh/docs/istio"
weight: 2
description: >
  本文将介绍如何使用 MOSN 在 Istio 框架下搭建 Service Mesh 的开发环境，并验证 MOSN 的一些基础路由能力、负载均衡能力等。
---

{{% pageinfo color="primary" %}}
MOSN 从 v1.0.0 版本开始 已通过 Istio 1.10.6 的 `Bookinfo` 测试，关于最新版 Istio 的支持情况可关注 [MOSN Istio WG](https://github.com/mosn/community/blob/master/wg-istio.md)。
{{% /pageinfo %}}

本文介绍的内容将包括 :

- MOSN 与 Istio 的关系
- 部署 Istio 与 MOSN
- Bookinfo 实验

## MOSN 与 Istio 的关系

我们曾在 [MOSN 介绍](../../overview) 中介绍过，MOSN 是一款采用 Go 语言开发的 Service Mesh 数据平面代理。

下图是 Istio 整体框架下，MOSN 的工作示意图。

<div align=center><img src="mosn-with-service-mesh.svg" width = "450" height = "400" alt="MOSN 介绍" /></div>


## 部署 Istio 与 MOSN
注意：Istio 1.10.6 不支持在 arm64 上运行 Kubernetes 的集群 。 [Istio Issues]([(https://github.com/istio/istio/issues/32841)])

### 安装 kubectl 命令行工具

kubectl 是用于针对 Kubernetes 集群运行命令的命令行接口，安装参考 [kubectl doc](https://kubernetes.io/docs/tasks/tools/install-kubectl)。

### 安装 Kubernetes 平台

安装 Istio，首先需要根据实际需求选择安装平台，可参考 Istio 官方文档推荐的方式 [Platform Setup](https://istio.io/latest/docs/setup/platform-setup/)。
后文中，我们假定选择的是`minikube`的安装方式，方便进行介绍。

安装minikube 流程：

1、根据本机环境选择下载地址 [Minikube 官网](https://minikube.sigs.k8s.io/docs/start/),下面用的系统是`macOS x86`系统。

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
```

2、`minikube start 
   --memory=7851 
   --cpus=4 
   --image-mirror-country='cn'` // 注意内存必须大于4GB，且镜像为`cn`(国内)

3、安装成功后，查看Pod情况

```bash
 minikube kubectl -- get pods -A 

NAMESPACE      NAME                                    READY   STATUS    RESTARTS      AGE
kube-system    coredns-64897985d-vw7b8                 1/1     Running   2 (16h ago)   7d20h
kube-system    etcd-minikube                           1/1     Running   2 (16h ago)   7d20h
kube-system    kube-apiserver-minikube                 1/1     Running   2 (16h ago)   7d20h
kube-system    kube-controller-manager-minikube        1/1     Running   2 (16h ago)   7d20h
kube-system    kube-proxy-cmjcq                        1/1     Running   2 (16h ago)   7d20h
kube-system    kube-scheduler-minikube                 1/1     Running   2 (16h ago)   7d20h
kube-system    storage-provisioner                     1/1     Running   5 (16h ago)   7d20h 
```

### 安装 Istio，使用 MOSN 作为数据面

1、下载对应的 Istio Release 版本，可以在 [Istio release](https://github.com/istio/istio/releases/tag/1.10.6) 页面下载与您操作系统匹配的压缩文件，或者使用官方提供的下载方式

```bash
VERSION=1.10.6 # istio version
export ISTIO_VERSION=$VERSION && curl -L https://istio.io/downloadIstio | sh -
```

2、下载完成以后（或者解压完成），切换到对应的目录，同时可以设置对应的`istioctl`命令行工具到环境变量，方便配置自定义 Istio 控制平面和数据平面配置参数。

```bash
cd istio-$ISTIO_VERSION/
export PATH=$PATH:$(pwd)/bin
```

3、执行默认安装 ```istioctl install```

```bash
 istioctl install

This will install the Istio 1.14.1 default profile with ["Istio core" "Istiod" "Ingress gateways"] components into the cluster. Proceed? (y/N) y
✔ Istio core installed                                                                                                                                                                                                                                                        
✔ Istiod installed                                                                                                                                                                                                                                                            
✔ Ingress gateways installed                                                                                                                                                                                                                                                  
✔ Installation complete
Making this installation the default for injection and validation.

Thank you for installing Istio 1.14.  Please take a few minutes to tell us about your install/upgrade experience!  https://forms.gle/yEtCbt45FZ3VoDT5A

```
安装成功，可以通过 ```minikube kubectl -- get pods -A```  查看pods 情况

```bash
 minikube kubectl -- get pods -A 

NAMESPACE      NAME                                    READY   STATUS    RESTARTS      AGE
istio-system   istio-ingressgateway-6b7fb88874-rgmrj   1/1     Running   0             2m44s
istio-system   istiod-65c9767c55-vjppv                 1/1     Running   0             3m12s

kube-system    coredns-64897985d-vw7b8                 1/1     Running   2 (16h ago)   7d20h
kube-system    etcd-minikube                           1/1     Running   2 (16h ago)   7d20h
kube-system    kube-apiserver-minikube                 1/1     Running   2 (16h ago)   7d20h
kube-system    kube-controller-manager-minikube        1/1     Running   2 (16h ago)   7d20h
kube-system    kube-proxy-cmjcq                        1/1     Running   2 (16h ago)   7d20h
kube-system    kube-scheduler-minikube                 1/1     Running   2 (16h ago)   7d20h
kube-system    storage-provisioner                     1/1     Running   5 (16h ago)   7d20h 
```
注意：当你失败时，可以通过 ```minikube ssh``` 进入虚机所构建的集群内部，并通过 ```docker pull ${失败的镜像} ``` 来获取失败镜像

4、创建 istio 命名空间，并且设置 MOSN proxyv2 镜像为数据面镜像

下载 Mosn proxyv2的镜像，并设置其为istio的proxy镜像。`docker pull mosnio/proxyv2:v1.0.0-1.10.6` ,也可以通过手动去创建proxy镜像 （[MOSN 与 Istio 的 proxyv2 镜像 build 方法介绍](../images)）
```bash
kubectl create namespace istio-system
istioctl manifest apply --set .values.global.proxy.image=${MOSN IMAGE} --set meshConfig.defaultConfig.binaryPath="/usr/local/bin/mosn"

例:
istioctl manifest apply --set .values.global.proxy.image= mosnio/proxyv2:v1.0.0-1.10.6 --set meshConfig.defaultConfig.binaryPath="/usr/local/bin/mosn"
```

5、验证 Istio 相关 POD 服务是否部署成功

```bash
kubectl get pod -n istio-system

NAME                                    READY   STATUS    RESTARTS   AGE
istio-ingressgateway-6b7fb88874-rgmrj   1/1     Running   0          102s
istiod-65c9767c55-vjppv                 1/1     Running   0          109s
```

如果 pod 显示所有容器 READY，并且 STATUS 为 Running，则表示 Istio 安装成功


## Bookinfo 实验

MOSN 已经通过 Istio 1.10.6 的 `Bookinfo` 测试，可以通过 [MOSN with Istio](https://katacoda.com/mosn/courses/istio/mosn-with-istio) 的教程来进行 Bookinfo 示例的演示操作，另外在该教程中您也可以找到更多关于使用 MOSN 和 Istio 的说明。
更多的使用场景可以参考 Istio 官方 [Example](https://istio.io/latest/docs/examples/)。
MOSN 目前并没有支持 Istio 的所有场景，如果您在运行实验过程中有遇到不支持的情况，请给我们提出 [issue](https://github.com/mosn/mosn/issues)，欢迎贡献代码。

### Bookinfo 介绍

`Bookinfo` 是一个类似豆瓣的图书应用，它包含四个基础服务：

-  Product Page：主页，由 python 开发，展示所有图书信息，它会调用 Reviews 和 Details 服务
-  Reviews：评论，由 java 开发，展示图书评论，会调用 Ratings 服务
-  Ratings：评分服务，由 nodejs 开发
-  Details：图书详情，由 ruby 开发

<div align=center><img src="bookinfo.png" width = "550" height = "400" alt="bookinfo" /></div>

#### 部署 `Bookinfo` 应用并注入 MOSN

> 详细过程可以参考 [Bookinfo doc](https://istio.io/docs/examples/bookinfo/)

通过 kube-inject 的方式实现 Sidecar 注入：

```bash
istioctl kube-inject -f samples/bookinfo/platform/kube/bookinfo.yaml > bookinfo.yaml
# sed -i '' is the MacOS command, if you are in linux, use sed -i instead.
sed -i '' "s/\/usr\/local\/bin\/envoy/\/usr\/local\/bin\/mosn/g" ./bookinfo.yaml
```

部署注入 Sidecar 后的 Bookinfo 应用：

```bash
$ kubectl apply -f bookinfo.yaml
```

验证部署是否成功：

```bash
$ kubectl get services
NAME          TYPE        CLUSTER-IP        EXTERNAL-IP   PORT(S)    AGE
details       ClusterIP   192.168.248.118   <none>        9080/TCP   5m7s
kubernetes    ClusterIP   192.168.0.1       <none>        443/TCP    25h
productpage   ClusterIP   192.168.204.255   <none>        9080/TCP   5m6s
ratings       ClusterIP   192.168.227.164   <none>        9080/TCP   5m7s
reviews       ClusterIP   192.168.181.16    <none>        9080/TCP   5m6s
```

等待所有的 pod 等成功运行起来：

```bash
$ kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
details-v1-77497b4899-67gfn       2/2     Running   0          98s
productpage-v1-68d9cf459d-mv7rh   2/2     Running   0          97s
ratings-v1-65f97fc6c5-npcrz       2/2     Running   0          98s
reviews-v1-6bf4444fcc-9cfrw       2/2     Running   0          97s
reviews-v2-54d95c5444-5jtxp       2/2     Running   0          97s
reviews-v3-dffc77d75-jd8cr        2/2     Running   0          97s
```

当上述状态为 Running 后，可通过如下方式确认 Bookinfo 应用是否正常运行：

```bash
kubectl exec -it $(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}') -c ratings -- curl productpage:9080/productpage | grep -o "<title>.*</title>"
```

#### 访问 Bookinfo 服务

开启 gateway 模式。

```bash
$ kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
$ kubectl get gateway
NAME               AGE
bookinfo-gateway   6s
```

设置 `GATEWAY_URL` 参考[文档](https://istio.io/docs/tasks/traffic-management/ingress/ingress-control/#determining-the-ingress-ip-and-ports)

```bash
$ export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
$ export INGRESS_HOST=$(minikube ip)
$ export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
```

验证 gateway 是否生效，输出 `200` 表示成功。

```bash
$ curl -o /dev/null -s -w "%{http_code}\n"  http://$GATEWAY_URL/productpage
200
```

**观察页面情况**

访问 `http://$GATEWAY_URL/productpage` （注意： `$GATEWAY_URL` 需要替换成你设置的地址），正常的话通过刷新会看到如下所示 `Bookinfo` 的界面，其中 Book Reviews 有三个版本，刷新后依次会看到（可以查看 samples/bookinfo/platform/kube/bookinfo.yaml 中的配置发现为什么是这三个版本）版本一的界面。

![版本一](v1.png)

版本二的界面。

![版本二](v2.png)

版本三的界面。

![版本三](v3.png)

#### 验证 MOSN 按 version 路由能力

首先为 `Bookinfo` 的 service 创建一系列的 destination rules。

```bash
$ kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml
```

指定 reviews 服务只访问 v1 版本。

```bash
$ kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml
```

访问 `http://$GATEWAY_URL/productpage` 发现 reviews 固定在如下版本一的页面不再变化。

![版本一](v1.png)

#### 验证 MOSN 按 weight 路由能力

我们通过下面操作将 v1 和 v3 版本各分配 50% 的流量。

```bash
$ kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-50-v3.yaml
```

访问 `http://$GATEWAY_URL/productpage` 这次 v1 和 v3 各有 1/2 几率出现。

#### 验证 MOSN 按照特定 header 路由能力

`Bookinfo` 系统右上角有一个登陆的入口，登陆以后请求会带上 end-user 这个自定义，值是 user name，Mosn 支持根据这个 header 的值来做路由。比如，我们尝试将 jason 这个用户路由到 v2 版本，其他的路由到 v1 版本（用户名和密码均是：jason，为什么是这个用户可以查看对应的 yaml 文件）。

```bash
$ kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml
```

访问 `http://$GATEWAY_URL/productpage` 时：

以 jason 身份登陆，会看到 v2 版本。

![登录](login.png)

以其他身份登录，始终在 v1 版本。

![版本一](v1.png)


### 卸载 Bookinfo

可以使用下面的命令来完成应用的删除和清理工作：

删除路由规则，并销毁应用的 Pod。

```bash
$ sh samples/bookinfo/platform/kube/cleanup.sh
```

确认 `Bookinfo` 应用已经关停：

```bash
$ kubectl get virtualservices   #-- there should be no virtual services
$ kubectl get destinationrules  #-- there should be no destination rules
$ kubectl get gateway           #-- there should be no gateway
$ kubectl get pods              #-- the Bookinfo pods should be deleted
```

## 卸载 Istio

执行如下命令，删除 Istio 相关 CRD 以及 pod 等资源：

```bash
$ istioctl manifest generate | kubectl delete -f -
```

确认 Istio 是否成功卸载：

```bash
$ kubectl get namespace istio-system
```
