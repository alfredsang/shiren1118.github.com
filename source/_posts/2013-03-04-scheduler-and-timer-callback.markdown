---
layout: post
title: "[cocos2d-x wiki翻译]Scheduler and Timer Callback"
date: 2013-03-04 16:57
comments: true
categories: [cocos2d-x wiki翻译]
---


## Scheduler and Timer Callback

<div style='display:none;'>
Scheduler is responsible for triggering the scheduled callbacks.
</div>

调度器负责触发调度回调.

### Two different types of callbacks (selectors):

<div style='display:none;'>
update selector: the 'update' selector will be called every frame. You can customize the priority.
custom selector: A custom selector will be called every frame, or with a custom interval of time.
The 'custom selectors' should be avoided when possible. It is faster, and consumes less memory to use the 'update selector'.
</div>

更新selector:  'update' selector可以在每个frame调用.你可以自定义优先级.
自定义selector: 自定义selector可以在每个frame调用, 或自定义的时间段内调用.
'custom selectors'尽可能避免使用. 相对于使用'update selector'来说，它比较快，且内存消耗较小.



### CCScheduler vs. NSTimer

<div style='display:none;'>
The Cocos2D Scheduler provides your game with timed events and calls. You should not use NSTimer. Instead use CCScheduler class.

The reasons as follow:
</div>

Cocos2D Scheduler 为你的游戏提供了时间时间和调用.你不要使用NSTimer.而不是用CCScheduler类.

原因如下:


<div style='display:none;'>
CCNode objects know how to schedule and unschedule events,and using the Cocos2D Scheduler has several distinct advantages over just using NSTimer.
</div>

CCNode对象知道如何去调度和解除调度事件，和仅使用NSTimer相比，使用Cocos2D Scheduler有很多不一样的优点.

- The scheduler calls get deactivated whenever the CCNode is no longer visible or is removed from the scene.
- The scheduler calls are also deactivated when Cocos2D is paused and are rescheduled when Cocos2D is resumed.
- The scheduler delivers a interval time of the milliseconds that have passed since the last call.This interval time is useful in Physics engines.
- Using scheduler with this->scheduleUpdate(); call ensures that your update function will be called before each frame needs to be rendered.

<div style='display:none;'>
Accordingly,CCScheduler can save you a lot of time over NSTimer and let you focus on the mechanics of your game.
</div>

因此，CCScheduler会比NSTimer节省很多时间，让你更加关注于你的游戏构成.

Last updated by Iven Yang at Updated about 1 month ago.


### Comment