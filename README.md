# AkkaPowerFramework

AkkaPowerFramework is a micro service framework that extense the C# .net port of the Akka Framework [Akka.net](https://getakka.net) to build highly concurrent, distributed, fault tolerant and event-driven application of micro services.

Some of the useful services for micro service application like authenticating nodes and services, service discovering, persistance solutions, central configurations handling and handling of redundancy and fail over management are implemented but can also easily be extended and/or replaced by custom solutions.

The framework strives to deliver these basic micro service needs as easy and hassle free as it can with best practices in mind by using the cluster capabilities of [Akka.net](https://getakka.net). Our goal is that the developer of an ````AkkaPowerApplication```` has only to concern himself with writing ````AkkaPowerService````s with Actor of Akka and the rest is handled by the framework. An intervention or customization of how to build, deploy or run a micro service application is possible in situation where there is a special need for.

We also have the goal to make the transition of single a node Akka.net application to an ````AkkaPowerApplication```` as easy and hassle free as it possibly makes sense.

## Builds

| Part/Service/Build | Badge |
| ------------------ | ----- |
| ManagedNode        |       |

### ManagedNode deployments

![https://vsrm.dev.azure.com/Stelzi79/_apis/public/Release/badge/318a0b9d-3d9a-4175-8e0a-ccc7bf37d064/1/2](https://vsrm.dev.azure.com/Stelzi79/_apis/public/Release/badge/318a0b9d-3d9a-4175-8e0a-ccc7bf37d064/1/2)

## Architecture

Every ````AkkaPowerApplication```` is build by independant ````AkkaPowerService````s which are deployed on one or more ````ManagedNode````s. ````ManagedNode````s deploy ````AkkaPowerService````s as configured by the ````ApplicationAuthority```` in there own Docker containers or on bare metal environments if there is no Docker support available or useful on this particular node. ````AkkaPowerService````s can be grouped by ````SharedServiceLocation````s to have them physically near each other in the same container for performance reasons.

## Provided Framework Services

The following framework Services are currently implemented and can easily be extended and/or replaced by custom solutions if there is a need for.

### Authenticating Nodes and Services

````ApplicationAuthority````: This service is the defines the Application identity and delegates some basic capability to the providing services of the application. It is the central authority of the application that composes everything.

### Service Discovering

### Persistance Solutions

### Central Configurations Handling

## Handling of Redundancy and Fail Over Management
