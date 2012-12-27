---
layout: post
title: "问题0——Event Listener 的概念"
category: [problem]
tags: [event-listener]
---
{% include JB/setup %}


今天我想写一下“事件监听”这个概念。因为我一直都不太明白listener的用法，甚至在上次的作业中，我居然把listener写成了一个带有runnable的类，现在想想，真的挺可笑的。

最近，在看一个写贪吃蛇的代码，里面的体现出来的event listener的思想挺好的，所以我想写下来，以供日后复习。

在贪吃蛇游戏中，主要有snake，food，ground这三个实体，还有GamePanel类显示游戏，Controller来控制游戏逻辑和处理按键事件。

这三个组件正好体现了MVC的概念，model，view，controller。当然，snake，food，ground是model，GamePanel是view。

![](https://lh3.googleusercontent.com/-_ZjwXTEXNPU/TYCA8_PHtFI/AAAAAAAAAH8/6SdAHlIbwtA/s640/0_1291978371BOIV.jpg)

这样大概就形成了MVC模型。Controller负责处理来自GamePanel的键盘事件和来自Snake的移动事件。

![](https://lh4.googleusercontent.com/-Go0xZBfsSlk/TYCA9eIWvjI/AAAAAAAAAIA/nUHwdxWXtr8/s640/0_12919787264KO3.jpg)

要实现事件监听，

首先，要在Snake中要创建一个Set来存储所有的Listener。

private Set&lt;SnakeListener&gt; listeners = new HashSet&lt;SnakeListener&gt;();

然后，在Snake中要有一个addListener的方法，可以讲监听者加入到Set中。

public void addSnakeListener(SnakeListener l){ 		if(l != null){ 			this.listeners.add(l); 		} 	}

在Snake移动之后，立即触发SnakeListener的事件

move();

for(SnakeListener l : listeners) { 		l.snakeMoved(Snake.this); 	}

最后，Controller要实现SnakeListener的snakeMoved方法，当snake移动之后，触发的事件。Controller就可以触发GamePanel重新显示。

这样很好的完成了，Snake移动是事件源，Controller实现了SnakeListener，并通过addListener使得Controller成为了Snake的监听者。当Snake移动时，就会触发Controller中的事件。

同样的道理，对于GamePanel的keyboard事件监听，只需要Controller实现keyListener，并用addKeyListener使得Controller成为GamePanel的监听者，这样当键盘事件发生时，就会触发Controller中相应的方法。

事件源=&gt;监听接口=&gt;监听者相应的方法
