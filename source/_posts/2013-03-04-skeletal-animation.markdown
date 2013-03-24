---
layout: post
title: "[cocos2d-x wiki翻译]骨骼动画"
date: 2013-03-04 16:57
comments: true
categories: [cocos2d-x wiki翻译]
---

<!-- Skeletal animation -->

## 骨骼动画 vs. 精灵表
<!-- ## Skeletal animation vs. Sprite sheets -->

<div style='display:none;'>
You can creat animations using "sprite sheets" which is quick and easy. Until you realize your game needs lots of animations and the memory consumption goes way up, together with the time required to load all the data. Also, to limit the size, you need to limit yourself to a low FPS for the animation, which also means the animation doesn’t look as smooth as you’d like. This is where skeletal animation comes in.
</div>
又快有简单的方法是使用“精灵表”创建动画.当你实现游戏的时候需要大量动画,内存消耗会涨上来,而且需要耗时去加载所有数据.此外,限于大小,你需要为了动画进行自我限制去降低FPS,这意味着动画不是你想要的那么平滑.这就是骨骼动画的由来.

<!-- ## Introducing Skeletal Animation -->
## 骨骼动画简述
<div style='display:none;'>
Skeletal animation is a technique in cocos2d-x animation in which a character is represented in two parts: a surface representation used to draw the character (called skin or mesh) and a hierarchical set of interconnected bones (called the skeleton or rig) used to animate (pose and keyframe) the mesh.
</div>
骨骼动画是cocos2d-x动画在人物渲染方面的技术,分二个部分:用于绘制人物的表面和用于动画(pose and keyframe) 啮合的相互连接的骨骼.

![](http://www.cocos2d-x.org/attachments/1606/Skeletal-Animation.jpg)

<div style='display:none;'>
Cocos2d-x provides a way to have 2d skeletal animations in your applications. The process of skeletal animation may be a bit complicated to setup, but using them afterwards is easy, and there are some tools to simplify the process.
</div>

在你的应用中,Cocos2d-x提供了拥有2d骨骼动画的方式.构建骨骼动画过程可能有点复杂,但随后用起来却非常简单,而且有一些工具可以简化此过程.

<div style='display:none;'>
When using skeletal animation, the animation is composed of several bones which are connected to one another. Affecting a bone also affects all of its children. By composing different transformations on each bone, you obtain different poses for the skeleton.
</div>

<div style='display:none;'>
Now, if you define keyframes with certain transformations for a point in time for each of the bones in the skeleton, you can interpolate between the keyframes to obtain a smooth transition and thus animate the skeleton.
</div>

<div style='display:none;'>
In the attached code, I used a class named Transformation, which contains data about 2D transformations, like translation, rotation and scale. Then, a Keyframe is defined by a frame number and one such Transformation. A collection of Keyframes defines a KeyframeAnimation. Finally, a SkeletonAnimation is a collection of KeyframeAnimations, one for each bone in the skeleton.
</div>

<div style='display:none;'>
Separately, you use a Skeleton, which keeps a list of Joints that define the hierarchy of bones in the skeleton. Different from "sprite sheets",each bone is then assigned a certain texture, like the ones below:
</div>

![](http://www.cocos2d-x.org/attachments/1607/animated-grossini.png)

<!-- ## Tool for skeleton animation -->
## 骨骼动画工具
So far as we know, [CocosBuilder](http://cocosbuilder.com/) is a great, free (MIT license) tool for creating skeleton animations.
CocosBuilder is built for Cocos2d’s Javascript bindings, which means that your code, animations, and interfaces will run unmodified in Cocos2d-x.
A tutorial for cocos2d-iphone can be found at the [Zynga Engineering blog](http://code.zynga.com/2012/10/creating-a-game-with-cocosbuilder/)

<!-- ## Working with cocosbuilder Animations -->
## 与cocosbuilder动画协作
You can use CocosBuilder for creating character animations, animating complete scenes or just about any animation you can imagine. The animation editor has full support for multiple resolutions, easing between keyframes, boned animations and multiple timelines to name a few of the features.

h3.The Basics

In the bottom of the main window you can find the timeline. You use the timeline to create your animations.

![](http://www.cocos2d-x.org/attachments/1610/timeline.png)

By default your ccb-file has a single timeline that is 10 seconds long. CocosBuilder edits animations at a frame rate of 30 frames per second, but when you play back the animation in your app it will use whatever you have set cocos2d to use (the default in cocos2d is 60 fps). The current time is displayed in the top right corner, and has the format minute:second:frame. The blue vertical line also shows the current time. Click the time display to change the duration of the current timeline.

<!-- ### Adding Keyframes -->
### 增加 Keyframes
Animations in CocosBuilder are keyframe based. You can add keyframes to different properties of a node and CocosBuilder will automatically interpolate between the keyframes, optionally with different types of easing.

To add a keyframe, first expand the view of the node by clicking the triangle to the right of the name of the node. This will reveal all the animatable properties of the node. What can be animated varies slightly depending on what type of node you have selected. Once the properties are visible you can click the property in the timeline with the option key held down. This will create a new keyframe at the time of the click. Alternatively, you can create a new keyframe at the time of the time marker by selecting a node then choosing Insert Keyframe in the Animation menu.

Keyframes are automatically added at the current time if you transform a node in the canvas area, given that the transformed property already has one or more keyframes in the timeline.

<!-- ### Editing Keyframes -->
### 编辑 Keyframes
You edit a specific keyframe of a node by moving the time marker to the time of the keyframe and selecting the node. You can focus on a keyframe by double clicking it (which will select the node and move the time marker).

You can select keyframes and move them together by dragging a selection box around them. You can also copy and paste keyframes between nodes. Make sure you only have one selected node when pasting the keyframes. The keyframes will be pasted starting at the time of the time marker.

If you have selected a set of keyframes it is possible to reverse the order of them by selecting Reverse Selected Keyframes in the Animation menu. Use the Stretch Selected Keyframes… option to speed up or slow down an animation by a scaling factor.

<!-- ### Importing a Sequence of Images -->
### 导入多张连续图片 
If you have an animation created by sprite frames it can be tedious to move each individual frame to the timeline. CocosBuilder simplifies this process by automatically importing a sequence of images. Select the frames that you want to import in the left hand project view, then select a CCSprite in the timeline. Now choose Create Frames from Selected Resources in the Animation menu. The frames will automatically be created at the start of the marker. If you need to slow down the animation, select the newly created keyframes and use the Stretch Selected Keyframes… command.

<!-- ### Applying Easing -->
### 应用渐变
CocosBuilder offers a carefully selected subset of the easings provided by cocos2d. To apply an easing right click between two keyframes and select the type of easing that you want to apply.

![](http://www.cocos2d-x.org/attachments/1611/keyframes.png)

Some of the easings have additional options, after the easing has been applied you can right click again and select Easing Setting… from the popup menu.

<!-- ### Using Multiple Timelines -->
### 使用多个时间轴
A very powerful feature of CocosBuilder's animation editor is the ability to have multiple timelines in a single file. You can name the different sequences and play them back from your code by using their name. It's even possible to smoothly transition between the different timelines.

To select, add or edit your timelines use the timeline popup menu:

![](http://www.cocos2d-x.org/attachments/1612/Multiple%20Timelines.png)

In the edit timelines dialog you can get an overview of your timelines, rename them, add new ones and (optionally) set one of the timelines to automatically start playback directly when the ccbi-file is loaded by your app.

![](http://www.cocos2d-x.org/attachments/1613/autoStart.png)

Properties in timelines that do not have keyframes set share their values across timelines. E.g. if you move one node in one timeline it will be moved in all timelines as long as they do not have a keyframe set for the position property. I can sometimes be useful to add a single keyframe to a property just to override the shared value for a specific timeline.

<!-- ### Chaining Timelines -->
### 丈量时间轴
You can automatically play back a sequence of timelines by chaining them. You can also use this feature for automatically looping a timeline.

![](http://www.cocos2d-x.org/attachments/1614/autoPlayback.png)

To have a timeline play in sequence, click the No chained timeline text and select the timeline you want to play right after the current one.

### Playing Back Animations in Code
To programmatically control the animations you create with CocosBuilder you will need to retrieve theCCBAnimationManager. The animation manager will be assigned to the nodes userObject when the ccbi-file is loaded.

1CCNode *myNodeGraph = ccbReader->readNodeGraphFromFile("myFile.ccbi", this);

The action manager will be returned as an autoreleased object. To play back a specific timeline call therunAnimationsForSequenceNamed: method. If a timeline is currently playing it will be immediately stopped when calling this method.

1animationManager->runAnimationsForSequenceNamed("My Timeline");

Optionally, you can use a tween duration to smoothly transition to the new timeline. Where possible linear interpolations will be used for the transition.

1animationManager->runAnimationsForSequenceNamedTweenDuration("My Timeline",0.5f);

It is also possible to receive a callback whenever a timeline has finished playing. You will receive these callbacks even if another timeline is chained in sequence. Use the CCBAnimationManagerDelegate to receive the callbacks.

## 总结 
Thank you taking the time to read through the tutorial and happy coding!If you have other tools for skeleton animation,please let us known or post to this wiki.

## 参考 
[Creating a game with cocosbuilder](http://code.zynga.com/2012/10/creating-a-game-with-cocosbuilder/)
[CocosBuilder Documentation](http://cocosbuilder.com/)
