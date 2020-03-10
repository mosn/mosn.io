---
title: "Sidecar pattern"
date: 2019-01-20
linkTitle: "Sidecar pattern"
weight: 2
description: >
  The sidecar pattern is commonly used in service mesh.
---

The sidecar pattern is commonly used in service mesh. It is a container design pattern that came into being earlier than service mesh did. This topic will help you have a general idea about the sidecar pattern.

## What is the sidecar pattern?

In the **sidecar pattern**, features of applications are separated as processes. As shown in the following figure, the sidecar pattern allows you to add more features alongside every application container, without configuring third-party components or modifying the application code.

![Sidecar pattern](sidecar-pattern.jpg)

Similar to a sidecar mounted to a three-wheeled motorcycle, in a software architecture, a sidecar is connected to a primary application and adds extension or advanced features to it. Sidecar applications and primary applications are loosely coupled. This pattern can cover the difference between programming languages, and unify microservice features such as observability, monitoring, logging, configuration, and circuit breaker.

## Benefits

Benefits of using the sidecar pattern are as follows:

- Moves features unrelated to the application business logic into the common infrastructure. This reduces code complexity of microservices.

- Avoids repeatedly writing configuration files and code for third-party components. This reduces duplicated code in the microservice architecture.

- Loosens the coupling between the application code and the underlying platform.

## How does the sidecar pattern work?

Sidecar is a container widely used in service mesh. For details, see [Service Mesh Architectures](https://aspenmesh.io/service-mesh-architectures/). That blog describes the **node agent** and **sidecar** service mesh architectures in detail.

You can deploy a sidecar service mesh without having to run a new agent on every node, but you will be running multiple copies of an identical sidecar. In sidecar deployments, you have one adjacent container deployed for every application container. This container is a sidecar. The sidecar handles all the network traffic in and out of the application container. In a Kubernetes pod, a sidecar container is deployed alongside the original application container. These two containers share resources such as storage and network. If we take the pod that runs the sidecar container and the application container as a host, these two containers share all host resources.

