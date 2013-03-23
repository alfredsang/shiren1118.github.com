---
layout: post
title: "about delegete"
date: 2013-03-18 00:29
comments: true
categories: 
---


rootviewcontroller 上增加了一个tableviewcontroller，tablecell里有一个view，view里又有一个button


view大于2层就很难受了，delegate传来传去的。

```
view1:
	button - click
		-(void)click_event_callback;
	
tableviewcontroller
	view1.delegate = self;
	
	-(void)click_event_callback;
	
	
rootviewcontroller
	tableviewcontroller *tbvc.delegate = self;
	
	-(void)click_event_callback;
	
```


这样做的原因是点击button的时候，需要在rootviewcontroller.navagationcontroller中push一个viewcontroller，而tableviewcontroller是没有navagationcontroller的。

我们通常设置delegate

	@property(nonatomic,assign,readwrite) id<*Protocol> _delegate;

用assign是防止产生交替dealloc死锁内存问题。


	