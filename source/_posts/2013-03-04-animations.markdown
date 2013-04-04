---
layout: post
title: "[cocos2d-x wiki翻译]Animations"
date: 2013-03-04 16:57
comments: true
categories: [cocos2d-x wiki翻译]
---


## 动画(Animations)
<div style='display:none;'>
Animations
Manual Animation
Sprite Sheet Animation
Creating from .png and .plist file
File animation
Skeleton Animation
Comment
</div>
### Frame Animation（帧动画）

<div style='display:none;'>
You can create an animation from a series of image file, like this:
</div>
你可以通过多张图片文件来创建一个动画，比如:

```
 CCAnimation *animation = CCAnimation::create();
 
 // load image file from local file system to CCSpriteFrame, then add into CCAnimation
 for (int i = 1; i < 15; i++)
 {
     char szImageFileName[128] = {0};
     sprintf(szImageFileName, "Images/grossini_dance_%02d.png", i);
     animation->addSpriteFrameWithFileName(szImageFileName);  
 }

animation->setDelayPerUnit(2.8f / 14.0f); // This animation contains 14 frames, will continuous 2.8 seconds.
animation->setRestoreOriginalFrame(true); // Return to the 1st frame after the 14th frame is played. 

CCAnimate *action = CCAnimate::create(animation);
sprite->runAction(action);  // run action on sprite object
```
<div style='display:none;'>
Note that CCAnimation is composed by sprite frames, delay time per frame, durations etc, it's a pack of "data". While CCAnimate is an action, which is created base on CCAnimation object.
</div>

注意CCAnimation是由sprite frame帧组、单个frame延时，持续时间等组成的，它是一组"数据".
而CCAnimate是一个action，它是基于CCAnimation对象创建的。

### Sprite Sheet Animation

<div style='display:none;'>
Although manual animation is very easy to understand, it's rarely used in real game projects. Instead, sprite sheet animation is the common solution of 2D animations.
</div>

尽管帧动画是非常易于理解的,但事实上，它在真实游戏项目中是非常少用到的.相反，sprite sheet animation是2D动画中最常见的解决方案.

<div style='display:none;'>
This is a typical sprite sheet. It can be a sequence of sprite frames for an animation, or can be images pack that will be used in a same scene.
</div>

下面是一个典型的sprite sheet.它是动画中的一连串sprite frames，或者是可以在一个场景中用到的一组图片.


![](http://www.cocos2d-x.org/attachments/download/1570)
<div style='display:none;'>
In OpenGL ES 1.1 period, sprite sheets was widely used for these benefits:
Reduce times of file I/O. Loading a big sprite sheet file is faster than loading lots of small files.
Reduce the memory consumption. OpenGL ES 1.1 can only use power-of-two sized textures (that is a width or height of 2,4,864,128,256,512,1024,...). In other words, OpenGL ES 1.1 allocates power-of-two sized memory for each texture even if this texture has smaller width and height. So using packed sprite sheet image will reduce the fragments of memory.
Reduce the draw calls to OpenGL ES draw method and speed up rendering.
</div>

在OpenGL ES 1.1时代，sprite sheets被广泛使用是得益于:

1. 减少文件I/O时间.加载一个大的sprite sheet文件比加载多个小文件要快得多.
2. 减少内存消耗.OpenGL ES 1.1仅可用2幂次方大小的texture（通常为 2,4,864,128,256,512,1024,...的宽高）.换言之，如果宽高比较小的话，OpenGL ES 1.1甚至为每个texture分配2幂次方大小内存.所以使用成组的 sprite sheet图片会减少内存碎片.
3. 减少 OpenGL ES 调用方法绘制，加速渲染.

<div style='display:none;'>
Cocos2d-x v2.0 upgraded to OpenGL ES 2.0 based. OpenGL ES 2.0 doesn't allocate power-of-two memory block for textures anymore, but the benefit of reducing file I/O times and draw calls are still working.
</div>
Cocos2d-x v2.0已更新，是基于OpenGL ES 2.0的.OpenGL ES 2.0不再为textures分配2的幂次方内存块，但减少文件系统I/O和绘图调用时间的好处仍然是有效的.

<div style='display:none;'>
Then how about the animation? As we can see, sprite sheet has no MUST-BE relationship with animations. But considering to these benefits above, sprite sheet animations are efficient. There're different ways to create sprite sheet animations in cocos2d.
</div>

那该如何动画呢？正如我们看到的，sprite sheet与动画没有的必然关系.但考虑到上面这些益处，sprite sheet是有效的.在cocos2d中有很多种不同的方法来创建sprite sheet.

#### Creating from .png and .plist file

<div style='display:none;'>
In cocos2d-x 0.x and 1.x versions, CCSpriteSheet works for this purpose. While CCSpriteBatchNode is a replacement of CCSpriteSheet since v2.0
</div>

在cocos2d-x 0.x和1.x版本中，CCSpriteSheet便是为此目的而生的.而CCSpriteBatchNode在v2.0版本之后替换了CCSpriteSheet


<div style='display:none;'>
A CCSpriteBatchNode object contains the actual image texture of all the sprite frames. You must add it to a scene, even though it won't draw anything itself; it just needs to be there so that it is part of the rendering pipeline. For example:
</div>

CCSpriteBatchNode对象包含了所有sprite frames中用到的真实图片texture.你必须在场景中添加它，甚至它自身什么都不用画;它只需要放到那里，这样它就成了rendering pipeline的组成部分.比如:

<div style='display:none;'>
1CCSpriteBatchNode* spritebatch = CCSpriteBatchNode::create("animations/grossini.png");
Next, you need to use the CCSpriteFrameCache singleton to keep track how frame names correspond to frame bounds – that is, what rectangular area of the sprite sheet. Example:
</div>

```
CCSpriteBatchNode* spritebatch = CCSpriteBatchNode::create("animations/grossini.png");
```

接下来，你需要使用CCSpriteFrameCache单例对象来保存frame名字如何对应到frame bounds——
也就是，sprite sheet中的矩形区域.例如:

```
CCSpriteFrameCache* cache = CCSpriteFrameCache::sharedSpriteFrameCache();
cache->addSpriteFramesWithFile("animations/grossini.plist");
```

<div style='display:none;'>
Once your sprite sheet and frames are loaded, and the sprite sheet has been added to the scene, you can create sprites that use these frames by using the “createWithSpriteFrameName” method, and adding it as a child of the sprite sheet:
</div>
一旦你的sprite sheet和frames已经加载完毕，并且sprite sheet已经增加到场景中了，你可以通过这些frame创建sprite，使用 “createWithSpriteFrameName” 方法，把它添加为sprite sheet的子对象:

```
m_pSprite1 = CCSprite::createWithSpriteFrameName("grossini_dance_01.png");
spritebatch->addChild(m_pSprite1);
addChild(spritebatch);
```

<div style='display:none;'>
createWithSpriteFrameName method will find the corresponding coordinates and rectangle from grossini.plist, then "clip" the texture grossini.png to a sprite frame.
</div>

createWithSpriteFrameName方法会查找相应坐标和grossini.plist中定义的矩形，接着"裁剪"texture grossini.png到sprite frame.


<div style='display:none;'>
Now we need to create a CCArray object and add all frames of the animation to it. In the case of this animation, we know all 14 frames have the same size, so we can use a nested loop to iterate through them all, and break the loop when we finish adding the 14th frame.
</div>

现在，我们需要创建CCArray对象，增加动画中的所有frame到此对象中。在这个动画例子中，我们知道这14个frame都有一模一样的尺寸,所以我们使用了嵌套循环来迭代，当我们完成增加第14个frame的时候中断循环.

```c++
CCArray* animFrames = CCArray::createWithCapacity(15);

char str[100] = {0};

for(int i = 1; i < 15; i++) 
{
    sprintf(str, "grossini_dance_%02d.png", i);
    CCSpriteFrame* frame = cache->spriteFrameByName( str );
    animFrames->addObject(frame);
}
```

<div style='display:none;'>
Finally, we need to create a CCAnimate action instance which we can run on the CCSprite. Below, we also wrap the CCAnimate action in a CCRepeatForever action that does what you would expect: repeats the animation forever,like so:
</div>

最后，我们需要创建CCAnimate动作实例，该实例能在CCSprite对像上运作.下面，我们还将CCAnimate动作封装到CCRepeatForever动作中，此动作正是你所需要的:重复动画,像这样:

```
CCAnimation* animation = CCAnimation::createWithSpriteFrames(animFrames, 0.3f);
m_pSprite1->runAction( CCRepeatForever::create( CCAnimate::create(animation) ) );
```

#### File animation

<div style='display:none;'>
CCAnimationCache can load a xml/plist file which well describes the batch node, sprite frame names and their rectangles. The interfaces are much easier to use.
</div>

CCAnimationCache 可以加载xml/plist文件，此文件可以非常好的描述批量node,sprite frame names和它们的矩形.
这个接口更简单易用.

```
CCAnimationCache *cache = CCAnimationCache::sharedAnimationCache(); // "caches" are always singletons in cocos2d
cache->addAnimationsWithFile("animations/animations-2.plist");
CCAnimation animation = cache->animationByName("dance_1");  // I apologize for this method name, it should be getAnimationByName(..) in future versions
CCAnimate animate = CCAnimate::create(animation);  // Don't confused between CCAnimation and CCAnimate :)
sprite->runAction(animate);
```

<div style='display:none;'>
Easy to use, isn't it?
</div>

简单易用吧？哈哈

### 骨骼动画(Skeleton Animation)
请参考 [Skeletal Animation](http://www.cocos2d-x.org/projects/cocos2d-x/wiki/Skeletal_Animation) 页.

### 评论