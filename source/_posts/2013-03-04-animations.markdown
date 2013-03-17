---
layout: post
title: "Animations"
date: 2013-03-04 16:57
comments: true
categories: 
---


## Animations

Animations
Manual Animation
Sprite Sheet Animation
Creating from .png and .plist file
File animation
Skeleton Animation
Comment

### Manual Animation

You can create an animation from a series of image file, like this:

```c++
 1CCAnimation *animation = CCAnimation::create();
 2
 3// load image file from local file system to CCSpriteFrame, then add into CCAnimation
 4for (int i = 1; i < 15; i++)
 5{
 6    char szImageFileName[128] = {0};
 7    sprintf(szImageFileName, "Images/grossini_dance_%02d.png", i);
 8    animation->addSpriteFrameWithFileName(szImageFileName);  
 9}
10
11animation->setDelayPerUnit(2.8f / 14.0f); // This animation contains 14 frames, will continuous 2.8 seconds.
12animation->setRestoreOriginalFrame(true); // Return to the 1st frame after the 14th frame is played. 
13
14CCAnimate *action = CCAnimate::create(animation);
15sprite->runAction(action);  // run action on sprite object
```

Note that CCAnimation is composed by sprite frames, delay time per frame, durations etc, it's a pack of "data". While CCAnimate is an action, which is created base on CCAnimation object.

### Sprite Sheet Animation
Although manual animation is very easy to understand, it's rarely used in real game projects. Instead, sprite sheet animation is the common solution of 2D animations.

This is a typical sprite sheet. It can be a sequence of sprite frames for an animation, or can be images pack that will be used in a same scene.

![](http://www.cocos2d-x.org/attachments/download/1570)

In OpenGL ES 1.1 period, sprite sheets was widely used for these benefits:
Reduce times of file I/O. Loading a big sprite sheet file is faster than loading lots of small files.
Reduce the memory consumption. OpenGL ES 1.1 can only use power-of-two sized textures (that is a width or height of 2,4,864,128,256,512,1024,...). In other words, OpenGL ES 1.1 allocates power-of-two sized memory for each texture even if this texture has smaller width and height. So using packed sprite sheet image will reduce the fragments of memory.
Reduce the draw calls to OpenGL ES draw method and speed up rendering.
Cocos2d-x v2.0 upgraded to OpenGL ES 2.0 based. OpenGL ES 2.0 doesn't allocate power-of-two memory block for textures anymore, but the benefit of reducing file I/O times and draw calls are still working.

Then how about the animation? As we can see, sprite sheet has no MUST-BE relationship with animations. But considering to these benefits above, sprite sheet animations are efficient. There're different ways to create sprite sheet animations in cocos2d.

#### Creating from .png and .plist file
In cocos2d-x 0.x and 1.x versions, CCSpriteSheet works for this purpose. While CCSpriteBatchNode is a replacement of CCSpriteSheet since v2.0

A CCSpriteBatchNode object contains the actual image texture of all the sprite frames. You must add it to a scene, even though it won't draw anything itself; it just needs to be there so that it is part of the rendering pipeline. For example:

1CCSpriteBatchNode* spritebatch = CCSpriteBatchNode::create("animations/grossini.png");
Next, you need to use the CCSpriteFrameCache singleton to keep track how frame names correspond to frame bounds – that is, what rectangular area of the sprite sheet. Example:

```c++
1CCSpriteFrameCache* cache = CCSpriteFrameCache::sharedSpriteFrameCache();
2cache->addSpriteFramesWithFile("animations/grossini.plist");
```

Once your sprite sheet and frames are loaded, and the sprite sheet has been added to the scene, you can create sprites that use these frames by using the “createWithSpriteFrameName” method, and adding it as a child of the sprite sheet:

```c++
1m_pSprite1 = CCSprite::createWithSpriteFrameName("grossini_dance_01.png");
2spritebatch->addChild(m_pSprite1);
3addChild(spritebatch);
```

createWithSpriteFrameName method will find the corresponding coordinates and rectangle from grossini.plist, then "clip" the texture grossini.png to a sprite frame.
Now we need to create a CCArray object and add all frames of the animation to it. In the case of this animation, we know all 14 frames have the same size, so we can use a nested loop to iterate through them all, and break the loop when we finish adding the 14th frame.


```c++
 1CCArray* animFrames = CCArray::createWithCapacity(15);
 2
 3char str[100] = {0};
 4
 5for(int i = 1; i < 15; i++) 
 6{
 7    sprintf(str, "grossini_dance_%02d.png", i);
 8    CCSpriteFrame* frame = cache->spriteFrameByName( str );
 9    animFrames->addObject(frame);
10}
```

Finally, we need to create a CCAnimate action instance which we can run on the CCSprite. Below, we also wrap the CCAnimate action in a CCRepeatForever action that does what you would expect: repeats the animation forever,like so:

```c++
1CCAnimation* animation = CCAnimation::createWithSpriteFrames(animFrames, 0.3f);
2m_pSprite1->runAction( CCRepeatForever::create( CCAnimate::create(animation) ) );
```

#### File animation
CCAnimationCache can load a xml/plist file which well describes the batch node, sprite frame names and their rectangles. The interfaces are much easier to use.

```c++
1CCAnimationCache *cache = CCAnimationCache::sharedAnimationCache(); // "caches" are always singletons in cocos2d
2cache->addAnimationsWithFile("animations/animations-2.plist");
3CCAnimation animation = cache->animationByName("dance_1");  // I apologize for this method name, it should be getAnimationByName(..) in future versions
4CCAnimate animate = CCAnimate::create(animation);  // Don't confused between CCAnimation and CCAnimate :)
5sprite->runAction(animate);
```

Easy to use, isn't it?

### Skeleton Animation
Please refer to [Skeletal Animation](http://www.cocos2d-x.org/projects/cocos2d-x/wiki/Skeletal_Animation) page.

### Coment