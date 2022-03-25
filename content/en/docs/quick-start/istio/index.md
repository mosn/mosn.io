---
title: "Use MOSN as an Istio data plane"
linkTitle: "Istio integration"
date: 2022-03-25
weight: 2
description: >
  This topic shows you how to build a Service Mesh development environment with MOSN under the Istio framework, and verify the basic routing and load balancing capabilities of MOSN.
---

{{% pageinfo color="primary" %}}
MOSN v1.0.0 succeeded in the BookInfo test for Istio 1.10.6. For more information about the support from the latest version of Istio, see [MOSN Istio WG](https://github.com/mosn/community/blob/master/wg-istio.md).
{{% /pageinfo %}}

This topic covers the following content:

- Relationship between MOSN and Istio
- Instructions on how to build the Istio proxyv2 image with MOSN
- Istio deployment with MOSN
- Bookinfo test

## Relationship between MOSN and Istio

As described in the [MOSN overview](../../overview), MOSN is a service mesh data plane proxy developed in Golang.

The following figure shows the operating principle of MOSN in Istio.

<div align=center><img src="mosn-with-service-mesh.png" width = "450" height = "400" alt="MOSN overview" /></div>

## Instructions on how to build the Istio proxyv2 image with MOSN

The operating system described in this topic is MacOS, and the Istio version is 1.10.6. If you are using anither OS, there may be some differences.
If only the MOSN code changes, you can also use the MOSN-only update method to build the proxyv2 image.
Usually, you do not need to build a proxyv2 image, use the image provided by us is ok. `docker pull mosnio/proxyv2:v1.0.0`.

Build proxyv2 image with source code. (On MacOS and Istio 1.10.6)
==========
1. Download the istio source code, and checkout to the target version.

```
git clone git@github.com:istio/istio.git
cd istio
git checkout 1.10.6
```

2. Istio will load wasm by default. To simplify, we need to comment out this part of the code. see details in [istio-diff](./istio-diff.md)

3. Build a MOSN binary. You can use the make command provided by MOSN project to build a binary on linux. As the same time, since we are using MacOS, we also need to compile a MacOS binary.

4. Tar the binary compiled by the step 2, the file path should be `usr/local/bin`.

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

5. Upload the `mosn-macos.tar.gz` `mosn-centos.tar.gz` `mosn.tar.gz` to a store serivce that can be accessed, if you do not have one, you can use Go to build a simple service in your local system.

```Go
func main() {
  address := "" // an address can be reached when proxyv2 image build. for example, 0.0.0.0:8080
  filespath := "" // where the .tar.gz files stored.
  http.ListenAndServe(address, http.FileServer(http.Dir(filespath)))
}
```

6. Build proxyv2 images, with some ENV

```bash
address=$1 # your download service address
export ISTIO_ENVOY_VERSION=$2 # MOSN Version, can be any value.
export ISTIO_ENVOY_RELEASE_URL=http://$address/mosn.tar.gz
export ISTIO_ENVOY_CENTOS_RELEASE_URL=http://$address/mosn-centos.tar.gz
export ISTIO_ENVOY_MACOS_RELEASE_URL=http://http://$address/mosn-macos.tar.gz
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

7. Retag the new image, and push it to your docker hub.


Update MOSN binary in the proxyv2 image.
==========

1. Build the MOSN binary

```bash
cd ${MOSN Project Path}
make build # build mosn binary on linux
```

2. Write a DOCKERFILE, and build a new image

```Dockerfile
FROM mosnio/proxyv2:v1.0.0
COPY build/bundles/${MOSN VERSION}/binary/mosn /usr/local/bin/mosn
```

```bash
docker build --no-cache --rm -t ${your image tag}
```

3. Push the new image to your docker hub.

## Istio deployment with MOSN

### Install kubectl

kubectl is the Kubernetes command-line tool, allows you to run commands against Kubernetes clusters. How to install it, see details in [kubectl install](https://kubernetes.io/docs/tasks/tools/install-kubectl).

### Install Kubernetes Platform

How to prepare various Kubernetes platforms before installing Istio, see details in [Platform Setup](https://istio.io/latest/docs/setup/platform-setup/).
We use `minikube` later.

### Install Istio, use MOSN as sidecar

1. Download istio release version, you can find it in [Istio release](https://github.com/istio/istio/releases/tag/1.10.6), or use bash command

```bash
VERSION=1.10.6 # istio version
export ISTIO_VERSION=$VERSION && curl -L https://istio.io/downloadIstio | sh -
```

2. Checkout to the path, add the `istioctl` to `PATH` environment variable.

```bash
cd istio-$ISTIO_VERSION/
export PATH=$PATH:$(pwd)/bin
```

3. create istio namespace, set MOSN proxyv2 image as sidecar image

```bash
kubectl create namespace istio-system
istioctl manifest apply --set .values.global.proxy.image=${MOSN IMAGE} --set meshConfig.defaultConfig.binaryPath="/usr/local/bin/mosn"
```
4. Verify the installation

```bash
kubectl get pod -n istio-system

NAME                                    READY   STATUS    RESTARTS   AGE
istio-ingressgateway-6b7fb88874-rgmrj   1/1     Running   0          102s
istiod-65c9767c55-vjppv                 1/1     Running   0          109s
```

If all pods in the `istio-system` namespace are running, the deployment succeeds.

## BookInfo test

MOSN v1.0.0 succeeded in the BookInfo test for Istio 1.10.6. You can alsoe use [MOSN with Istio](https://katacoda.com/mosn/courses/istio/mosn-with-istio) to run the BookInfo test.
More test cases see Istio [Example]((https://istio.io/latest/docs/examples).
If you have any questions, please contact us  [issue](https://github.com/mosn/mosn/issues), code contributions are welcome too.

### BookInfo introduction

The application displays information about a book, similar to a single catalog entry of an online book store. Displayed on the page is a description of the book, book details (ISBN, number of pages, and so on), and a few book reviews.
The Bookinfo application is broken into four separate microservices:
- productpage. The productpage microservice calls the details and reviews microservices to populate the page.
- details. The details microservice contains book information.
- reviews. The reviews microservice contains book reviews. It also calls the ratings microservice.
- ratings. The ratings microservice contains book ranking information that accompanies a book review.

<div align=center><img src="bookinfo.png" width = "550" height = "400" alt="bookinfo" /></div>

#### Deploy Bookinfo and inject MOSN

> For details, go to [https://istio.io/docs/examples/bookinfo/](https://istio.io/docs/examples/bookinfo/).

Use kube-inject way to inject MOSN

```bash
istioctl kube-inject -f samples/bookinfo/platform/kube/bookinfo.yaml > bookinfo.yaml
# sed -i '' is the MacOS command, if you are in linux, use sed -i instead.
sed -i '' "s/\/usr\/local\/bin\/envoy/\/usr\/local\/bin\/mosn/g" ./bookinfo.yaml
```

Deploy BookInfo app:

```bash
$ kubectl apply -f bookinfo.yaml
```

Verify whether the deployment succeeds

```bash
$ kubectl get services
NAME          TYPE        CLUSTER-IP        EXTERNAL-IP   PORT(S)    AGE
details       ClusterIP   192.168.248.118   <none>        9080/TCP   5m7s
kubernetes    ClusterIP   192.168.0.1       <none>        443/TCP    25h
productpage   ClusterIP   192.168.204.255   <none>        9080/TCP   5m6s
ratings       ClusterIP   192.168.227.164   <none>        9080/TCP   5m7s
reviews       ClusterIP   192.168.181.16    <none>        9080/TCP   5m6s
```
Wait until all pods are running

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

Check BookInfo is working properly

```bash
kubectl exec -it $(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}') -c ratings -- curl productpage:9080/productpage | grep -o "<title>.*</title>"
```

#### Access Bookinfo services

Enable the gateway mode.

```bash
$ kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
$ kubectl get gateway
NAME               AGE
bookinfo-gateway   6s
```
Set `GATEWAY_URL`. For details, go to [doc](https://istio.io/docs/tasks/traffic-management/ingress/ingress-control/#determining-the-ingress-ip-and-ports)

```bash
$ export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
$ export INGRESS_HOST=$(minikube ip)
$ export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
```

Check whether the gateway takes effect.

```bash
$ curl -o /dev/null -s -w "%{http_code}\n"  http://$GATEWAY_URL/productpage
200
```

**View the page**

Go to `http://$GATEWAY_URL/productpage` (replace $GATEWAY_URL with your address). The Bookinfo page is shown as follows after you refresh it. Three versions of Book Reviews are available.
The three versions will be displayed in turn when you refresh the page. For more information, go to samples/bookinfo/platform/kube/bookinfo.yaml.

Page v1

![v1](v1.png)

Page v2

![v2](v2.png)

Page v3

![v3](v3.png)

### Verify routing by version in MOSN

Create a series of destination rules for Bookinfo services.

```bash
$ kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml
```

Specify access to only v1 of the Book Reviews service.

```bash
$ kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml
```

Go to `http://$GATEWAY_URL/productpage`. You will find that book reviews are now fixed to be displayed on page v1.

![v1](v1.png)

### Verify routing by weight in MOSN

Allocate 50% of the traffic to v1 and v3 each.

```bash
$ kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-50-v3.yaml
```

Go to `http://$GATEWAY_URL/productpage`. You will find that page v1 and page v3 are displayed with a possibility of 50% each.

### Verify routing by specific header in MOSN

The Bookinfo page provides a login entry in the upper-right corner. After you log in, your request will carry the end-user custom field, the value of which is the user name. MOSN supports routing by the value of this header. For example, route user Jason to v2 and others to v1. (The username and password are both jason. For the reason of using this user account, check the corresponding YAML file).

```bash
$ kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml
```

When you go to `http://$GATEWAY_URL/productpage`:

Page v2 will be displayed if you log in as Jason.

![Login](login.png)

Page v1 will be displayed if you log in as other users.

![v1](v1.png)


### Uninstall BookInfo


```bash
$ sh samples/bookinfo/platform/kube/cleanup.sh
```

Make sure BookInfo is uninstalled

```bash
$ kubectl get virtualservices   #-- there should be no virtual services
$ kubectl get destinationrules  #-- there should be no destination rules
$ kubectl get gateway           #-- there should be no gateway
$ kubectl get pods              #-- the Bookinfo pods should be deleted
```

## Uninstall Istio

```bash
$ istioctl manifest generate | kubectl delete -f -
```

Make sure Istio is uninstalled

```bash
$ kubectl get namespace istio-system
```
