---
layout: post
title: "[翻译] Generative models for road network reconstruction"
categories: [translation]
tags: [map-inference]
published: false
---

[Kuntzsch, Colin, Monika Sester, and Claus Brenner. "Generative models for road network reconstruction." International Journal of Geographical Information Science (2015): 1-28.](http://www.tandfonline.com/doi/abs/10.1080/13658816.2015.1092151?journalCode=tgis20)

这是我读到的一篇论文，我觉得文笔不错，所以，决定翻译一下，顺便学习一下句式。

<h2 style="text-align:center;">Generative models for road network reconstruction</h2>
<h2 style="text-align:center;">交通道路网络重建的生成模型</h2>

**ABSTRACT**

This work aims at the inference of traffic networks from GPS
trajectories. 

这项工作旨在从GPS轨迹中推断出交通网络。

We perform geometry and topology reconstruction
of the network in a multistep process. 

我们通过一个多步骤的过程，来实施网络的几何和拓扑重建。

Our main contributions are
the formulation of an explicit intersection model with a score
function that accounts for consistency with the raw tracking
data, as well as for a topology prior and the search for the best
model by maximization of this score function using a Markov
chain Monte Carlo sampler. 

我们最主要的贡献在于，构想一个显式的路口模型，通过一个评分函数，来解释与原追踪数据的一致性，
以及一个拓扑先验，通过使用Markov chain Monte Carlo采样器，来最大化这个评分函数，
从而得到最佳模型。

We demonstrate the viability of our
model-based approach with experiments on GPS data sets of
varying size and data quality, followed by a comparison with
results achieved by alternative, heuristic approaches.

我们通过在不同规模和数据质量的GPS数据集上的试验，展示了基于模型方法的可行性。
同时，与另外的探索性方法的结果进行了比较。



**Introduction**

Official road maps usually underlie temporal update cycles of several months 
or even years. 

官方的道路地图通常基于几个月甚至几年的临时更新周期。

underlie: [no passive] underlie something (formal) to be the basis or cause 
of something. 

They only incorporate static aspects of traffic networks, ignoring temporary
modifications of the network geometry and/or topology, for example, due to 
road construction works. 

他们通常集成了交通网络的静态方面，但忽略了网络几何和/或者拓扑的临时更改，例如，道路建设工作。

In recent years, collaborative mapping has become feasible at an
unprecedented scale, leading to global mapping efforts such as OpenStreetMap. 

最近几年，协作式制作地图大规模地变得可行，导致了例如OpenStreetMap这样的全球制作地图的努力。

unprecedented: adj. never done or known before

These approaches have many advantages, including the prospect of up-to-date maps.

这些方法有很多优势，例如实时地图的前景。

However, a major drawback is that the timeliness as well as the homogeneity of quantity
and quality of the map is not guaranteed since they depend on the time budget and
skills of their voluntary contributors. 

然而，一个主要的缺点是，及时性和地图的数量和质量同质性无法得到保证，
因为这些是基于贡献者的时间成本和技能。

timeliness: n. done or occurring at a favorable or useful time

homogeneity: n. the quality or state of being homogeneous

Automatic map construction methods offer a very attractive way to address all of
these problems. 

自动地图重建提供了一个非常吸引人的方式去解决所有的这些问题。

Timeliness, quantity and quality of the produced map are a function of
the input data only. 

生成地图的及时性，数量和质量都会变成一个函数的输入数据。

They are neither dependent on operator skills and time budgets, nor
on fixed update cycles. 

他们不基于操作者的技能和时间成本，也不基于固定的更新周期。

The required tracking data will be ubiquitously available in the
near future. 

在不久的将来，需要的追踪数据会变得到处都是。

ubiquitously: adv. by seeming to be everywhere or in several places at the same time

Based on a large number of input tracks, a homogeneous quantity and
quality can be achieved; 

基于大规模的输入轨迹，可以达到一致的数量和规模。

short-term changes in geometry and/or topology can be
detected and real-time updates will become possible. 

可以检测到几何和拓扑上短期的改变，实时的更新变得可能。

In this way, even changes to the
traffic network geometry and/or topology due to temporary or permanent modifications
of traffic rules (road works, detours, etc.) can be detected as they are immediately
observable in the actual traffic movement. 

通过这种方式，由于交通规则的临时或者永久改变（道路工作，绕路，等等）
造成的交通网络的几何和拓扑改变，
都可以立即在实际的交通移动中被观察到。

detour: n. a road or route that is used when the usual one is closed

Having access to high-quality, high-update
rates map information is essential for many of today’s location-based services in the
general field of navigation and also for driver assistance systems.

对于许多今天的地理位置服务来说，例如导航的通用领域和驾驶者辅助系统，
能够获取高质量和快速更新的地图信息是十分关键的。

In order to be usable in practice, automatically derived road maps need to feature
high quality, both geometrically and topologically. 

为了能用于实际中，自动产生的地图需要几何和拓扑上的高质量。

derive from something | be derived from something: to come or develop from something

However, as recent work on road
network reconstruction has demonstrated, 
there are still a lot of challenges, for example,
resulting from working with low-sampling frequency, 
low-accuracy GPS tracking data.

然而，根据最近的道路网络重建的相关工作来看，这个方向还存在很多的挑战，
例如，从低采样率和低精度的GPS追踪数据中得到结果。

The geometric quality of GPS lies in the range of 5–20 m – which is far beyond the
requirements for advanced mapping products. 

GPS的几何质量在5-20米的范围内，这远远低于很多先进地理产品的要求。

Thus, the challenge is to integrate large
quantities of possibly inaccurate data while eliminating gross errors and data gaps in
order to delineate the underlying road network. 

因此，挑战是，在消除明显的错误和数据空隙的同时，如何集成大数量的可能不准确的数据，
为了详细描述潜在的道路网络。

gross: adj. being the total amount of something before anything is taken away. 
very obvious and unacceptable. 

delineate something (formal): v. to describe, draw or explain something in detail

underlying: 1) important in a situation but not always easily 
noticed or stated clearly; 2) existing under the surface of 
something else. 

The paper presents a novel approach that combines a 
raster-based method for clustering potential roads with a subsequent
statistical approach based on a Monte Carlo sampler. 

这篇文章展示了一个新颖的方法，结合了一个基于光栅的方法来聚类潜在的道路，
和随后一个基于Monte Carlo采样器的统计方法。

subsequent: happening or coming after something else. 

Prior knowledge is introduced in terms of typical junction structures. 

判断典型的结合点(junction)结构上，引入了现有的经验。






