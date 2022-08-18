---
title: "MOSN 与 Istio 的 proxyv2 镜像 build 方法"
linkTitle: "MOSN 与 Istio 的 proxyv2 镜像 build 方法"
date: 2022-03-25
aliases: "/zh/docs/istio"
weight: 3
description : >
    本文主要讲述 MOSN 与 Istio 的 proxyv2 镜像 build 方法，并且如何仅更新 MOSN 
---

本文的完整构建镜像方法均是基于 MacOS 和 Istio 1.10.6 版本进行的构建，在其他操作系统 Istio 版本上可能存在部分细节差异，需要进行调整。
除了完整构建方式外，如果仅有 MOSN 代码发生变化，还可以使用 单独更新 MOSN 版本 的方式构建镜像。

**通常情况下，您不需要额外构建镜像，可直接用我们提供的镜像 `mosnio/proxyv2:${MOSN-VERSION}-${ISTIO_VERSION}`，如`docker pull mosnio/proxyv2:v1.0.0-1.10.6`**

完整的镜像构建（基于 MacOS 和 Istio 1.10.6）
==========

1、下载完整的 istio 源代码，并且切换到对应的版本

```bash
git clone git@github.com:istio/istio.git
cd istio
git checkout 1.10.6
```

2、由于目前 Istio 默认会加载 wasm，我们需要将相关逻辑注释掉，再重新编译镜像，避免一些不必要的错误。详细的改动可见 [istio-diff](../istio/istio-diff)

3、编译 MOSN 二进制，MOSN 提供了镜像编译的方式可直接编译 linux 的二进制；同时由于在 MacOS 上构建的过程中，Istio 还会下载一个 MacOS 版本，因此还需要编译一个 MacOS 的二进制

4、将编译好的二进制，使用 tar 方式进行打包，并且打包路径需要是 `usr/local/bin`

```bash
cd ${MOSN Project Path}
mkdir -p usr/local/bin
make build # build mosn binary on linux
cp build/bundles/${MOSN VERSION}/binary/mosn usr/local/bin
tar -zcvf mosn.tar.gz usr/local/bin/mosn
cp mosn.tar.gz mosn-centos.tar.gz # copy a renamed tar.gz file

make build-local # build mosn binary on macos
cp build/bundles/${MOSN VERSION}/binary/mosn usr/local/bin
tar -zcvf mosn-macos.tar.gz usr/local/bin/mosn
```

5、将生成的`mosn-macos.tar.gz` `mosn-centos.tar.gz` `mosn.tar.gz` 上传到一个编译环境可访问的存储服务中，可用 Go 语言简单快速在本地环境搭建一个

```Go
func main() {
address := ""   // an address can be reached when proxyv2 image build. for example, 0.0.0.0:8080
filespath := "" // where the .tar.gz files stored.
http.ListenAndServe(address, http.FileServer(http.Dir(filespath)))
}
```

6、指定参数，开始编译 proxyv2 镜像

```bash
address=$1 # your download service address
export ISTIO_ENVOY_VERSION=$2 # MOSN Version, can be any value.
export ISTIO_ENVOY_RELEASE_URL=http://$address/mosn.tar.gz
export ISTIO_ENVOY_CENTOS_RELEASE_URL=http://$address/mosn-centos.tar.gz
export ISTIO_ENVOY_MACOS_RELEASE_URL=http://$address/mosn-macos.tar.gz
export ISTIO_ENVOY_MACOS_RELEASE_NAME=mosn-$2 # can be any value
export SIDECAR=mosn

make clean # clean the cache
make docker.proxyv2 \
 SIDECAR=$SIDECAR \
 ISTIO_ENVOY_VERSION=$ISTIO_ENVOY_VERSION \
 ISTIO_ENVOY_RELEASE_URL=$ISTIO_ENVOY_RELEASE_URL \
 ISTIO_ENVOY_CENTOS_RELEASE_URL=$ISTIO_ENVOY_CENTOS_RELEASE_URL \
 ISTIO_ENVOY_MACOS_RELEASE_URL=$ISTIO_ENVOY_MACOS_RELEASE_URL \
 ISTIO_ENVOY_MACOS_RELEASE_NAME=$ISTIO_ENVOY_MACOS_RELEASE_NAME
```

7、编译完成以后，可以将镜像打上新的 Tag 并且上传（如个人测试 dockerhub 的地址），确保 istio 使用时可访问即可


单独更新 MOSN 版本
==========


1、重新编译 MOSN 二进制

```bash
cd ${MOSN Project Path}
make build # build mosn binary on linux
```

2、直接基于现有 MOSN 的 proxyv2 镜像更新二进制

```Dockerfile
FROM mosnio/proxyv2:v1.0.0-1.10.6
COPY build/bundles/${MOSN VERSION}/binary/mosn /usr/local/bin/mosn
```

```bash
docker build --no-cache --rm -t ${your image tag}
```

3、将新镜像上传，确保 istio 使用时可访问即可

```bash
istioctl manifest apply --set .values.global.proxy.image=${MOSN IMAGE} --set meshConfig.defaultConfig.binaryPath="/usr/local/bin/mosn"
```
