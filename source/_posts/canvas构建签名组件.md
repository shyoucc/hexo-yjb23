---
title: canvas构建签名组件
date: 2017-07-14 22:43:03
tags:
- canvas
---

#### 需求

项目需求在webapp中插入一个类似合同的东西，所以避免不了手写签名，找了一圈网上的第三方库，没找到合适的，所以打算下手自己写一个，技术选择html5的canvas。

#### 分析
- 因为项目是放在移动端上，不考虑兼容普通pc浏览器的兼容性问题
- 要求比较简单，手指输入，图片导出

#### 创建canvas
此处可以对画布的宽高和画笔的一些基本属性做设置, point用来存放触摸的点信息

```javascript
let canvas = options.dom || document.getElementsByTagName('canvas')[0]
let ctx = canvas.getContext('2d')
ctx.lineWidth = options.lineWidth

let point = []
```

#### 绑定事件
画布监听了触摸屏的三个事件: `touchstart ` `touchmove ` `touchend ` :

```javascrit

   Uw.prototype._handleMove = function(e){
        e.preventDefault()
        // 获取发生变化的触点
        let touches = e.changedTouches
        let point = this.point
        let ctx = this.ctx
        let that = this

        for (let i = 0; i < touches.length; i++) {
            let id = this._getPointIndex(touches[i].identifier)
            ctx.beginPath()
            ctx.moveTo(point[id].pageX - that.range.left, point[id].pageY - that.range.top)
            ctx.lineTo(touches[i].pageX - that.range.left, touches[i].pageY - that.range.top)
            ctx.stroke()
            point.splice(id, 1, touches[i])
        }
    }

```
触发点击状态(start)把第一个起始点存放起来，之后(move)不断调用和覆盖,在最后触点离开(end)后清空,此处的range是dom偏移坐标的修正，`canvas.getBoundingClientRect()`来获取上下左右偏移

#### 获取
可以通过

```javascript
canvas.toDataURL(type, encoderOptions)

```
来获取base64地址，此方法通常可以传两个参数，第一个可选image/png(默认)或image/jpeg, 第二个参数可以设置jpegs质量(仅对image/jpeg时有效)

#### 最后
> [仓库地址](https://github.com/shyoucc/underwrite)








