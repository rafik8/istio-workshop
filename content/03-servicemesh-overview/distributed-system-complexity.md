---
title: "Distributed is hard"
chapter: true
weight: 3
draft: true
---

Companies with large, monolithic applications are increasingly breaking these unwieldy apps into smaller, containerized microservices. Microservices are popular because they offer agility, speed, and flexibility, but they can be complex, which can be a hurdle for adoption. And having multiple microservices, rather than a single monolith, can increase the attack surface of the app.


# Distributed is hard
A realistic application will have multiple components packaged across multiple containers, with multiple instances of those components deployed across multiple machines. That’s a lot of multiplication, and in a fast-moving environment it can quickly become hard to manage and keep track of everything. How do component instances find each other? How does network traffic get routed between containers? Was my recent deployment successful? Did our latest roll out affect end users? How does a Dev and DevOps team understand all of the application interactions that are happening?


# Target:
With Istio, it’s easier to observe what is happening across an entire network of microservices, secure communication between services, and ensure that policies are enforced.
