# Architecture

Every ```AkkaPowerApplication``` is build by independant ```AkkaPowerService```s which are deployed on one or more ```ManagedNode```s. ```ManagedNode```s deploy ```AkkaPowerService```s as configured by the ```ApplicationAuthority``` in there own Docker containers or on bare metal environments if there is no Docker support available or useful on this particular node. ```AkkaPowerService```s can be grouped by ```ServiceLocation```s to have them physically near each other in the same container for performance reasons.

## ManagedNode

This is the smallest physical infrastructure unit of the framework. There are two different kind of ManagedNodes.

1. **DockerHost**: The most common and recommended type of node is the Docker host node. This Node is configures and runs Docker containers that contain ServiceLocations in them.

2. **BareMetal**: If a Node doesn't possess the capability to be a Docker host or if there is the need for an EdgeUI like WPF, WinForms or any other app that lacks vital capabilities inside a Docker container then you can use this ```ManagedNode``` type to configure and run ServiceLocations.

All Nodes acquire the necessary information on how to configure, service and run the ServiceLocations from the ```ApplicationConfig``` service.

## AkkaPowerApplication

A created application as a whole with all its  is called an ```AkkaPowerApplication```.

## AkkaPowerService

This is the smallest logical unit of the framework. It is comprised from one or more Akka.net PowerActors that provide something useful for the application and other AkkaPowerServices to consume.

The AkkaPowerService has to properly supervise any of it PowerActors it defines and must notify the ServiceLocation if it is not able to continue work.

## ServiceLocation

A ServiceLocation has one or more AkkaPowerServices in it. This can group different AkkaPowerServices together to conserve resources or have them logically near each other. ServiceLocation can be completely configured as need be. It is advised in production environments to not have every AkkaPowerService in one singular ServiceLocation. As a default there is one ```FrameworkServices``` and one ```ApplicationServices``` ServiceLocation setup when creating a new AkkaPowerApplication. Redundancy and fail over capabilities are enabled by duplicating ServiceLocations.

## ApplicationAuthority

It basically bootstraps the whole application and discovers for a ServiceLocation some basic framework services it needs to function properly. Every ServiceLocation only needs the location where it can find the ApplicationAuthority, for it to discover all for its AkkaPowerServices need to function properly.

There can be and for redundancy in production environments should be more than one ApplicationAuthority in an AkkaPowerApplication.

## Configuration Handling

If a PowerActor from AkkaPowerService needs access to or wants to define a configuration value it requests a subscription to it from the application wide ```ApplicationConfig``` services. A configuration value has by default restrictions to its read or write access to such an access must be granted at initially enabling the AkkaPowerService that wants to access such a configuration value.

Any AkkaPowerService can define configuration values. By default any defined configuration is protected from any other AkkaPowerService than the defining one to read or write. A defined configuration value can only be mutated by the defining AkkaPowerService. The defining AkkaPowerService can explicitly define that the configuration value can be requested by other AkkaPowerServices. It also can enable a built in way to get requests for changes of a configuration value it defines. At every possible mutation of the configuration value the defining AkkaPowerService must make sure that in only contains valid values. Any mutation to a configuration value is broadcast to any AkkaPowerService that consumes that configuration value.

A discovered configuration value is cached on a ServiceLocation basis. By default any AkkaPowerService is notified by the ServiceLocation when it receives a such a notification of mutation from the ```ApplicationConfig``` service.

## Service Discovery

By default defined PowerActors in a AkkaPowerService only can handle messages in there own service. An AkkaPowerService has to explicitly define which messages it is willing to handle from other AkkaPowerServices and PowerActors. The AkkaPowerService must forward such explicitly defined public messages to its appropriate PowerActors.

Any AkkaPowerService that wants a message handled by some AkkaPowerService outside its service boundary has to ask the ```ServiceDiscovery``` service for a subscription of AkkaPowerServices that are willing to handle that message. Any changes to which AkkaPowerService is willing to handle a certain message right now is similarly broadcast to any possible consumer AkkaPowerService as configuration is handled.

Every discovered AkkaPowerService that is willing to handle certain messages for other services is cached on a ServiceLocation basis. By default and AkkaPowerService is notified by the ServiceLocation when it receives such notification of changes to which AkkaPowerService is willing to handle that particular message right now.

## Application and Service Lifecycle events
