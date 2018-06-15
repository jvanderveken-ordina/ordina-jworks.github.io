---
layout: post
authors: [ken_coenen]
title: 'Accelerator 2018: Microservices deep-dive'
image: /img/accelerator-microservices/microservices-thumb.jpg
tags: [Architecture, Microservices]
category: Microservices
comments: true
---

> From the 13th to the 15th of October 2017, Ordina Belgium JWorks organized the first __Microservices deep-dive Accelerator course__.
The session was presided by Andreas Evers, Solution Expert Faster Time To Value.
A group of **eighteen senior consultants**, with different areas of expertise, were gathered **three days** in Mechelen to learn about all things microservices, from distributed architectures to DDD principles like CQRS and Event Sourcing.

## Table of Contents

1. [Distributed architectures](#distributed-architectures)
2. [Microservices](#microservices)
3. [Domain-Driven Design](#domain-driven-design)
4. [CQRS and Event Sourcing](#cqrs-and-event-sourcing)
4. [Transaction possibilities](#transaction-possibilities)
4. [Service meshes](#service-meshes)
4. [Testing microservices](#testing-microservices)
4. [Consumer-driven contract testing](#consumer-driven-contract-testing)
4. [Gathering and visualizing metrics](#gathering-and-visualizing-metrics)
4. [Using the Spring Framework](#using-the-spring-framework)
10. [Ordina Accelerator 2018](#ordina-ccelerator-2018)

## Distributed architectures

Andreas took us through a brief history of different architectures.
We move from a high degree of Governance with mainframes back in the 1970s, to more Velocity with microservices in the 2010s.

### 1970

Mainframe architecture.
One server, one database, several clients which connect to the same server.

### 1980

Microprocessors, less calculating power, but multiple units, which enabled distributed systems with RPC.
Functions and methods were mapped to network calls.
These components were coupled in a binary way.

### 1990

More networking possibilities.
With networking, you introduce several complexities: slower responses, no responses at all, etc.
Introduction of technologies like eg. EJB 1.x, CORBA, ...

### 2000

Introduction of Service-Oriented Architectures with an Enterprise Service Bus which integrated the different services of the company.
SOAP already uses HTTP, but it is still related to earlier technologies in a way that you create binary coupling by using the generated stub classes provided to you.
With this architecture, you have the problem that everything slows down because one team manages the ESB, and if you want new versions pushed to an environment, the team should approve and deploy the service, a very slow and cumbersome process in most companies.

### 2010

Market disruptors like Tesla, Netflix and Uber want to infiltrate in markets.
Speed is key for these companies.

To achieve this, they came up with the microservices architectures.
They are comparable to classic Service-Oriented Architectures, but the way of implementing them is different.
Sam Newman talks about **Orchestration versus choreography**.
Classic SOA architectures work with an Enterprise Service Bus, which acts as an orchestrator in calling other services.
With microservices, all services work independently of each other and know what they have to do.

Important complexities were identified by these companies:

* Awareness of distributed fallacies. Trying to make a remote call act like a local call is always a bad idea.
* Decoupled architectures
* Data distribution
* Independent teams and independent services

Instead of an Enterprise Service Bus, they use messaging principles to intercommunicate between components using a **Message Bus**.

## Microservices

We started off brainstorming about the different characteristics of microservices:

* Business capability oriented
* Independently deployable
* Dumb pipe
* Technology agnostic

Smart pipes, dumb endpoints = ESB, SOA, where the integration bus is smart.
Dumb pipes, smart endpoints = Microservices

Correct functional decomposition is the key to success with microservices.
Possibilities to decompose a monolithical application:

* Strangler pattern — Develop a Facade to decompose your monolith
* Six sigma business component definition
* Snap analysis
* Use DDD principles to define functional boundaries

## Domain-Driven Design

### Event Storming in practice

Andreas hosted an Event Storming session to detect functional boudaries for a concrete usecase, a timesheet application.

1. Start by brainstorming about the Events in your application
2. Make sure all terminology is according to ubiquitous language (absence, holiday, day transfer/absence transfer, …)
3. Put all events in chronological order
4. Afterwards we think about the Commands which throw these Events
5. Which Actors can we identify?
6. Next, we will identify the Aggregates. Important is to already think about boundaries here… What entities must live within the same context and must be consistent.

<img alt="Event Storming session" src="{{ '/img/accelerator-microservices/banner@0,75x.jpg' | prepend: site.baseurl }}" class="image fit" style="margin:0px auto;">

## CQRS and Event Sourcing

Command changes data. Query only retrieves data.
Mutability and Immutability is more clear.

https://github.com/MicrosoftArchive/cqrs-journey
https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs

Event Sourcing — State is stored as a series of events. Projections are created to query that data.
By storing events, you don’t have the problem of object-relational impedance mismatch, since your event is stored in a whole and not translated to a database representation.

Techniques used by companies:

* 2/24 Versioned Events
* 11/24 Weak schema / Tolerant reader
* 12/24 Upcasters (transformation at runtime)
* 5/24 in-place information (replace data in the past)
* 14/24 copy-transform (read from one event-store, write to another event store with schema changes)

## Transaction possibilities

XA transactions are not the way to go in a microservices world.
Sagas provide a solution for this in an event-sourced architecture.

When processing events, you should keep the following into account:
* Events can arrive in another order
* Idempotent consumers help when something went wrong during processing

## Service meshes

Istio, Linkerd, Opencontrail
Some networking problems can be tackled by the Networking Stack, not within the applications.
Circuit breaker, service discovery, etc. can be encapsulated into a library, eg. Eureka, Hystrix.
Service meshes run these responsibilities as a sidecar next to the application, so that this boilerplate code doesn’t pollute our application.

Components:
* Sidecar which manages circuit breaking
* Control plane

## 12-Factor App

The <a href="https://12factor.net/" target="_blank">12-Factor App</a> are principles you need the make sure your application run smoothly in the cloud.

## Testing microservices

Testing Strategies in a Microservice Architecture
https://martinfowler.com/articles/microservice-testing/

Unit testing
Integration testing
CDCT testing
SLA testing
End to end testing
Penetration testing
Load testing
Canary testing
A/B testing
Chaos monkey testing

Useful tools: Wiremock, Saboteur
Make sure to run Saboteur inside a VM or Docker container, because it will mess with IP tables and the network card configuration.

Fault tolerant microservices on the JVM
https://www.youtube.com/watch?v=dKWNZnuZhd0

## Deploying microservices

One rule when deploying your microservices is that they should be independently deployable.

Monolithical builds should be splitted up into separate builds.
To achieve this, you'll probably want to create different Git repositories too.

The market is moving towards Kubernetes and Openshift.
The latter uses Kubernetes under the hood.
Kubernetes is a platform developed by Google which runs containers in a clusterized environment.

Nodes
Pods
Labels and selectors
Services
Clusters

Deploy in the right timeframe.
When Github is down, they show a unicorn, because this is a very rare occasion.

Spinnaker gives you a dashboard which visualizes all your clusters, cross-cloud provider.
This means that deployments to your Amazon or your Azure servers can all be managed and monitored using one tool.
You need a cloud provider agnostic like Spinnaker to manage your deployments.
Spinnaker also allows you to skip most manual steps during deployment so that your **deployments become safer**.

Cluster locking
Traffic guards
Manual judgements
Conditional pipeline & stage execution
Notifications

## Consumer-driven contract testing

We saw that messaging creates an implicit coupling between microservices.
It is important to describe the dependencies between microservices in the form of contracts.

### Pact vs. Spring Cloud Contract

Andreas gave a thorough comparison between Pact and <a href="https://cloud.spring.io/spring-cloud-contract/" target="_blank">Spring Cloud Contract</a>.

Pact allows a consumer to create a contract in its own repository.
That contract is deployed to a central repository which is picked up by the producer.
With Spring Cloud Contract, the workflow is a bit different.
The consumer creates a contract in the producer's repository.
As a JavaScript developer, cloning a Java repository and making the adaptations himself, can be awkward, as he has to create groovy contracts and stub implementations himself.
So you should carefully choose which path you want to take.

Both can visualize dependencies in a graph.

## Monitoring and alerting

It's important to pay attention to monitoring and alerting so that you're not blind when you have to analyze a problem in a certain environment.

You have tools that check your application logs and metrics for anomalies.
This way you can proactively detect problems in production.
One possible tool you can use is <a href="https://www.coscale.com/" target="_blank">CoScale</a>.
<a href="https://www.elastic.co/products/x-pack">X-Pack</a> is another tool which integrates with <a href="https://www.elastic.co/products/kibana" target="_blank">Kibana</a> and uses artificial intelligence to detect these anomalies.

Distributed tracing allows you to track a request through your microservice landscape.
By knowing where a request spends most time, you can optimize that service.
An opensource tool which can help you with that is Zipkin or Open Tracing, which makes your microservices traceable.

These can be combined with tools like CollectD and StatsD to gather metrics on a server level.

## Gathering and visualizing metrics

### Prometheus

Our blog contains a  <a href="https://ordina-jworks.github.io/monitoring/2016/09/23/Monitoring-with-Prometheus.html" target="_blank">blogpost about Prometheus</a>, written by my colleague Tom Verelst.

### Micrometer

Micrometer is comparable to what SLF4J is for logging.
You get an API for metrics that is independent of any vendor.
You can record values and expose those values with a registry system to the outside world.

Micrometer has extensive support for tools like RabbitMQ and Prometheus.

Our blog contains an interesting <a href="https://ordina-jworks.github.io/microservices/2017/09/17/monitoring-your-microservices-with-micrometer.html" target="_blank">blogpost about Micrometer</a>, written by my colleague Tim Ysewyn, if you want to find out more about Micrometer.

### Dashboards

PromDash
Grafana
Spring Boot Admin
Netflix visualizes its baked images (VMs bundled with the application) by using their opensource tool <a href="http://getvector.io/">Vector</a>.
Microservices Dashboard

## Using the Spring Framework

### Spring Boot

Spring Boot is the de facto standard these to efficiently build Spring applications.
It makes the life of the developer easier by **auto-configuring** your application.
Auto-configuration isn't magic, like some programmers believe.
You can look at the Spring code and build your own auto-configuration classes.

However, auto-configuration is not the only feature of Spring Boot.
It adds **cloudnative features** to your application so that you can build **production-grade microservices**.
An example of one of these features are the Actuator endpoints.

It embeds a servlet container of your choice to be able to run your application.
By default, Tomcat is used as a servlet container, but it's easy to change this to Jetty.

### Spring Cloud

Spring Cloud contains lots of modules which support the development of microservices.
By using Spring Cloud, we automatically have a smarter application context, called **Spring Cloud Context**.
It adds eg. a ``/features`` Actuator endpoint to see which Spring Cloud modules are active.

Kubernetes also solves problems that Spring Cloud tries to solve.
You might wonder how much you still need Spring Cloud when you deploy to Kubernetes.
There is overlap, however, there are Spring Cloud features that add value.

**Spring Cloud Netflix** contains out-of-the-box solutions for most problems we saw earlier, like circuit breakers, load balancing between service instances, service discovery with Eureka, etc.

## Ordina Accelerator 2018

This course was part of the <a href="https://ordina-accelerator.be/programmas/jworks/" target="_blank">Ordina JWorks Accelerator program</a>.
With Accelerator, Ordina offers its employees the necessary tools to develop themselves further.
Not only technical-, but also social- and organizational skills are included in the program.

Medior and Senior experts get the chance to literally __accelerate their career__ by extensively following courses and workshops over a period of two years.

## Links and resources

- Recommended reading: 
<a href="https://www.goodreads.com/book/show/17255186-the-phoenix-project" target="_blank">The Phoenix Project</a> by Gene Kim,  Kevin Behr and George Spafford.
- <a href="https://ordina-accelerator.be/programmas/jworks/" target="_blank">Ordina JWorks Accelerator program</a>


