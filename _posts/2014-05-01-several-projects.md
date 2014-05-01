---
layout: post
title:  "OSM-parser, KNN, Beijing Traffic Visualization"
date:   2014-05-01 11:54:00
categories: programming
---

最近有把两个小工具放在github上，osm-parser和knn的实现。因为之前自己的研究，有涉及到这两个
东西，而且觉得还蛮实用的。然后，就是自己对于北京traffic视觉化的东西，纯属娱乐。

### OSM-parser

OSM-parser是一个针对OSM文件的reader，解析成简单的graph的数据格式。有很多的同事都需要这个
map的数据，现在唯一比较好的途径就是通过OpenStreetMap。处理map数据，也是从我之前的traffic
simulator的经验得来的。其实没有多大的技术含量，就是简单的读取一下xml文件，然后产生graph的数
据。不过，得对具体tag的意义，和文件的格式有大致的了解才行。

[OSM-parser on Github](https://github.com/HengfengLi/osm-parser)

### KNN in road network

KNN也就是k nearest neighbors问题，在很大的graph里面，查找最近的node或者edge
。当然，这个算法可以扩展到多维空间。这个也是自己在做map matching的时候，一开始
用Java写了一个，写得特别复杂，后来用python写，花了不到300行代码。当然，还有可能
进一步优化。现在这个KNN是基于简单的grid索引方式，也就是将地图分割成大小一样的格子。
当需要搜索一个位置附近的node或edge，就看它的附近（100米之内）覆盖了几个方格。
然后，只需要搜索相应的方格就可以了，不需要搜索整个地图。更高级的算法，应该有quad tree，
kd tree，R tree等等。以后有机会，可以一一写出了，然后，比较一下build和search的时间。
纯粹满足好奇心。

[Explore KNN on Github](https://github.com/HengfengLi/spatial-knn-implementations)

### Beijing Traffic Visualization

这个是北京traffic可视化的视频和图片，靠processing完成。由于从清华大学那边拿到了一个数据集，
大概6000辆的士的行车轨迹，2009年的5月份。然后，因为之前有看到一个飞机的可视化作品，觉得很
有意思，所以自己也就玩了一下。

在视频中，GPS点的颜色会根据时间的变化而渐变。就是把每个GPS点都画出来，然后整体覆盖一个透明层
，这样之前画的点就会慢慢褪色。

<iframe width="420" height="315" src="//www.youtube.com/embed/BXpFkSrY_Pw" frameborder="0" allowfullscreen></iframe>

这个图是根据密度（density）来画的。越明亮，意味着同一时间，收集到的GPS点很多。

<a href="https://www.flickr.com/photos/hengfengli/14070765882/" title="Flickr 上 Hengfeng 的 Beijing Traffic Plotting"><img src="https://farm8.staticflickr.com/7370/14070765882_2d9d8251fb.jpg" width="500" height="331" alt="Beijing Traffic Plotting"></a>

当然，这只是taxi的可视化。跟所有北京的车相比，还是九牛一毛的。