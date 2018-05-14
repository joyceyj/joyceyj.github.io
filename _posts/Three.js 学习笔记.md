---
layout: post
title: Three.js学习笔记(一)
date: 2018-05-14
categories: 计算机图形学
tag: 计算机图形学

---

# Three.js 学习笔记（一）
## 一. 准备Three.js
下载three.js：https://github.com/mrdoob/three.js

在html文件中引用文件three.js

```
// 注意文件地址
<script src="/Users/admin/Documents/opengl/three.js-dev/build/three.js"></script>
```
## 二. 三大组建
在three.js中，要渲染物体到网页，需要三个组建：场景（scene），相机（camera）和渲染器（renderer）
### 场景scene
在three.js中，场景就只有一种，用THREE.Scene表示，场景时所有物体的容器。
创建一个场景：

```
scene = new THREE.Scene();
```
### 相机camera
相机决定了场景中那个角度的景色会显示出来。相机就像人的眼睛一样，人站在不同位置，抬头或者低头都能够看到不同的景色。

*场景只有一种，但是相机却又很多种*。和现实中一样，不同的相机确定了呈相的各个方面。比如有的相机适合人像，有的相机适合风景，专业的摄影师根据实际用途不一样，选择不同的相机。

对程序员来说，只要设置不同的**相机参数**，就能够让相机产生不一样的效果。
在Three.js中有多种相机，最常用的是透视相机（THREE.PerspectiveCamera），创建一个透视相机：

```
var camera = new THREE.PerspectiveCamera( 70, window.innerWidth / window.innerHeight, 0.01, 10 );
```
### 渲染器renderer
渲染器决定了渲染的结果应该画在页面的什么元素上面，并且以怎样的方式来绘制。定义了一个WebRenderer渲染器，代码如下所示：

```
// antialias平滑
renderer = new THREE.WebGLRenderer( { antialias: true } );
renderer.setSize( window.innerWidth, window.innerHeight );
document.body.appendChild( renderer.domElement );
```
## 三. 渲染物体
### 添加物体到场景中
```
// 创建一个立方体几何体
// 函数原型：CubeGeometry(width, height, depth, segmentsWidth, segmentsHeight, segmentsDepth, materials, sides)
var geometry = new THREE.CubeGeometry(1,1,1); 
// 设置立方体的材质和颜色
var material = new THREE.MeshBasicMaterial({color: 0x00ff00});
var cube = new THREE.Mesh(geometry, material); 
// 添加立方体到已创建的场景中
scene.add(cube);

// 创建一个盒子
// 效果为 https://jsfiddle.net/f2Lommf5/
geometry = new THREE.BoxGeometry( 0.2, 0.2, 0.2 );
material = new THREE.MeshNormalMaterial();
mesh = new THREE.Mesh( geometry, material );
scene.add( mesh );
```
### 渲染
渲染函数的原型如下：

```
render( scene, camera, renderTarget, forceClear )
```

各个参数的意义是：

* scene：前面定义的场景

* camera：前面定义的相机

* renderTarget：渲染的目标，默认是渲染到前面定义的render变量中

* forceClear：每次绘制之前都将画布的内容给清除，即使自动清除标志autoClear为false，也会清除。

### 渲染循环
渲染有两种方式：**实时渲染和离线渲染**。

离线渲染：是事先渲染好一帧一帧的图片，然后再把图片拼接成电影的。这就是离线渲染。如果不事先处理好一帧一帧的图片，那么电影播放得会很卡。CPU和GPU根本没有能力在播放的时候渲染出这种高质量的图片。

实时渲染：就是需要不停的对画面进行渲染，即使画面中什么也没有改变，也需要重新渲染。

下面就是一个渲染循环：

```
function render() {
	cube.rotation.x += 0.1;
	cube.rotation.y += 0.1;
	renderer.render(scene, camera);
	requestAnimationFrame(render);
}
```
其中一个重要的函数是requestAnimationFrame，这个函数就是让浏览器去执行一次参数中的函数，这样通过上面render中调用requestAnimationFrame()函数，requestAnimationFrame()函数又让render()再执行一次，就形成了我们通常所说的游戏循环了。
> requestAnimationFrame()函数实际上可以放到render函数中的任何一行，而不是说非要放到第一行，放在第一行，render中其他的代码就不能执行，这是错误的，requestAnimationFrame函数并不是一个return语句，并没有退出render函数的功能。 requestAnimationFrame函数表示*下一帧将执行render函数*，不是马上执行render函数的意思。


