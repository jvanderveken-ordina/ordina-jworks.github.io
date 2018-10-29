---
layout: post
authors: [talha-demirci, michael_dewree, sander_smets, frederick_bousson, axel_bergmans]
title: "Liquor Ticker"
image: /img/2018-10-26-Lead-Dev-London-2018/lead-dev.png
tags: [Angular, NodeJS, NestJS, SAP Cloud, Openshift]
category: IoT
comments: true
---

# Liquor Ticker

Liquor Ticker is a new awesome Alcohol Sensor that reveals your alcohol level beautifully on your screen.
Just blow on the sensor and you'll see the beer flowing, the more you drank, the more beer flows.

The app indicates whether you are safe to drive or not by showing your alcohol level, a green or red color and multiple categories: Safe, Alarm and Positive.
- Safe meaning you are safe to drive, you barely drank any alcohol.
- Alarm means you aren't allowed to drive anymore, if you do you will lose your driver licence at least for 3 hours.
- Positive means your alcohol level is higher than 0.8 promille and you should definitely not get behind the wheel anymore.

## Back End
As back end we made a simple NodeJS application with NestJS framework on top of it.

Our NestJS app receives a HTTP package which contains the alcohol level. 
The beer level will be calculated based on the promille and send to the front end together with the alcohol level. 

To communicate with the front end, we make use of the NestJS websocket library.
We first create a 'WebSocketGateway' which is responsible for putting the calculated data onto the websocket.

```javascript
@WebSocketGateway(8000, { namespace: 'socket' })
export class BoozeLevelGateway {

    constructor() { }

    @WebSocketServer()
    private server: any;

    emit(event, data) {
        this.server.emit(event, data);
    }
}
```

This gateway gets called in the Service where it emits the data onto the websocket.

```javascript
this._gateway.emit('app-event', JSON.stringify(level));
```

## Front End
The front end exists of an Angular app.
This Angular app was originally part of the NodeSimpleServer from [this blog post](/iot/2017/01/21/Node-with-TypeScript.html).

The front end listens to the websocket and when it receives data the view gets rendered based upon this data.

```javascript
this.socket.connect();
this.socket.on('app-event', (message) => {
    ... // View gets rendered.
}
```


## Deployment
We deployed Liquor Ticker on two cloud platforms: SAP Cloud Foundry and Openshift. 
The reason why we deployed it on different platforms, is to see the time differences of those two platforms when we push data.

### Openshift
Both our back end and front end projects are deployed as seperate applications on JWorks Openshift.
The two applications have a lot in common in the way they are set up because they are both NodeJS apps.

#### Jenkins
Both applications use a Jenkins pipeline to enable a CI/CD process with GitHub.

We used it to set up both the back end as front end to be able to easily redeploy both projects. 
This makes it effortless to add multiple building stages.

To add Jenkins to our projects we had to create a new SSH key to link the GitHub repository to each application. 
It is very straightforward to add a new private key in Openshift and to add its public key on GitHub.

#### Routing
They key difference between both projects is that the back end works with a websocket which uses another port. This means we needed to add another unique route to this project to expose the websockets port.

#### Docker
To build the images of the applications we make use of Docker.

### SAP Cloud Foundry
SAP Cloud Foundry is an open-source cloud application platform. Cloud Foundry makes it faster and easier to build, test, deploy and scale applications.

#### Buildpack
Instead of Docker, we used the buildpack of NodeJS that is provided by Cloud Foundry. 
Buildpacks provide framework and runtime support for apps. 
Buildpacks typically examine your apps to determine what dependencies to download and how to configure the apps to communicate with bound services.

#### Routing
Cloud Foundry only listens on port 8080. That's the reason why the both ports of the applications were exposed on 8080. 
That's not a problem because both applications are in different containers. 
Otherwise it will give a error 'Address already in use'.
Also another difference with Openshift is that you don't have to give the port where the websocket is listening. 
When you leave this blank, Cloud Foundry will automaticaly search for a suitable port (in this case 8080).
