---
layout: post
title: "[cocos2d-x wiki翻译]Transitions"
date: 2013-03-04 16:58
comments: true
categories: [cocos2d-x wiki翻译]
---


## 转场动画 (Transitions)
 
### 介绍

<div style='display:none;'>
One of the cool features that Cocos2d-x has to offer is the power of transitions within two different scene. Transitions are effects such as: wipe, fade, zoom, and split. You can use transitions to switch between Cocos2d-x Scene objects. Sceneclass is derived from CocosNode and it is similar to Layer. You can add other CocosNode, such as Layer(s) and Sprite(s) into a Scene.
</div>

Cocos2d-x提供的最爽的一个特性是在2个不同场景间转场的能力.转场的效果有：wipe, fade, zoom, 和 split. 你可以使用转场动画在Cocos2d-x场景对象中切换.Sceneclass继承自CocosNode,它和Layer非常相似.你可以增加其他CocosNode,如 Layer(s) 和 Sprite(s) 放到场景中.

<div style='display:none;'>
Technically, a transition scene is an scene that performs a transition effect before setting control to the new scene.
</div>

技巧上说,转场的场景是一个在设置控制到新的场景前执行转场效果的场景.

### 创建转场动画

<div style='display:none;'>
Time is the number of seconds for the transition. To apply transition to scenes, the syntax is as follows:
</div>

Time 是转场的秒数.为了把转场动画应用到场景中，语法如下:

```
CCDirector::sharedDirector()->replaceScene(CCTransitionFade::create(0.5,newScene));
```

<div style='display:none;'>
Some transitions has custom parameter(s); for example, FadeTransition has the fade color as extra parameter.
static CCTransitionFade* create(float duration,CCScene* scene, const ccColor3B& color);
To enable a transition, it is not much more difficult. Here we have an small example:
</div>

一些转场可以有自定义参数;例如,FadeTransition有fade color作为额外参数.

```
static CCTransitionFade* create(float duration,CCScene* scene, const ccColor3B& color);
```

运行转场动画其实一点不难，这儿有一个小例子:

```
CCScene *s = SecondPage::scene();

CCDirector::sharedDirector()->setDepthTest(true);

CCTransitionScene *transition = CCTransitionPageTurn::create(3.0f, s, false);
```

<div style='display:none;'>
If you run this you will have a “page turn” effect. This is, like turning the page on a paper made book.
</div>

如果运行这段代码，你会看到翻页效果.看起来就像是一页一页翻书。

![](http://www.cocos2d-x.org/attachments/download/1623)

### 更多转场动画
<div style='display:none;'>
There are many more transition types, you can see the full list in the class reference, in the official Cocos2D-X documentation.
</div>

想知道更多转场动画类型，你可以在官方Cocos2D-X文档的相关类中参考完整列表。
<div style='display:none;'>
Last updated by Zhe Wang at Updated about 1 month ago.
</div>

### Comment