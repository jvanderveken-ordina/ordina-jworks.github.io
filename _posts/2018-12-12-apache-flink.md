---
layout: post
authors: [hans_vanbellingen]
title: 'Apache Flink introduction'
image: /img/apacheflink/apacheflink.jpg
tags: [Flink, Stream]
category: Flink
comments: true
---

# Apache FLink Stream - A practical introduction

##  The Goal

Since my previous post about kafka, I was interested to see how the kafka streaming api compares to other streaming frameworks.
This post is what I discovered when modifying the example of that post into a flink streaming application.

The goal for me was multiple, as well finding out how Flink works for a very simple use case, as comparing both solutions for this use case.
This does not include making the Flink a real clustered production ready application.

## Apache Flink

### The differences with kafka streams

This is no way supposed to be a complete set of what is different between Kafka and Flink, but more a list of the major differences encountered when adapting the small sample application.

- The biggest difference is that Kafka stream applications are part of the normal application used by importing a library 
and Flink is a seperate (clustered) environment on which the stream applications are deployed.
- Kafka streams only works with kafka, and Flink integrates with a lot of different systems.
- Kafka streams is only stream processing, Flink can do both streams and batch processing.

This means that at first glance kafka streams seems simpler to start with, but also less flexible then Flink. But that is something that we'll discover when trying to implement the example application.

## The setup

Since Flink is a seperate cluster application, we need to install the Flink runtime.
This is very easy and well explained on the quickstart page https://ci.apache.org/projects/flink/flink-docs-release-1.6/quickstart/setup_quickstart.html

## The application

### design
Since Flink has a lot of possibilities for input and output connectors TODO

### setup of the project
Luckely the Flink project has a maven archetype so starting a new project is as simple as executing this: 
```
$ mvn archetype:generate                               \
      -DarchetypeGroupId=org.apache.flink              \
      -DarchetypeArtifactId=flink-quickstart-java      \
      -DarchetypeVersion=1.6.1
```
Since I'm trying to create a streaming example the first thing to do is delete the auto generated BatchJob

### dooing something usefull
Finally we are at the stage that something usefull can be done. Up to the next tutorial to get some inspiration on how to translate the Kafka example into a Flink example.

