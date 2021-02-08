---

title: "FeatureGate overview"
linkTitle: "FeatureGate overview"
date: 2020-03-13
weight: 2
description: >
  This topic describes FeatureGate of MOSN.
---

## FeatureGate overview

When using MOSN, you may want to enable some features at startup and disable those that you do not need. To meet this requirement, MOSN provides a feature switch: FeatureGate.

FeatureGate specifies a pair of states (enabled/disabled) for features in MOSN. Each feature has its own default state. Different MOSN versions support different features and default states. FeatureGate describes features and their states in a string in the form of `${feature}=${bool}`. Multiple features and states are separated by a comma.

```bash
// General template
./mosn start -c ${config path} -f ${feature gates description}
// Example
./mosn start -c mosn_config.json -f "auto_config=true,XdsMtlsEnable=true"
```

FeatureGate supports not only feature switching, but also extensible development. During MOSN-based secondary development, you can use the following FeatureGate capabilities:

- Feature switching. You can use FeatureGate to enable/disable a specific feature.
- Feature dependency management. This includes startup sequence and enabled/disabled state dependencies between features.
   - For example, we have two features A and B implemented in MOSN. Initialization of Feature B depends on the initialization result of Feature A. That is to say Feature B depends on Feature A. When Feature A is disabled, Feature B is disabled or downgraded accordingly. FeatureGate provides a simple mechanism to help you focus on feature development, without being distracted by the management of startup items and dependencies.

FeatureGate is mainly intended for secondary development of different features in MOSN.

## FeatureGate-based development

### Implementation of Featuregate

FeatureGate provides the following APIs:

```Go
// Returns the current status of a feature, where "true" indicates "enabled", and "false" indicates "disabled".
func Enabled(key Feature) bool
// Subscribes to a feature and returns its status after subscription finishes.
// Returns whether the subscribed feature is enabled after it is initialized.
// Returns "false" if the subscribed feature is disabled; returns a subscription timeout error if
// the initialization is still not finished after the subscription times out. If the timeout value is less than or equal to 0, no subscription timeout error is returned.
func Subscribe(key Feature, timeout time.Duration) (bool, error)
// Sets the state of FeatureGate. "value" is a complete FeatureGate description.
func Set(value string) error
// Sets the state of FeatureGate. "key" of "map" is the key of the feature, and "value" is the expected state of the feature.
func SetFromMap(m map[string]bool) error
// Registers a new feature with FeatureGate.
func AddFeatureSpec(key Feature, spec FeatureSpec) error
// Sets the state of a feature.
func SetFeatureState(key Feature, enable bool) error
// Enables feature initialization.
func StartInit()
// Waits for the initialization of all features to finish.
func WaitInitFinsh() error
```

StartInit and WaitInitFinsh are called by MOSN and can be ignored during secondary development based on MOSN. Generally, Set and SetFromMap can also be ignored. All the above APIs are exposed by the default non-exportable global FeatureGate object. You can use the default FeatureGate object without creating a new one, unless otherwise required in special cases (for example, when writing unit tests).

The following example shows how FeatureGate is implemented.

```Go
type knownFeatureSpec struct {
        FeatureSpec
        once    sync.Once
        channel chan struct{}
}
type Feature string
type FeatureGate struct {
        // lock guards writes to known, enabled, and reads/writes of closed
        lock  sync.Mutex
        known map[Feature]*knownFeatureSpec
        // inited is set to true when StartInit is called.
        inited bool
        wg     sync.WaitGroup
        once   sync.Once
}
```

FeatureGate consists of the following member variables:
- `map`: records all supported features;
- `inited`: a status indicator that indicates whether FeatureGate has been initialized;
- `once`: used to ensure that FeatureGate is initialized only once;
- `WaitGroup`: used to synchronize the feature initialization result;
- `Mutex`: used for concurrency control.

According to the design of FeatureGate, you can add new features by calling the `Add` method, and modify their states by calling the `Set` method. However, initialization of all features is uniformly implemented by executing the `Init` function. After `Init` is executed, no feature can be added, and no feature state can be modified. That is why we need the `inited` status indicator to record the initialization action.
`knownFeatureSpec` is a non-exportable structure that specifies `FeatureSpec` of different features. Both `once` and `channel` are used for subscription and initialization in FeatureGate, and are not detailed here.
The following describes the definition of `FeatureSpec`, which is the core data structure for FeatureGate-based development.

```Go
type prerelease string
const (
        // Values for PreRelease.
        Alpha = prerelease("ALPHA")
        Beta  = prerelease("BETA")
        GA    = prerelease("")
)
type FeatureSpec interface {
        // Default is the default enablement state for the feature
        Default() bool
        // LockToDefault indicates that the feature is locked to its default and cannot be changed
        LockToDefault() bool
        // SetState sets the enablement state for the feature
        SetState(enable bool)
        // State indicates the feature enablement
        State() bool
        // InitFunc used to init process when StartInit is invoked
        InitFunc()
        // PreRelease indicates the maturity level of the feature
        PreRelease() prerelease
}
```

- `prerelease` refers to a non-exportable type. There are only three conventional exportable variables whose type is prerelease. It is equivalent to the Enum type in conventional languages and is used to describe feature information. The role of prerelease is currently not clear.
- `FeatureSpec` can be implemented with your own code. Generally, you can use `BaseFeatureSpec` provided by the framework or implement a new one based on `BaseFeatureSpec`. As described in the following comments, you generally only need to encapsulate an additional `InitFunc` function to implement this API.

```Go
// BaseFeatureSpec is a basic implementation of FeatureSpec.
// Usually, a feature spec just need an init func.
type BaseFeatureSpec struct {
        // Default state.
        DefaultValue    bool
        // Specifies whether the state can be modified, where "true" indicates that the feature can only remain in the default state.
        // Generally, when it is set to "true" here, the default state is also "true".
        // This feature is mainly used as a "base dependency" for other features.
        IsLockedDefault bool
        PreReleaseValue prerelease
        stateValue      bool  // stateValue shoule be setted by SetState
        inited          int32 // inited cannot be setted
}
```

### Usage

After understanding the basic implementation of FeatureGate, you can use it for basic extended programming. The following describes several usage scenarios of FeatureGate and methods for writing features.

#### 1. Basic global switch

In feature switching, the most basic usage scenario of FeatureGate, FeatureGate works like a global variable. `if` conditional judgement is used to execute different logic here. This allows FeatureGate to integrate all feature switching control parameters into the startup parameters.

```Go
var featureName featuregate.Feature = "simple_feature"
func init() {
    fs := &featuregate.BaseFeatureSpec{
        DefaultValue: true
    }
    featuregate.AddFeatureSpec(featureName,fs)
}

func myfunc() {
    if featuregate.Enable(featureName) {
        dosth()
    } else {
        dosth2()
    }
}
```

#### 2. Initialization operation

The InitFunc function enables related initialization procedures to be uniformly performed when MOSN starts. If a feature is disabled, it will not be executed by InitFunc.

```Go
var featureName featuregate.Feature = "init_feature"

type MyFeature struct {
    *BaseFeatureSpec
}

func (f *MyFeature) InitFunc() {
    doInit()
}
// The rest are similar to the code example provided in 1. Basic global switch.
```

#### 3. Feature dependency management

This is the most important capability of FeatureGate. It easily solves the following problem.

- Assume that we have four independent features: A, B, C, and D.
- B and C depend on A, which means we must enable A before we can enable B and C. D depends on B, which means we must enable B before we can enable D.
- If A is not enabled, B cannot be enabled, but C can still work in downgrade mode.
- The four features can be implemented separately in FeatureGate as follows.

```Go
var FeatureA featuregate.Feature = "A"

func init() {
    fs := &featuregate.BaseFeatureSpec{
        DefaultValue: true
    }
    featuregate.AddFeatureSpec(FeatureA,fs)
}
```

```Go
var FeatureB featuregate.Feature = "B"

type FB struct {
    *BaseFeatureSpec
}

func (f *FB) InitFunc() {
    enabled, err := featuregate.Subscribe(FeatureA, 5 * time.Second)
    if err != nil || !enabled {
        f.SetState(false) // If feature A is not enabled, feature B will not be enabled.
    }
}
```

```Go
var FeatureC  featuregate.Feature = "C"

type FC struct {
    *BaseFeatureSpec
    mode int32
}

func (f *FC) InitFunc() {
    enabled, err := featuregate.Subscribe(FeatureA, 5 * time.Second)
    if err != nil || !enabled {
        f.mode = -1 // Downgrade mode.
        return
    }
    if enabled {
        f.mode = 1 // Normal mode.
    }
}

func (f *FC) MyFunc() {
    if f.mode == 1 {
        dosth()
    } else if f.mode == -1 {
        dosth2()
    }
}
```

```Go
type FeatureD featuregate.Feature = "D"

type FD struct {
    *BaseFeatureSpec
}

func (f *FD) InitFunc() {
    enabled, err := featuregate.Subscribe(FeatureB, 0) // No timeout. Waits until B is enabled.
    if err != nil || !enabled {
        return
    }
    f.Start()
}

func (f *FD) Start() {
}
```

### FAQ

#### Why is FeatureGate used instead of configuration files?

- Unlike configuration files that must be parsed first, FeatureGate is more efficient in implementing advanced features.
- Decision needs to be made on some features earlier than parsing of the configuration files, and may even affect the parsing logic.

