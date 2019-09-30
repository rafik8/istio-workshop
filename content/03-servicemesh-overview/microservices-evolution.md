---
title: "The evolution of microservices"
chapter: true
weight: 1
tags: ["microservices", "basics"]
---

## The evolution of microservices

Companies with large, monolithic applications are increasingly breaking these unwieldy apps into smaller, containerized microservices.
Microservices are popular because they offer **agility**, speed, and **flexibility**, but they can be complex, which can be a hurdle for adoption.

In recent years evolving software development practices have fundamentally changed applications. These changes have impacted the requirements to the underlying infrastructure, tools, and processes to manage applications properly throughout the lifecycle.
Applications transformed from large monolithic code bases to collections of many small services, loosely coupled together into what is called a microservices architecture. Driven by the desire to ship more software, faster in a way that is portable across infrastructure, the resulting benefits of more agile organizations.

Such microservices architectures provide agility to development teams. But, it also introduces **operational complexity**. Both Dev and Ops teams often struggle during the migration to microservices. When a monolith is broken into tens or even hundreds of microservices, how do these components discover and communicate with each other? Where to begin debugging efforts? What is the service dependency graph? Who is calling whom? These are questions that they did not have to answer in the monolithic world. In a microservices world where a microservices architecture may seem like a distributed service mess, they must have these answers and more.

![Microservices evolution](/images/microservices-evolution.png)


## Microservices challenges

Adoting microservices oriented architecture bring new challenges:

- N to N communications.
- Distributed software interconnection and troubleshooting is hard.
- Containers should stay thin and platform agnostic.
- Upgrade of polyglot microservices is hard at scale.


![Microservices evolution](/images/microservices-building-blocks.png)



The only value visible from business stackhelder is the business service so we have a develop a bench of microservices concerns to deliver a business value.

![Microservices evolution](/images/microservices-iceberg.png)



<!-- ## The need of a service Mesh

## Service mesh basics
Service mesh solutions have two distinct components that behave somewhat differently: 1) a data plane, and 2) a control plane. The following diagram illustrates the basic architecture.

The data plane is composed of a set of intelligent proxies (Envoy) deployed as sidecars. These proxies mediate and control all network communication between microservices along with Mixer, a general-purpose policy and telemetry hub.

The control plane manages and configures the proxies to route traffic. Additionally, the control plane configures Mixers to enforce policies and collect telemetry. -->