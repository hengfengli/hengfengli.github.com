---
layout: post
title: "Traffic Simulator"
categories: [computer]
tags: [traffic]
published: false
---

最近一直在写MEDC project的报告，MEDC project是我们专业一个课程，占两门课的学分，跟着老师做project。
这次我们做的是一个分布式的Traffic Simulator，然后老师希望能通过Browser的形式展现出来。

由于之前在百度贴吧前端组实习，在这方面比其他人要强一点点，所以我就负责前端和地图处理的方面。
在开发的过程中呢，也学习了许多东西，例如，开发OpenStreetMap地图的OpenLayer.js，画图的Processing.js，
Facebook跨语言通信的框架Thrift，还有HTTP长连接的实现，等等。

最终，算是圆满实现了，老师也比较开心，因为之前有个本科生的8人小组，开发了一个类似的系统，但最终效果很差，
只能模拟很少量的车。而我们的系统，可以模拟上万条街吧。

这个是用户选定指定模拟的区域大小。

![screen](https://lh4.googleusercontent.com/-N1npJ6VDYZw/T-QBKPTz9yI/AAAAAAAAAUQ/9smVZsGRvg4/s640/area_captor.png)

这个是模拟汽车在真实的地图上跑。

![screen](https://lh6.googleusercontent.com/-syxNFy1ugTA/T-QBO72ufaI/AAAAAAAAAUo/4n60rKe3Zu4/s640/snapshot1.png)

显示交通流量。

![screen](https://lh3.googleusercontent.com/-9WPkdHtmWJo/T-QBLqxUfgI/AAAAAAAAAUY/RLa0_Gk6Yuw/s640/density.png)

由于是分布式计算，所以，给出了地图是如何分割，哪些worker负责哪些街道的模拟计算。

![screen](https://lh4.googleusercontent.com/-X4dCfjSlwjA/T-QBMwtYXDI/AAAAAAAAAUg/ns-dV10hwig/s640/partitions2.png)

接下来，这个月有很多事情要办，要弄offer，签证，手机，搬房子，带我妈出去玩玩，选定课题，开始phd的旅途。

这是人生另一个新的开始，就像高中毕业，本科毕业一样，我即将Master毕业了。