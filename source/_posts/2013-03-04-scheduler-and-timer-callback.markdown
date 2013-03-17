---
layout: post
title: "Scheduler and Timer Callback"
date: 2013-03-04 16:57
comments: true
categories: 
---


## Scheduler and Timer Callback
 
Scheduler is responsible for triggering the scheduled callbacks.

### Two different types of callbacks (selectors):
update selector: the 'update' selector will be called every frame. You can customize the priority.
custom selector: A custom selector will be called every frame, or with a custom interval of time.
The 'custom selectors' should be avoided when possible. It is faster, and consumes less memory to use the 'update selector'.

### CCScheduler vs. NSTimer
The Cocos2D Scheduler provides your game with timed events and calls. You should not use NSTimer. Instead use CCScheduler class.
The reasons as follow:

CCNode objects know how to schedule and unschedule events,and using the Cocos2D Scheduler has several distinct advantages over just using NSTimer.

- The scheduler calls get deactivated whenever the CCNode is no longer visible or is removed from the scene.
- The scheduler calls are also deactivated when Cocos2D is paused and are rescheduled when Cocos2D is resumed.
- The scheduler delivers a interval time of the milliseconds that have passed since the last call.This interval time is useful in Physics engines.
- Using scheduler with this->scheduleUpdate(); call ensures that your update function will be called before each frame needs to be rendered.

Accordingly,CCScheduler can save you a lot of time over NSTimer and let you focus on the mechanics of your game.

Last updated by Iven Yang at Updated about 1 month ago.


### Comment