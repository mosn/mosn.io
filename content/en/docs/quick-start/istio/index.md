---
title: "Use MOSN as an Istio data plane"
linkTitle: "Istio integration"
date: 2020-01-20
weight: 2
description: >
  This topic shows you how to build a Service Mesh development environment with MOSN under the Istio framework, and verify the basic routing and load balancing capabilities of MOSN.
---

{{% pageinfo color="primary" %}}
MOSN succeeded in the Bookinfo test for Istio 1.1.4. For more information about the support from the latest version of Istio, see [Issue #933](https://github.com/mosn/mosn/issues/933). 
`Note: After the MOSN 0.14.0 or later can supports xDS, And tested pass all the sample of bookinfo in istio 1.5.2, here is an [MOSN with Istio](https://katacoda.com/mosn/courses/istio/mosn-with-istio) tutorial.`
{{% /pageinfo %}}

This topic covers the following content:

- Relationship between MOSN and Istio
- Preparations
- Istio deployment with source code
- Bookinfo test

## Relationship between MOSN and Istio

As described in the [MOSN overview](../../overview), MOSN is a service mesh data plane proxy developed in Golang.

The following figure shows the operating principle of MOSN in Istio.

<div align=center><img src="mosn-with-service-mesh.png" width = "450" height = "400" alt="MOSN overview" /></div>

## Preparations

The operating system described in this topic is macOS. You can install the software versions for your OS, too.

### Install hyperkit

Install [docker-for-mac](https://store.docker.com/editions/community/docker-ce-desktop-mac) and then [install drivers](https://github.com/kubernetes/minikube/blob/master/docs/drivers.md).

#### Install docker

Download the software package or run the following command to install it:

```bash
$ brew cask install docker
```

#### Install drivers

```bash
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/docker-machine-driver-hyperkit \
&& chmod +x docker-machine-driver-hyperkit \
&& sudo mv docker-machine-driver-hyperkit /usr/local/bin/ \
&& sudo chown root:wheel /usr/local/bin/docker-machine-driver-hyperkit \
&& sudo chmod u+s /usr/local/bin/docker-machine-driver-hyperkit
```

### Install Minikube (or a commercial Kubernetes cluster)

We recommend that you use Minikube v0.28 or later versions. For more information, go to [https://github.com/kubernetes/minikube](https://github.com/kubernetes/minikube).

```bash
$ brew cask install minikube
```

### Start Minikube

The pilot requires a memory space of at least 2 GB. Therefore, you can allocate resources to Minikube by adding parameters upon startup. If you do not have sufficient resources on your machine, we recommend that you use the commercial Kubernetes cluster.

```bash
$ minikube start --memory=8192 --cpus=4 --kubernetes-version=v1.15.0 --vm-driver=hyperkit
```

Create an Istio namespace

```
$ kubectl create namespace istio-system
```

### Install the kubectl command-line tool

kubectl is a command line interface for executing commands against Kubernetes clusters. For details about the installation of kubectl, go to [https://kubernetes.io/docs/tasks/tools/install-kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl).

```bash
$ brew install kubernetes-cli
```

### Install Helm

Helm is a package management tool of Kubernetes. For details about the installation of Helm, go to [https://docs.helm.sh/using_helm/#installing-helm](https://docs.helm.sh/using_helm/#installing-helm).

```bash
$ brew install kubernetes-helm
```

## Istio deployment with source code

{{% pageinfo color="primary" %}}
SOFAMesh is now directly developed based on Istio to contribute to the Istio community. Development through the fork method is not supported. It will be directly supported by Istio in the future. MOSN succeeded in the Bookinfo test for Istio 1.1.4. For more information about the support from the latest version of Istio, see [Issue #933](https://github.com/mosn/mosn/issues/933).
{{% /pageinfo %}}

### Download the SOFAMesh source code

```bash
$ git clone https://github.com/sofastack/sofa-mesh.git
```

### Install SOFAMesh with Helm

**Run `helm template` to install SOFAMesh.**

Switch to the directory that stores the Istio source code and then install the Istio CRD and other components with Helm.

```bash
$ cd istio
$ helm template install/kubernetes/helm/istio-init --name istio-init --namespace istio-system | kubectl apply -f -
$ helm template install/kubernetes/helm/istio --name istio --namespace istio-system | kubectl apply -f -
```

### Verify the installation

If all pods in the `istio-system` namespace are running, the deployment succeeds.
If you only need to run Bookinfo, running three pods, pilot, injector, and citadel, will suffice your need.

```bash
$ kubectl get pods -n istio-system
NAME                                       READY    STATUS   RESTARTS    AGE
istio-citadel-6579c78cd9-w57lr              1/1     Running   0          5m
istio-egressgateway-7649f76df4-zs8kw        1/1     Running   0          5m
istio-galley-c77876cb6-nhczq                1/1     Running   0          5m
istio-ingressgateway-5c9c8565d9-d972t       1/1     Running   0          5m
istio-pilot-7485f9fb4b-xsvtm                1/1     Running   0          5m
istio-policy-5766bc84b9-p2wfj               1/1     Running   0          5m
istio-sidecar-injector-7f5f586bc7-2sdx6     1/1     Running   0          5m
istio-statsd-prom-bridge-7f44bb5ddb-stcf6   1/1     Running   0          5m
istio-telemetry-55ff8c77f4-q8d8q            1/1     Running   0          5m
prometheus-84bd4b9796-nq8lg                 1/1     Running   0          5m
```

### Uninstall

Uninstall SOFAMesh.

```bash
$ helm template install/kubernetes/helm/istio --name istio --namespace istio-system | kubectl delete -f -
$ kubectl delete namespace istio-system
```

## Bookinfo test

Bookinfo is a book app. It provides four basic services:

- Product Page: The product page is developed in python. It displays all book information and calls the Reviews and Details services.
- Reviews: The review service is developed in java. It displays book reviews and calls the Ratings service.
- Ratings: The rating service is developed in nodejs.
- Details: The book details is developed in ruby.

<div align=center><img src="bookinfo.png" width = "550" height = "400" alt="bookinfo" /></div>

### Deploy Bookinfo and inject MOSN

> For details, go to [https://istio.io/docs/examples/bookinfo/](https://istio.io/docs/examples/bookinfo/).

Inject MOSN

```bash
$ kubectl label namespace default istio-injection=enabled
```

Deploy Bookinfo

```bash
$ kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
```

Verify whether the deployment succeeds

```bash
$ kubectl get services
NAME                       CLUSTER-IP   EXTERNAL-IP   PORT(S)              AGE
details                    10.0.0.31    <none>        9080/TCP             6m
kubernetes                 10.0.0.1     <none>        443/TCP              7d
productpage                10.0.0.120   <none>        9080/TCP             6m
ratings                    10.0.0.15    <none>        9080/TCP             6m
reviews                    10.0.0.170   <none>        9080/TCP             6m
```

Wait until all pods are running

```bash
$ kubectl get pods
NAME                                        READY     STATUS    RESTARTS   AGE
details-v1-1520924117-48z17                 2/2       Running   0          6m
productpage-v1-560495357-jk1lz              2/2       Running   0          6m
ratings-v1-734492171-rnr5l                  2/2       Running   0          6m
reviews-v1-874083890-f0qf0                  2/2       Running   0          6m
reviews-v2-1343845940-b34q5                 2/2       Running   0          6m
reviews-v3-1813607990-8ch52                 2/2       Running   0          6m
```

### Access Bookinfo services

Enable the gateway mode.

```bash
$ kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
$ kubectl get gateway        // Check whether the gateway is running.
NAME               AGE
bookinfo-gateway   24m
```

Set GATEWAY_URL. For details, go to https://istio.io/docs/tasks/traffic-management/ingress/ingress-control/#determining-the-ingress-ip-and-ports.

```bash
$ export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
$ export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].nodePort}')
$ export INGRESS_HOST=$(minikube ip)
$ export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
```

Check whether the gateway takes effect.

```bash
$ curl -o /dev/null -s -w "%{http_code}\n"  http://$GATEWAY_URL/productpage   // 200 indicates a success. 
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

