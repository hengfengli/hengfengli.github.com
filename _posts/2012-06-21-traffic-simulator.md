---
layout: post
title: "Traffic Simulator"
categories: [computer]
tags: [traffic]
published: true
---

This is a MEDC research project. We aim to develop a decentralized 
traffic simulator presented by Web browsers. 

My tasks: 

*   Processed the map data from OpenStreetMap and learnt related APIs 
and the structure of an OSM file. 
*   Implemented a Web map application by using OpenLayer.js and 
Processing.js. 
*   Solved the cross-platform communication problem by using 
Apache Thrift. 
*   Addressed the key issue of continuously pushing simulation data
from server side to browser side (long-HTTP connection technique using
PHP and JavaScript). 

Before us, an undergraduate group of eight people developed a  similar
system, but only can simulate few vehicles. However, our system is able
to simulate more than <b>10,000</b> vehicles on a single computer. 

A user can select a region for simulation. The system will automatically 
download the map data from OpenStreetMap. 

![screen](https://lh4.googleusercontent.com/-N1npJ6VDYZw/T-QBKPTz9yI/AAAAAAAAAUQ/9smVZsGRvg4/s640/area_captor.png)

A dot in red or green color (like an ant) represent a single vehicle. 
The movement of vehicles is calculated by a car-following model. 
Vehicles can also change their lanes. 

![screen](https://lh6.googleusercontent.com/-syxNFy1ugTA/T-QBO72ufaI/AAAAAAAAAUo/4n60rKe3Zu4/s640/snapshot1.png)

The presentation can be changed to a density view. In this view, traffic 
flow of road segments are presented which is similar to Google Traffic Maps 
but the key difference is that our system updates the traffic in real time. 

![screen](https://lh3.googleusercontent.com/-9WPkdHtmWJo/T-QBLqxUfgI/AAAAAAAAAUY/RLa0_Gk6Yuw/s640/density.png)

We divide the map data into different parts for processors in order 
to achieve decentralized computing. Different colors of streets means
that they are in different processors. 

![screen](https://lh4.googleusercontent.com/-X4dCfjSlwjA/T-QBMwtYXDI/AAAAAAAAAUg/ns-dV10hwig/s640/partitions2.png)

This is a research project so that I cannot make the 
code publicly available. 

