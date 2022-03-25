---
title: "Quick start"
linkTitle: "Quick start"
weight: 1
date: 2020-01-20
description: >
  This topic shows you how to quickly set up the development environment for building, testing, packaging, and running the sample code.
---

This topic is intended to help developers who are new to MOSN projects quickly set up a development environment for building, testing, packaging, and running the sample code.


## Prepare the running environment

+ If you want to run MOSN with docker, [install docker](https://docs.docker.com/install/) first.
+ If you are using a local machine, set up a Unix-like environment.
+ Install the compilation environment of Go.
+ Install dep. For details, see the [official installation document of deb](https://golang.github.io/dep/docs/installation.html).

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

### Compilation with a docker image

```bash
make build // Compile a Linux 64-bit executable binary file.
```

### Local compilation

Run the following command to compile a local executable binary file.

```bash
make build-local
```

Cross-compile a Linux 64-bit executable binary file on a non-Linux machine.

```bash
make build-linux64
```

Cross-compile a Linux 32-bit executable binary file on a non-Linux machine.

```bash
make build-linux32
```

Find the compiled binary file in `build/bundles/${version}/binary`.

## Create an RPM package

Run the following command in the project's root directory to create an RPM package.

```bash
make rpm
```

Find the package in `build/bundles/${version}/rpm`.

## Create a docker image

Run the following command to create a docker image.

```bash
make image
```

## Run tests

Run the following command in the project's root directory to start the unit test.

```bash
make unit-test
```

Run the following command in the project's root directory to start the integration test (which takes some time).

```bash
make integrate
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

