---
title: "Introducing Service Mesh"
chapter: true
weight: 2
---
# Introducing Service Mesh


## Target microservices platform

As mentioned earlier, building distributed and disparate microservices involves a set of microservices. The following diagram depicts the target microservice platform within the ultimate target that Services team will be focusing on building and delivering business services and they will be relying on the underlying platform for the technical requirements.


![Microservices Building Blocks](/images/microservices-building-blocks-map.png  "Microservices Building Blocks")

## Code oriented pattern

 First attempts to resolve such concerns were concentrated around developing libraries that will provide these  capabilites. The concept is based on embedding libraries to the service base code:

![Code oriented frameworks](/images/code-oriented-frameworks.png  "Code oriented frameworks")

[Netflix OSS](https://netflix.github.io) and then [Spring Cloud](https://spring.io/projects/spring-cloud) were the most popular and widely adopted frameworks for the Java Ecosystem.

Limits of the oriented pattern:

- Language oriented: We need almost a library per each programming language.
- Error prone (implementation).
- Hard to upgrade each microservice when system grow.
- Add technical challenges and duties to development teams.
- Different teams in the same organization may have different implementations.
- Each team should maintain his implementation.

{{% notice note  %}}
  Microservices challenges need to be solved **uniformly**.
{{% /notice%}}    

### Desired state

- We want to keep microservice concerns **separate** from the business logic.

- The network should be **transparent** to applications.

- Developers should focus on delivering business capabilities and not implementing microservices common concerns.

- Microservices interconnection should be **language agnostic**.

- Microservices concerns  should be **easy to upgrade** solution.

### Sidecar pattern

This pattern enables applications to be composed of heterogeneous components and technologies. This pattern is named Sidecar because it resembles a sidecar attached to a motorcycle. In the pattern, the sidecar is attached to a parent application and provides supporting features for the application. Thus, all microservices concerns will be handled by the sidecar in homogenous way and injected on the fly by platform when deploying the application.

![Sidecar oriented pattern](/images/sidecar-oriented-pattern.png  "Sidecar oriented pattern")


### Definition of a Service mesh

A service mesh is a dedicated infrastructure layer for handling service-to-service communication. It’s responsible for the reliable delivery of requests through the complex topology of services that comprise a modern, cloud native application.(buoyant.io)


{{% notice tip  %}}
  Microservices challenges need to be solved **uniformly**.
{{% /notice%}}    


{{% notice info  %}}
**Benefits**: Adopting service mesh increases delivery speed and reliability, improve security and governance of delivered services.
{{% /notice%}}  


![Microservices Building Blocks with Istio](/images/microservices-building-blocks-istio.png  "Microservices Building Blocks with Istio")
