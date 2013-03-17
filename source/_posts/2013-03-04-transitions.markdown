---
layout: post
title: "Transitions"
date: 2013-03-04 16:58
comments: true
categories: 
---


## Transitions
 
### Introduction
One of the cool features that Cocos2d-x has to offer is the power of transitions within two different scene. Transitions are effects such as: wipe, fade, zoom, and split. You can use transitions to switch between Cocos2d-x Scene objects. Sceneclass is derived from CocosNode and it is similar to Layer. You can add other CocosNode, such as Layer(s) and Sprite(s) into a Scene.

Technically, a transition scene is an scene that performs a transition effect before setting control to the new scene.

### Create transition
Time is the number of seconds for the transition. To apply transition to scenes, the syntax is as follows:

```c++
CCDirector::sharedDirector()->replaceScene(CCTransitionFade::create(0.5,newScene));
```

Some transitions has custom parameter(s); for example, FadeTransition has the fade color as extra parameter.
static CCTransitionFade* create(float duration,CCScene* scene, const ccColor3B& color);
To enable a transition, it is not much more difficult. Here we have an small example:

```c++
CCScene *s = SecondPage::scene();

CCDirector::sharedDirector()->setDepthTest(true);

CCTransitionScene *transition = CCTransitionPageTurn::create(3.0f, s, false);
```

If you run this you will have a “page turn” effect. This is, like turning the page on a paper made book.

![](http://www.cocos2d-x.org/attachments/download/1623)

### More transitions
There are many more transition types, you can see the full list in the class reference, in the official Cocos2D-X documentation.

Last updated by Zhe Wang at Updated about 1 month ago.


### Comment