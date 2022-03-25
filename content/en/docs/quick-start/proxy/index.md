---
title: "Quick start"
linkTitle: "Quick start"
weight: 1
date: 2022-03-25
description: >
  This topic shows you how to quickly set up the development environment for building, testing, packaging, and running the sample code.
---

This topic is intended to help developers who are new to MOSN projects quickly set up a development environment for building, testing, packaging, and running the sample code.


## Prepare the running environment

+ If you want to run MOSN with docker, [install docker](https://docs.docker.com/install/) first.
+ If you are using a local machine, set up a Unix-like environment.
+ Install the compilation environment of Go.

## Obtain the code

Code for MOSN projects is hosted at [Github](https://github.com/mosn/mosn). Run the following command to obtain the code.

```bash
go get -u mosn.io/mosn
```

If the go get command fails, manually create a project.

```bash
# Access the src directory under GOPATH.
cd $GOPATH/src
# Create the mosn.io directory.
mkdir -p mosn.io
cd mosn.io

# Clone MOSN code.
git clone git@github.com:mosn/mosn.git
cd mosn
```

The MOSN source code is finally saved to `$GOPATH/src/mosn.io/mosn.`

## Import the MOSN source code into the IDE

Import `$GOPATH/src/mosn.io/mosn` into your preferred Go IDE (Goland is recommended).

## Compile the code

In the project's root directory, compile the binary file of MOSN by running the following commands, depending on your machine type and the environment in which you want to execute the binary file.

### Istio version switch

MOSN support xDS v2 and xDS v3, which are represented by Istio 1.5.2 and Istio 1.10.6 respectively. It can be switched between different versions according to needs.
The default version is 1.10.6.

Switch to Istio 1.5.2 (xDS v2)

```bash
make istio-1.5.2
```

Switch to Istio 1.10.6 (xDS v3)

```bash
make istio-1.10.6
```

### Compilation with a docker image

```bash
make build // Compile a Linux 64-bit executable binary file.
```

### Local compilation

Run the following command to compile a local executable binary file.

```bash
make build-local
```

Find the compiled binary file in `build/bundles/${version}/binary`.

## Run tests

Run the following command in the project's root directory to start the unit test.

```bash
make unit-test
```

Run the following command in the project's root directory to start the integration test (which takes some time).

```bash
make integrate
make integrate-new
```

## Start MOSN from the configuration file

Run the following command to start MOSN from the configuration file.

```bash
./mosn start -c '$CONFIG_FILE'
```

## Run MOSN samples

Refer to the example projects in the `examples` directory to [run samples](../../samples).

## Build the Service Mesh platform with MOSN

For more information, see the [Istio integration](../istio).

