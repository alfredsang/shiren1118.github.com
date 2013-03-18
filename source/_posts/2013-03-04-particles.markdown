---
layout: post
title: "[cocos2d-x wiki翻译]Particles"
date: 2013-03-04 16:58
comments: true
categories: 
---


# 粒子(Particles)
Particles
Introduction
Point vs Quad
CCParticleBatchNode
Creating a Quad Particle System
Gravity vs Radius mode
Gravity Mode
These properties are only valid in Gravity Mode:
Radius Mode
These properties are only valid in Radius Mode:
Properties common to all modes
Common properties of the particles:
Common properties of the system:
Examples
References
Comment
## Introduction
The term particle system refers to a computer graphics technique that uses a large number of very small sprites or other graphic objects to simulate certain kinds of "fuzzy" phenomena, which are otherwise very hard to reproduce with conventional rendering techniques - usually highly chaotic systems, natural phenomena, and/or processes caused by chemical reactions.


Point vs Quad
In early versions of Cocos2d-x, there were two types of particle system in cocos2d-x: Quad and Point particle system:

CCParticleSystemQuad
CCParticleSystemPoint
The CCParticleSystemQuad has these additional features that CCParticleSystemPoint doesn't support:

Spinning particles: particles can rotate around its axis. CCParticleSystemPoint ignores this property.
Particles can have any size. In CCParticleSystemPoint if the size is bigger than 64, it will be treated as 64.
The whole system can be scaled using the scale property.
As time goes on，we found that CCParticleSystemPoint is much slower than CCParticleSystemQuad.In addition,CCParticleSystemPoint does not support CCParticleBatchNode. As the result, CCParticleSystemPoint has been removed from cocos2d-x particle system.

CCParticleBatchNode
A CCParticleBatchNode can reference one and only one texture (one image file, one texture atlas). Only the CCParticleSystems that are contained in that texture can be added to the CCSpriteBatchNode. All CCParticleSystems added to a CCSpriteBatchNode are drawn in one OpenGL ES draw call. If the CCParticleSystems are not added to a CCParticleBatchNode then an OpenGL ES draw call will be needed for each one, which is less efficient.

Creating a Quad Particle System
1CCParticleSystemQuad* m_emitter = newCCParticleSystemQuad();
2m_emitter = CCParticleFire::create();
## Gravity vs Radius mode
Gravity Mode
Gravity Mode lets particles fly toward or away from a center point. Its strength is that it allows very dynamic, organic effects. You can set gravity mode with this line:

1// Gravity Mode
2this->m_nEmitterMode = kCCParticleModeGravity;
3
4// Gravity Mode: gravity
5this->modeA.gravity = ccp(0,-90);


These properties are only valid in Gravity Mode:
gravity (a CGPoint). The gravity of the particle system
speed (a float). The speed at which the particles are emitted
speedVar (a float). The speed variance.
tangencialAccel (a float). The tangential acceleration of the particles.
tangencialAccelVar (a float). The tangential acceleration variance.
radialAccel (a float). The radial acceleration of the particles.
radialAccelVar (a float). The radial acceleration variance.
Radius Mode
Radius Mode causes particles to rotate in a circle. It also allows you to create spiral effects with particles either rushing inward or orating outward. You set radius mode with this line:

1// Radius Mode
2this->m_nEmitterMode = kCCParticleModeRadius;
3
4// Radius Mode: startRadius
5this->modeB.startRadius = 0;
6this->modeB.startRadiusVar = 0;//ccp(0,0);


These properties are only valid in Radius Mode:
startRadius (a float). The starting radius of the particles
startRadiusVar (a float). The starting radius variance
endRadius (a float). The ending radius of the particles.
endRadiusVar (a float). The ending radius variance
rotatePerSecond (a float). Number of degress to rotate a particle around the source pos per second.
rotatePerSecondVar (a float). Number of degrees variance.
Properties common to all modes

## Common properties of the particles:

startSize: Start size of the particles in pixels
startSizeVar
endSize: Use kCCParticleStartSizeEqualToEndSize if you want that the start size == end size.
endSizeVar
startColor (a ccColor4F)
startColorVar (a ccColor4F)
endColor (a ccColor4F)
endColorVar (a ccColor4F)
startSpin. Only used in CCParticleSystemQuad
startSpinVar. Only used in CCParticleSystemQuad
endSpin. Only used in CCParticleSystemQuad
endSpinVar. Only used in CCParticleSystemQuad
life: time to live of the particles in seconds
lifeVar:
angle: (a float). Starting degrees of the particle
angleVar
positon: (a CGPoint)
posVar
centerOfGravity (a CGPoint)
Common properties of the system:
emissionRate (a float). How many particle are emitted per second
duration (a float). How many seconds does the particle system (different than the life property) lives. Use kCCParticleDurationInfinity for infity.
blendFunc (a ccBlendFunc). The OpenGL blending function used for the system
positionType (a tCCPositionType). Use kCCPositionTypeFree (default one) for moving particles freely. Or use kCCPositionTypeGrouped to move them in a group.
texture (a CCTexture2D). The texture used for the particles
## Examples
cocos2d-x comes with some predefined particles that can be customized in runtime. List of predefined particles:

CCParticleFire: Point particle system. Uses Gravity mode.
CCParticleFireworks: Point particle system. Uses Gravity mode.
CCParticleSun: Point particle system. Uses Gravity mode.
CCParticleGalaxy: Point particle system. Uses Gravity mode.
CCParticleFlower: Point particle system. Uses Gravity mode.
CCParticleMeteor: Point particle system. Uses Gravity mode.
CCParticleSpiral: Point particle system. Uses Gravity mode.
CCParticleExplosion: Point particle system. Uses Gravity mode.
CCParticleSmoke: Point particle system. Uses Gravity mode.
CCParticleSnow: Point particle system. Uses Gravity mode.
CCParticleRain: Point particle system. Uses Gravity mode.
References
Particle
Particle System in cocos2d

Last updated by Iven Yang at Updated about 1 month ago.


