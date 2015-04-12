---
layout: post
title: "Machine learning in navigation devices: detect maneuvers using accelerometer and gyroscope"
description: ""
category:
tags: []
---
{% include JB/setup %}

## Introduction

Car navigation software greatly assist drivers. It helps navigate in unfamiliar areas and bypass traffic jams. This is a result of big work of people from around the world, which made ​​our lives easier. But we can not rest on our laurels, technology go ahead and quality of the programs should also grow.

![](https://raw.githubusercontent.com/blindmotion/docs/master/pics/article/habr/kdpv4.png)

Today, in my opinion, one of the problems of navigation devices - is that they do not suggest driver which traffic lane he should take. This problem increases travel time, traffic jams and accidents. Google maps recently started to display traffic lanes for turns, it is a good result, but there is much to improve. The maps do not know on what lane the car is and it is hard to find it out with means of gps as it has too big inaccuracy. If we knew the current lane, we would know the speed of movement for each of lanes and we could suggest user which lane he should take. For example, the navigator could say "continue to keep this lane until the intersection" or "take the left lane."

In this article we will try to tell how we are trying to determine the lane changes, the current lane, overtakes and other maneuvers using machine learning and accelerometer with gyroscope (the sensors that every modern mobile phone has).

Lane change recommendation could be not only if the speed on the lane is slow due to the fact that cars turn from this lane. Also lane change could be recommended if there is an accident on this lane. Currently, accidents and other problems are mostly provided manually by users in navigation software. Using this algorithm, it would be possible to provide accidents automatically, looking at the maneuvers of cars. If cars in the same place massively make an obstacle avoidance, it seems, that there was an accident or other problem. A server could aggregate these events from vehicles and automatically add road difficulties to maps. The system could alert drivers to change lanes in advance.

## How could those problems be solved?

Me and several students of [Computer Science Center](https://compscicenter.ru/) in Saint-Petersburg, make an open source project, that detects car maneuvers using accelerometer and gyroscope. In the end, we want to make a library with an open source license, which processes the input data from the sensors of a mobile phone or any other device and outputs events such as lane changes, overtakes, obstacle avoidance, turns and u-turns. The only thing that the users of library will need to do is to implement a switch in their program and react to certain events.

The library, in theory, could be used not only by phones, but also, for example, by devices based on microcontrollers that monitor vehicles. I have to say that we are in the middle of the road and there is no assurance that we will be able to identify all the events, but for now it seems like we could succeed.

## How do maneuvers look like at a chart?

The events are marked with dashed vertical lines:

![](https://raw.githubusercontent.com/blindmotion/docs/master/pics/pres-events-smooth.png)

Let's leave only y accelerometer axis (lateral acceleration, top graph) and the z gyroscope axis (car rotation, top view, bottom graph) active. It can be seen that u-turns and turns are accompanied by increased lateral acceleration and rotation around the z axis. During lane change gyroscope and accelerometer quickly change their values from positive to negative.

It seems that the person looking at these graphs can more or less understand what type of events took place. A classification algorithm based on machine learning can presumably make it too.

## What has already been done

We gathered the original data: it is about 1000 kilometers of video and telemetry data gathered with a phone on the roads. It is available [here](https://yadi.sk/d/NJSeeOk5bYm7V) and can freely be used in your project if you need it.

We also made dashboard for convenient work with video and data that looks like this:

![](https://raw.githubusercontent.com/blindmotion/docs/master/pics/pres-dashboard_full.png)


It consists of three parts. At the top there is a video preview with the movement of car. In the center there is a chart in which the data can be viewed: accelerometer, gyroscope, current speed. In the bottom there is a script (coffescript) with the help of which you can modify the data on the fly and display it on the chart (for example, we need a smooth graph).

In addition the dashboard provide an opportunity to mark events on video timeline and save them to a file and then display the marked events on the chart with annotations.

Perhaps some of you in your projects also compare video with sensors data, or just look at the sensor data. If so, the dashboard [github.com/blindmotion/dashboard](http://github.com/blindmotion/dashboard) could be useful for you. It is quite easy to use, allows you to scale, transform data on the fly and has an open license, which means you can freely use it and modify.
