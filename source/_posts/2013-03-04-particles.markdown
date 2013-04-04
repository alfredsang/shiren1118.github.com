---
layout: post
title: "[cocos2d-x wiki翻译]Particles"
date: 2013-03-04 16:58
comments: true
categories: 
---


# 粒子(Particles)
## 简介

<div style='display:none;'>
The term particle system refers to a computer graphics technique that uses a large number of very small sprites or other graphic objects to simulate certain kinds of "fuzzy" phenomena, which are otherwise very hard to reproduce with conventional rendering techniques - usually highly chaotic systems, natural phenomena, and/or processes caused by chemical reactions.
</div>

术语粒子系统关系到计算机图形学技术，使用大量非常小的sprite或其他图像对象来模拟特定种类的"模糊"现象,通过常规的渲染技术是很难制造的——一般来说有混乱系统、自然现象、或者活学反应引起的过程。

![](http://www.cocos2d-x.org/attachments/download/1631)

## Point vs Quad

<div style='display:none;'>
In early versions of Cocos2d-x, there were two types of particle system in cocos2d-x: Quad and Point particle system:
</div>

在Cocos2d-x早期版本中,Cocos2d-x里粒子系统有两种类型:Quad 和 Point粒子系统:

- CCParticleSystemQuad
- CCParticleSystemPoint

<div style='display:none;'>
The CCParticleSystemQuad has these additional features that CCParticleSystemPoint doesn't support:
</div>

CCParticleSystemQuad有很多特性是CCParticleSystemPoint不支持的:
<div style='display:none;'>
- Spinning particles: particles can rotate around its axis. CCParticleSystemPoint ignores this property.
- Particles can have any size. In CCParticleSystemPoint if the size is bigger than 64, it will be treated as 64.
- The whole system can be scaled using the scale property.
</div>
- Spinning particles:粒子可以绕自身的轴旋转。CCParticleSystemPoint忽略了该属性。
- 粒子可以为任意大小。在CCParticleSystemPoint中，如果大小大于64时，就视为64。
- 通过scale属性可以缩放整个系统。

<div style='display:none;'>
Because of CCParticleSystemPoint does not support CCParticleBatchNode. As the result, CCParticleSystemPoint has been removed from cocos2d-x particle system.
</div>

由于CCParticleSystemPoint不支持CCParticleBatchNode.所以,CCParticleSystemPoint已经从cocos2d-x粒子系统里移除了.


## CCParticleBatchNode

<div style='display:none;'>
A CCParticleBatchNode can reference one and only one texture (one image file, one texture atlas). Only the CCParticleSystems that are contained in that texture can be added to the CCSpriteBatchNode. All CCParticleSystems added to a CCSpriteBatchNode are drawn in one OpenGL ES draw call. If the CCParticleSystems are not added to a CCParticleBatchNode then an OpenGL ES draw call will be needed for each one, which is less efficient.
</div>

CCParticleBatchNode可以引用且只可以引用1个texture(一个图片文件，一个texture图集).只有CCParticleSystem可以增加到CCSpriteBatchNode中.
所有增加到CCSpriteBatchNode中的CCParticleSystem是在OpenGL ES调用绘图函数时绘制的.
如果CCParticleSystem没有增加到CCParticleBatchNode中，OpenGL ES会调用每个粒子系统的绘图函数，这样做是低效的.

### 创建Quad粒子系统

```
CCParticleSystemQuad* m_emitter = newCCParticleSystemQuad();
m_emitter = CCParticleFire::create();
```

## 重力和半径模式

### 重力模式 Gravity Mode
<div style='display:none;'>
Gravity Mode lets particles fly toward or away from a center point. Its strength is that it allows very dynamic, organic effects. You can set gravity mode with this line:
</div>

重力模式让粒子围绕一个中心点移近或移远.它的优点是非常动态，有组织有规则的效果.你可以通过下面这行代码设置重力模式:

```
// Gravity Mode
this->m_nEmitterMode = kCCParticleModeGravity;

// Gravity Mode: gravity
this->modeA.gravity = ccp(0,-90);
```

<div style='display:none;'>
These properties are only valid in Gravity Mode:
</div>

**下面这些属性只在重力模式下支持:**
<div style='display:none;'>
- gravity (a CGPoint). The gravity of the particle system
- speed (a float). The speed at which the particles are emitted
- speedVar (a float). The speed variance.
- tangencialAccel (a float). The tangential acceleration of the particles.
- tangencialAccelVar (a float). The tangential acceleration variance.
- radialAccel (a float). The radial acceleration of the particles.
- radialAccelVar (a float). The radial acceleration variance.
</div>
- 重力（CGPoint）。粒子系统的重力。
- 速度（float）。粒子发射时的速度。
- 速度变量speedVar(float)。速度的变异数。
- tangencialAccel(float)。粒子的正切加速度。
- tangencialAccelVar(float)。粒子正切加速度的差异数。
- radialAccel(float)。粒子的径向加速度。
- radialAccelVar(float)。粒子径向加速度的差异数。

### 半径模式 Radius Mode
<div style='display:none;'>
Radius Mode causes particles to rotate in a circle. It also allows you to create spiral effects with particles either rushing inward or orating outward. You set radius mode with this line:
</div>
半径模式会使粒子以圆圈方式旋转.它也可以创造螺旋效果让粒子急速前进或后退.你可以通过下面这行代码设置半径模式:

```
// Radius Mode
this->m_nEmitterMode = kCCParticleModeRadius;

// Radius Mode: startRadius
this->modeB.startRadius = 0;
this->modeB.startRadiusVar = 0;//ccp(0,0);
```
<div style='display:none;'>
These properties are only valid in Radius Mode:
</div>
**下面这些属性只在半径模式下支持:**

<div style='display:none;'>
- startRadius (a float). The starting radius of the particles
- startRadiusVar (a float). The starting radius variance
- endRadius (a float). The ending radius of the particles.
- endRadiusVar (a float). The ending radius variance
- rotatePerSecond (a float). Number of degress to rotate a particle around the source pos per second.
- rotatePerSecondVar (a float). Number of degrees variance.
</div>
- startRadius(float)。粒子开始时的半径。
- startRadiusVar(float)。粒子开始时的半径变异数。
- endRadius(float)。粒子结束时的半径。
- endRadiusVar(float)。结束时粒子的半径变异数。
- rotatePerSecond(float)。粒子围绕原点每秒旋转的度数。
- rotatePerSecondVar(float)。度数的变异数。

## 各模式下的通用属性
### 粒子通用属性：
<div style='display:none;'>
- startSize: Start size of the particles in pixels
- startSizeVar
- endSize: Use kCCParticleStartSizeEqualToEndSize if you want that the start size == end size.
- endSizeVar
- startColor (a ccColor4F)
- startColorVar (a ccColor4F)
- endColor (a ccColor4F)
- endColorVar (a ccColor4F)
- startSpin. Only used in CCParticleSystemQuad
- startSpinVar. Only used in CCParticleSystemQuad
- endSpin. Only used in CCParticleSystemQuad
- endSpinVar. Only used in CCParticleSystemQuad
- life: time to live of the particles in seconds
- lifeVar:
- angle: (a float). Starting degrees of the particle
- angleVar
- positon: (a CGPoint)
- posVar
- centerOfGravity (a CGPoint)
</div>

- startSize: 开始时粒子的像素大小
- startSizeVar
- endSize: 如果你愿意的话使用kCCParticleStartSizeEqualToEndSize，这样开始大小 == 结束大小
- endSizeVar
- startColor开始颜色 (ccColor4F)
- startColorVar (ccColor4F)
- endColor结束颜色 (ccColor4F)
- startSpin开始的旋转角度。只用于CCParticleSystemQuad
- startSpinVar.只用于CCParticleSystemQuad
- endSpin结束时的旋转角度。只用于CCParticleSystemQuad
- endSpinVar。只用于CCParticleSystemQuad
- life：粒子存活的时间，单位秒
- lifeVar：
- angle：(float)。粒子发射时的角度
- angleVar
- positon：(CGPoint)
- posVar
- centerOfGravity(CGPoint)重力中心点

### 粒子系统通用属性：
<div style='display:none;'>
- emissionRate (a float). How many particle are emitted per second
- duration (a float). How many seconds does the particle system (different than the life property) lives. Use kCCParticleDurationInfinity for infity.
- blendFunc (a ccBlendFunc). The OpenGL blending function used for the system
- positionType (a tCCPositionType). Use kCCPositionTypeFree (default one) for moving particles freely. Or use kCCPositionTypeGrouped to move them in a group.
- texture (a CCTexture2D). The texture used for the particles
</div>

- emissionRate (float)。每秒发射多少粒子。
- duration (float)。粒子系统可以存活多少秒（与life属性不同）。使用kCCParticleDurationInfinity为无限时间。
- blendFunc (ccBlendFunc)。系统中使用的OpenGL混合方法。
- positionType (CCPositionType)。粒子自由移动使用kCCPositionTypeFree（默认）。或者使用kCCPositionTypeGrouped，使粒子在集合中移动。
- texture纹理(CCTexture2D)。粒子使用的纹理。

## 示例

<div style='display:none;'>
cocos2d-x comes with some predefined particles that can be customized in runtime. List of predefined particles:
</div>

cocos2d-x里内置的预制粒子是可以在运行时自定义的.内置粒子列表:

- CCParticleFire: Point particle system. 使用重力模式.
- CCParticleFireworks: Point particle system. 使用重力模式.
- CCParticleSun: Point particle system. 使用重力模式.
- CCParticleGalaxy: Point particle system. 使用重力模式.
- CCParticleFlower: Point particle system. 使用重力模式.
- CCParticleMeteor: Point particle system. 使用重力模式.
- CCParticleSpiral: Point particle system. 使用重力模式.
- CCParticleExplosion: Point particle system. 使用重力模式.
- CCParticleSmoke: Point particle system. 使用重力模式.
- CCParticleSnow: Point particle system. 使用重力模式.
- CCParticleRain: Point particle system. 使用重力模式.

## 参考

[粒子](http://en.wikipedia.org/wiki/Particles)
[cocos2d中的粒子系统](http://www.cocos2d-iphone.org/wiki/doku.php/prog_guide:particles)
