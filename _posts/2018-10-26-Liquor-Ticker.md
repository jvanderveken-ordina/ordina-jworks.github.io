---
layout: post
authors: [talha-demirci, michael_dewree, sander_smets, frederick_bousson, axel_bergmans]
title: "Liquor Ticker"
image: 
tags: [Angular, NodeJS, NestJS, SAP Cloud, Openshift]
category: 
comments: true
---

# Liquor Ticker

Liquor Ticker is a new awesome Alcohol Sensor that reveals your alcohol level beautifully on your screen.
Just blow on the sensor and you'll see the beer flowing, the more you drank the more beer flows.

The app indicates whether you are safe to drive or not by showing your alcohol level, a green or red color and multiple categories: Safe, Alarm and Positive.
- Safe meaning you are safe to drive, you barely drank any alcohol.
- Alarm means you aren't allowed to drive anymore, if you do you will lose your driver licence at least for 3 hours.
- Positive means your alcohol level is higher than 0.8 promille and you should definitely not get behind the wheel anymore.

## Back End
For the back end we used NodeJS and NestJS on top of it.
Our NestJS app recieves a http package with the alcohol level 


## Front End
The front end exists of a Angular app.
This Angular app was originally part of the NodeSimpleServer from [this blog post](/iot/2017/01/21/Node-with-TypeScript.html).


## Deployment
We deployed Liquor Ticker on 2 cloud platforms: SAP Cloud and Openshift.

### Openshift


### SAP Cloud