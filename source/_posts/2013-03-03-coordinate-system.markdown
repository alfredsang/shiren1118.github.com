---
layout: post
title: "[cocos2d-x wiki翻译]Coordinate System(ok)"
date: 2013-03-03 17:05
comments: true
categories: [cocos2d-x wiki翻译]
---

Coordinate System·坐标系

- Introduction of Different Coordinate Systems
	- Cartesian Coordinate System
	- UI Coordinate System
	- Direct3D Coordinate System
	- OpenGL and Cocos2d Coordinate System
- Parent and Childrens
- Anchor Point
- getVisibleSize, getVisibleOrigin vs getWinSize
- How to convert co-ordinates
	- convertToNodeSpace：
	- convertToWorldSpace：
	- convertToWorldSpaceAR，
	- Sample code：
- References
- Comment

<div style='display:none;'>
## Cartesian Coordinate System
</div>

## 笛卡尔坐标系
<div style='display:none;'>
### Introduction of Different Coordinate Systems·
</div>
### 不同坐标系简介
<div style='display:none;'>
#### Cartesian Coordinate System·
</div>
#### 笛卡尔坐标系

<div style='display:none;'>
You probably have known "Cartesian Coordinate System" from school where it's heavily used in geometry lessons. If you have forgotten, these image will remind you quickly:
</div>

你可能上学的时候就已经知道“笛卡尔坐标系”了，它在几何课本里经常用到。如果你已经忘得差不多了，下面这些图片可以很快唤起你的记忆：

![](http://www.cocos2d-x.org/attachments/download/1561)


<div style='display:none;'>
There're 3 types of coordinate system that you will meet in mobile games development.
</div>

在移动游戏开发过程中，有三种类型的坐标系你可能遇到：

<div style='display:none;'>
#### UI Coordinate System·
</div>
#### UI坐标系

<div style='display:none;'>
 In common UI Coordinates on iOS/Android/Windows SDK:

- The origin (x=0, y=0) is at the top-left corner.
- X axis starts at the left side of the screen and increase to the right;
- Y coordinates start at the top of the screen and increase downward,
</div>

iOS/Android/Windows SDK中的通用UI坐标系：

- 起点坐标(x=0, y=0)位于左上角
- X轴从屏幕最左边开始，由左向右渐增
- Y轴坐标从屏幕最上方开始，由上向下渐增

<div style='display:none;'>
looks like this
</div>
详见下图

![](http://www.cocos2d-x.org/attachments/download/1564)


<div style='display:none;'>
	#### Direct3D Coordinate System·
</div>	
#### Direct3D坐标系

<div style='display:none;'>
DirectX uses Left-handed Cartesian Coordinate.
</div>

DirectX 使用Left-handed Cartesian Coordinate
<div style='display:none;'>
	#### OpenGL and Cocos2d Coordinate System
</div>

#### OpenGL和Cocos2d坐标系

<div style='display:none;'>
Cocos2d-x/-html5/-iphone uses the same coordinate system as OpenGL, which is so called "Right-handed Cartesian Coordinate System".
</div>

Cocos2d-x/-html5/-iphone使用的坐标系和OpenGL的坐标系一样，名为“Right-handed Cartesian Coordinate Syste”。

![](http://www.cocos2d-x.org/attachments/download/1563)


<div style='display:none;'>
We only use x-axis & y-axis in 2D world. So in your cocos2d games:

- The origin (x=0, y=0) is in the bottom-left corner of screen, which means the screen is in the first quartile of right-handed cartesian coordinate system,
- X axis starts at the left side of the screen and increase to the right;
- Y axis starts at the bottom of the screen and increase upwards.
</div>

在2D世界中，我们仅会使用x轴和y轴。所以在你的cocos2d游戏中：

- 起点坐标(x=0, y=0)位于右下角，这意味着屏幕位于
- X轴从屏幕最左边开始，由左向右渐增
- Y轴坐标从屏幕最下方开始，由下向上渐增

<div style='display:none;'>
And here’s a picture that helps illustrate Cocos2d-x Coordinates a bit better:
</div>
	
下面这张图片有助于更好的阐述Cocos2d-x坐标：
	
![](http://www.cocos2d-x.org/attachments/1556/SpriteCoordinates.jpg)

<div style='display:none;'>
Note that it's different from common UI coordinate systems and DirectX coordinate systems.
</div>

一定要注意：通用UI坐标系和DirectX坐标系是不一样的。

### Parent and Childrens

<div style='display:none;'>
Every class derived from CCNode (Ultimate cocos2d-x class) will have a anchorPoint property.
When determining where to draw the object (sprite, layer, whatever), cocos2d-x will combine both properties position and anchorPoint. Also, when rotating an object, cocos2d-x will rotate it around its anchorPoint.
</div>

由于每个类都继承自CCNode（cocos2d-x的最顶层类），所以每个类都会默认有anchorPoint属性。
当我们在一个位置画一个的对象的时候，cocos2d-x会合并属性位置和anchorPoint。当然，当旋转一个对象时，cocos2d-x会围绕绕anchorPoint旋转的。

<div style='display:none;'>
We create a sprite as a gray parent and another sprite as blue child.Set parent's position to ccp(100,100),child's anchor point in the center of circle .
</div>

我们创建一个灰色父对象和一个蓝色子对象。设置父对象位置是ccp(100,100),子对象的anchor point位于圆心。


```c++
CCSprite* parent = CCSprite::create("parent.png");
parent->setAnchorPoint(ccp(0, 0));// Anchor Point
parent->setPosition(ccp(100, 100));
parent->setAnchorPoint(ccp(0, 0));
addChild(parent);

//create child 
CCSprite* child = CCSprite::create("child.png");
child->setAnchorPoint(ccp(0.5, 0.5));
child->setPosition(ccp(0, 0));
parent->addChild(child);//add child sprite into parent sprite.
```

<div style='display:none;'>
Although we set child's position of ccp(0,0),parent's position is ccp(100,100).Therefore,child's position is :
</div>

由于我们设置子对象的位置是ccp(0,0)，父对象位置是ccp(100,100)。所以，子对象位置是：

![](http://www.cocos2d-x.org/attachments/1559/parent.jpeg）

<div style='display:none;'>
### Anchor Point·
</div>
### 锚点

<div style='display:none;'>
As a example, this sprite has an anchorPoint of ccp(0,0) and a position of ccp(0,0). 
</div>

作为例子，下面这个精灵有的锚点位于 ccp(0,0)，位置位于ccp(0,0)。

![](http://www.cocos2d-x.org/attachments/1545/bottomleft.png)


<div style='display:none;'>
This rectangle sprite will be placed at the bottom left corner of its parent, the layer.

example:
</div>

这个矩形精灵将被放到它的父对象（layer）的左下角。

示例：

```c++
// create sprite
CCSprite* sprite = CCSprite::create("bottomleft.png");
sprite->setAnchorPoint(ccp(0, 0));// Anchor Point
sprite->setPosition(ccp(0,0));
addChild(sprite);
```


![](http://www.cocos2d-x.org/attachments/1557/anchor_left.png)


<div style='display:none;'>
In another example, we will assign a anchorPoint of ccp(0.5,0.5) to better understand the relative value of the anchor point.
</div>

在另一个例子中，我们会摆放一个坐标为ccp(0.5,0.5)的anchorPoint，以便您更好的理解锚点的相对值。


![](http://www.cocos2d-x.org/attachments/1546/center.png)

```c++
// create sprite
CCSprite* sprite = CCSprite::create("center.png");
sprite->setAnchorPoint(ccp(0.5, 0.5));// Anchor Point
sprite->setPosition(ccp(0,0));
addChild(sprite);
```

![](http://www.cocos2d-x.org/attachments/1553/anchor_center.png)

<div style='display:none;'>
As you can see in the image, the anchor point is not a pixel value. The value of X and Y are relative to the size of the node.
</div>

正如你从图中看出的，锚点取的不是像素值，此值的X和Y是相对于此节点的大小的。

<div style='display:none;'>
### getVisibleSize, getVisibleOrigin vs getWinSize
</div>
### 获取可视区域大小, 获取可视区域起点 vs 获取窗口大小

- 获取可视区域大小, http://www.proxyee.com/sohu.php?u=g6QXlZ4DNWBAxCi9G6VfFZDcGz9wRSMxPcWDz6pirP7qxzcikzUMuwFBVm%2BmUfs%2F%2Bggq3kMo%2Fb%2FjyNLgzuR7KoJwBHGwiTD8HSw9yZSdW%2FifzOs%3D&b=3#a7cc45ff42a969700f878bb2485adf3b1
- 获取可视区域起点 http://www.proxyee.com/sohu.php?u=g6QXlZ4DNWBAxCi9G6VfFZDcGz9wRSMxPcWDz6pirP7qxzcikzUMuwFBVm%2BmUfs%2F%2Bggq3kMo%2Fb%2FjyNLgzuR7KoJwBHGwiTD8HSw9yZSdW%2FifzOs%3D&b=3#af991a412cb6621bf25ec655a95deddaa
- 获取窗口大小 http://www.proxyee.com/sohu.php?u=g6QXlZ4DNWBAxCi9G6VfFZDcGz9wRSMxPcWDz6pirP7qxzcikzUMuwFBVm%2BmUfs%2F%2Bggq3kMo%2Fb%2FjyNLgzuR7KoJwBHGwiTD8HSw9yZSdW%2FifzOs%3D&b=3#aa78f85a3666553d0d4fe73118e0c82ac

<div style='display:none;'>
VisibleSize returns visible size of the OpenGL view in points.The value is equal to getWinSize if don't invoke CCEGLView::setDesignResolutionSize().
getVisibleOrigin returns visible origin of the OpenGL view in points. Please refer to [Multi resolution support](Multi resolution support) for more details
</div>

VisibleSize（可视区域大小）会返回此点的OpenGL视图的可视区域大小。如果没有调用CCEGLView::setDesignResolutionSize()的话，此值等于getWinSize的大小。
getVisibleOrigin（获取可视区域起点）会返回此点的OpenGL视图的可视区域起点。请移步[Multi resolution support](Multi resolution support)查看详情。

<div style='display:none;'>
### How to convert co-ordinates·
</div>

### 如何转换坐标

#### convertToNodeSpace：

<div style='display:none;'>
convertToNodeSpace will be used in, for example, tile-based games, where you have a big map. convertToNodeSpace will convert your openGL touch co-ordinates to the co-ordinates of the .tmx map or anything similar.

Example:

The following picture shows that we have node1 with anchor point (0,0) and node2 with anchor point (1,1).

We invoke CCPoint point = node1->convertToNodeSpace(node2->getPosition()); convert node2's SCREEN coords to node1's local.As the result,node2 with the position of (-25，-60).
</div>
举例，convertToNodeSpace用于tile-based的游戏，即有一个大地图。convertToNodeSpace会转换openGL触摸点转成.tmx 地图或者其他近似的坐标。

例子：

下面的图片会展现，node1的锚点(0,0)，node2的锚点是(1,1)。

我们会调用CCPoint point = node1->convertToNodeSpace(node2->getPosition()); 转换node2的屏幕坐标为node1的位置。结果是，node2的位置是(-25，-60).

![](http://www.cocos2d-x.org/attachments/download/1783)

#### convertToWorldSpace：

<div style='display:none;'>
convertToWorldSpace(const CCPoint& nodePoint) converts on-node coords to SCREEN coordinates.convertToWorldSpace will always return SCREEN position of our sprite, might be very useful if you want to capture taps on your sprite but need to move/scale your layer

Example:
</div>

convertToWorldSpace(常量 CCPoint& nodePoint) 转换node坐标为SCREEN坐标。convertToWorldSpace会经常返回你的精灵的SCREEN位置，如果你想捕获精灵的taps而且需要移动/缩放layer的时候，这可能非常有帮助。

```c++
CCPoint point = node1->convertToWorldSpace(node2->getPosition()); 
```

<div style='display:none;'>
the above code will convert the node2‘s coordinates to the coordinates on the screen.

For example if the position of node2 is (0, 0) which will be the bottom left corner of the node1, but not necessarily on the screen. This will convert (0, 0) of the node2 to the position of the screen(in this case is (15,20)). The result shows in the following picture:
</div>

上面的代码会转换node2坐标为node2在屏幕上对应的坐标。

![](http://www.cocos2d-x.org/attachments/download/1784)


####  convertToWorldSpaceAR

<div style='display:none;'>
convertToWorldSpaceAR will return the position relatevely to anchor point: so if our scene - root layer has Anchor Point of ccp(0.5f, 0.5f) - default, convertToWorldSpaceAR should return position relatively to screen center.

convertToNodeSpaceAR - the same logic as for .convertToWorldSpaceAR
</div>

convertToWorldSpaceAR返回相对锚点的位置：所以如果你的场景 - 根layer有一个锚点位于ccp(0.5f, 0.5f)。- 默认的，convertToNodeSpaceAR应返回相对于屏幕中心的位置。

convertToNodeSpaceAR - 和convertToWorldSpaceAR是一样的逻辑。

#### 示例代码：
```c++
CCSprite *sprite1 = CCSprite::create("CloseNormal.png");

sprite1->setPosition(ccp(20,40));

sprite1->setAnchorPoint(ccp(0,0));

this->addChild(sprite1);

CCSprite *sprite2 = CCSprite::cteate("CloseNormal.png");

sprite2->setPosition(ccp(-5,-20));

sprite2->setAnchorPoint(ccp(1,1));

this->addChild(sprite2);

CCPoint point1 = sprite1->convertToNodeSpace(sprite2->getPosition());

CCPoint point2 = sprite1->convertToWorldSpace(sprite2->getPosition());

CCPoint point3 = sprite1->convertToNodeSpaceAR(sprite2->getPosition());

CCPoint point4 = sprite1->convertToWorldSpaceAR(sprite2->getPosition());

CCLog("position = (%f,%f)",point1.x,point1.y);

CCLog("position = (%f,%f)",point2.x,point2.y);

CCLog("position = (%f,%f)",point3.x,point3.y);

CCLog("position = (%f,%f)",point4.x,point4.y);
```

结果：

	position = (-25.000000,-60.000000)
	position = (15.000000,20.000000)
	position = (-25.000000,-60.000000)
	position = (15.000000,20.000000)

### 参考

- [Coordinate Systems (Direct3D 9) Windows](http://www.proxyee.com/sohu.php?u=g6QXj5oQdS1CziS8RrIdC8qdCjcyGDw%2FddSIjqVu4eLqwSd7gXEBrx4ZQSLtUL8yoQQ2kFI5rOC0n5Thgs13A8ABXxv6%2BDXmHzE%3D&b=3) from Microsoft MSDN
- [How to make a simple iphone game with cocos2d tutorial](http://www.proxyee.com/sohu.php?u=g6QXlZ4DNXFO3jCrR6UXH9LaCjBxVCk6d4TTk%2BZv7OemxzF5m3kErlwPH37gWLw1sEYvz1g08LWtwMC%2Fz5Y9HMdHSk28tTvmXS1zyY6bGuKCwOs%3D&b=3) written by Ray Wenderlich
- 如何使用cocos2d制作一个简单的iphone游戏
- [Coordinate Systems of cocos2dx](http://www.proxyee.com/sohu.php?u=g6QXgIUbfC0ekXTgSq4fQsTZDwcrWGk1NNiBjrpz4uTi0HFmxilb%2BUheBDy7Bf1r7Fl3hgNjq%2BW0npn9&b=3)
- cocos2dx坐标系

### Comment