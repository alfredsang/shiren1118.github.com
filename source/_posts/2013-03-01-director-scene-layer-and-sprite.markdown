---
layout: post
title: "[cocos2d-x wiki翻译]Director Scene Layer and Sprite(ok)"
date: 2013-03-01 16:22
comments: true
categories: [cocos2d-x wiki翻译]
---


## Director Scene Layer and Sprite

### Director Scene Layer and Sprite
- Scenes
- Director
- Layers
- Multiple Layers Example:
- Sprites
- References
- Comment


### Scenes·场景
<div style='display:none'>
A scene (implemented with the CCScene object) is more or less an independent piece of the app workflow. Some people may call them “screens” or “stages”. Your app can have many scenes, but only one of them is active at a given time.
</div>

场景是app工作流中一个独立的的部分。人们常常称呼它们为 “screens”或“stages”。你的app可能有很多场景，但每次你只能显示（激活）它们中的一个。

<div style='display:none'>
For example, you could have a game with the following scenes: Intro, Menu, Level 1, Cutscene 1, Level 2, Winning cutscene, losing cutscene, High scores screen.
</div>

比如，某个游戏有如下场景：介绍，菜单，Level 1, 画面1, Level 2, 获胜画面, 失败画面, 最高记录screen等.

<div style='display:none'>
You can define every one of these scenes more or less as separate apps; there is a bit of glue between them containing the logic for connecting scenes (the intro goes to the menu when interrupted or finishing, Level 1 can lead you to the cutscene 1 if finished or to the losing cutscene if you lose, etc.).
</div>

你可以定义场景为独立应用部分，通过它们包含的逻辑或连接场景(当中断或完成的时候，介绍界面会跳到菜单。如果完成，Level 1会引到你进入画面1，如果失败，Level 1会进入失败画面，等等.)将它们有机的组装起来。


<div style='display:none'>
	-Cutscene 画面剪辑
</div>


![](http://www.cocos2d-x.org/attachments/1591/scenes650x144.png)


<div style='display:none'>
A cocos2d CCScene is composed of one or more layers (implemented with the CCLayer object), all of them piled up. Layers give the scene an appearance and behavior; the normal use case is to just make instances of Scene with the layers that you want.
</div>

cocos2d CCScene是由一个或多个layer（通过CCLayer对象实现的）组合，堆砌而成。Layers给了场景外观和行为；常规用法是通过你想要的layers来产生场景实例。

<div style='display:none'>
There is also a family of CCScene classes called transitions (implemented with the CCTransitionScene object) which allow you to make transitions between two scenes (fade out/in, slide from a side, etc).
</div>

当然，CCScene家族类中也有被成为转换类（通过CCTransitionScene对象实现），它允许你在2个场景中切换（ 淡出/淡入，slide动画，等待）。


<div style='display:none'>
Since scenes are subclasses of CCNode, they can be transformed manually or by using actions. See Actions for more detail about actions.
</div>

由于场景都是CCNode的子类，所以可以手动或者通过actions方式来改变它们，详见actions部分。


<div style='display:none'>
- See more at: http://www.cocos2d-x.org/projects/cocos2d-x/wiki/Director_Scene_Layer_and_Sprite#sthash.jF0fVw56.dpuf
</div>



### Director·导演类

<div style='display:none'>
The CCDirector is the component which takes care of going back and forth between scenes.
</div>

CCDirector是控制场景之间进出的组件。

<div style='display:none'>
The CCDirector is a shared (singleton) object. It knows which scene is currently active, and it handles a stack of scenes to allow things like “scene calls” (pausing a Scene and putting it on hold while another enters, and then returning to the original). The CCDirector is the one who will actually change the CCScene, after a CCLayer has asked for push, replacement or end of the current scene.
</div>

CCDirector是共享（单例）对象。它知道当前哪个场景正在显示，并且它处理 场景栈 来让 比如场景调用（暂停场景和稍待片刻进入其他入口，之后返回起始点）。

CCDirector是实际改变对象的那个，在CCLayer被询问是否入栈之后，替换或者终止当前场景的控制者。

<div style='display:none'>
The CCDirector is also responsible for initializing OpenGL ES.
</div>

CCDirector也可以通过初始化OpenGL ES来响应。

<div style='display:none'>
- See more at: http://www.cocos2d-x.org/projects/cocos2d-x/wiki/Director_Scene_Layer_and_Sprite#sthash.jF0fVw56.dpuf
</div>



### Layers·层

<div style='display:none'>
A CCLayer has a size of the whole drawable area, and knows how to draw itself. It can be semi transparent (having holes and/or partial transparency in some/all places), allowing to see other layers behind it. Layers are the ones defining appearance and behavior, so most of your programming time will be spent coding CCLayer subclasses that do what you need.
</div>

CCLayer有 全部可画区域的尺寸，并且知道如何去绘图。它可以半拉式渐变（），让你 去看其他位于它下方的层。Layers是定义外观和行为的，所以在你的大部分编程时间都将花费在CCLayer子类做你需要做的事情上。

![](http://www.cocos2d-x.org/attachments/1592/layers.png)


<div style='display:none'>
The CCLayer is where you define event handlers. Events are propagated to layers (from front to back) until some layer catches the event and accepts it.
</div>

CCLayer是你定义事件处理的地方，事件会被传播到layer上（从前到后，冒泡），直到某个layer捕获此事件并接受它为止。

<div style='display:none'>
Although some serious apps will require you to define custom CCLayer classes, cocos2d provides a library of useful predefined layers (a simple menu layer: CCMenu, a color layer: CCColorLayer, a multiplexor between other layers: CCMultiplexLayer, and more ).
</div>


尽管一些apps会需要你去个性化定义CCLayer类，cocos2d还是提供了一个非常有用的内置预先定义好的layers（简单的菜单layer：如CCMenu，颜色layer：如CCColorLayer，在层之间多路传送的layer：如CCMultiplexLayer，等等）的类库。


<div style='display:none'>
Layers can contain CCSprite objects, CCLabel objects and even other CCLayer objects as children.
</div>

Layers可以包含CCSprite对象，CCLabel对象，甚至是CCLayer子对象。


<div style='display:none'>
Since layers are subclass of CCNode, they can be transformed manually or by using actions. See Actions for more detail about actions.
</div>

由于layers都是CCNode的子类，所以可以手动或者通过actions方式来改变它们，详见actions部分。
 


__Multiple Layers Example__:

	CCLayerGradient* layer1 = CCLayerGradient::create(ccc4(255, 0, 0, 255), ccc4(255, 0, 255, 255)); 
	layer1->setContentSize(CCSizeMake(80, 80)); 
	layer1->setPosition(ccp(50,50)); 
	addChild(layer1); 

	CCLayerGradient* layer2 = CCLayerGradient::create(ccc4(0, 0, 0, 127), ccc4(255, 255, 255, 127)); 
	layer2->setContentSize(CCSizeMake(80, 80)); 
	layer2->setPosition(ccp(100,90)); 
	addChild(layer2); 

	CCLayerGradient* layer3 = CCLayerGradient::create(); 
	layer3->setContentSize(CCSizeMake(80, 80)); 
	layer3->setPosition(ccp(150,140)); 
	layer3->setStartColor(ccc3(255, 0, 0)); 
	layer3->setEndColor(ccc3(255, 0, 255)); 
	layer3->setStartOpacity(255); 
	layer3->setEndOpacity(255); 
	ccBlendFunc blend; 
	blend.src = GL_SRC_ALPHA; 
	blend.dst = GL_ONE_MINUS_SRC_ALPHA; 
	layer3->setBlendFunc(blend); 
	addChild(layer3); 



![haha](http://www.cocos2d-x.org/attachments/download/1657)

<div style='display:none'>
- See more at: http://www.cocos2d-x.org/projects/cocos2d-x/wiki/Director_Scene_Layer_and_Sprite#sthash.jF0fVw56.dpuf
</div>


### Sprites·精灵

<div style='display:none'>
A cocos2d' sprite is like any other computer sprite. It is a 2D image that can be moved, rotated, scaled, animated, etc.
</div>

cocos2d的精灵看起来和其他计算机精灵一样。它是2D图像，可以移动，旋转，缩放，动画，等等

<div style='display:none'>
Sprites (implemented using the CCSprite class) can have other sprites as children. When a parent is transformed, all its children are transformed as well.
</div>


Sprites（通过使用CCSprite类实现）可以有多个其他精灵作为子对象，当父类被触发，它的所有子对象也同时被触发。

<div style='display:none'>
Since sprites are subclass of CCNode, they can be transformed manually or by using actions. See Actions for more detail about actions.
</div>

由于精灵都是CCNode的子类，所以可以手动或者通过actions方式来改变它们，详见actions部分。



###  References·参考

cocos2d for iPhone:  
[foo]: http://www.cocos2d-iphone.org/wiki/doku.php/prog_guide:basic_concepts (cocos2d基本概念)

 


### Comment

