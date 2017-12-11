---
title: WebGL从入门到放弃02
date: 2017-11-28 19:26:46
tags:
---

还好我没放弃。。之前的demo01 成功在canvas区域中间绘制了一个点，demo02 的目标是点击鼠标绘制一个点。

<!-- more -->

- shader,相比上次多了一个变量

```c++
    //顶点着色器
    attribute vec4 _Pos;
    void main(){
        gl_Position = _Pos;
        gl_PointSize = 10.0;
    }
    //片元着色器
      void main(){
        gl_FragColor = vec4(1.0, 0.0, 0.0, 1.0);
    }    
```

这段shader就是先声明一个变量 ，将这个变量赋值给`gl_Position`

中间的编译shader的过程与demo01 一样，下面是核心代码

```javascript
 const rect = e.target.getBoundingClientRect();
 const x = ((mouse.x - rect.left) - canvas.width / 2) / (canvas.width / 2);
 const y = (canvas.height / 2 - (mouse.y - rect.top)) / (canvas.height / 2);
 points.push(x,y);
 //每次绘制会清空颜色缓冲区 
 for (let index = 0; index < points.length; index+=2) {
     gl.vertexAttrib3f(_Pos, points[index], points[index+1], 0.0);//将数据绑定在这个变量上
     gl.drawArrays(gl.POINTS, 0, 1);
 }
```

这段代码使用了`gl.vertexAttrib3f()`函数进行数据绑定，将经过归一化的坐标传给`gl_Position`(_Pos)

补充：

WebGL函数说明：

```javascript
//功能：清空canvas
//说明：这个函数用于清空颜色缓冲区，参数gl.COLOR_BUFFER_BIT表示清空颜色缓冲区，这个参数还可以为gl.DEPTH_COLOR_BIT，深度缓冲区，模板缓冲区(不常用)
gl.clear(gl.COLOR_BUFFER_BIT);

//功能：绘制操作
//说明：执行顶点着色器,mode,指定绘制方式，可接受的值为：gl.POINTS,gl.LINES,gl.LINE_STRIP,gl.LINE_LOOP,gl.TRIANGLES,gl.TRIANGLE_STRIP,gl.TRIANGLE_FAN
//first参数表示从哪个顶点开始绘制，count表示需要用到多少个顶点
gl.drawArrays(mode,first,count)

//功能：获取attribute变量的存储位置
//说明：program表示包含了着色器的着色器程序对象，name表示想要获取存储位置的变量名称
gl.getAttribLocation(program,name)

//功能：向attribute变量赋值
//说明：location,attribute变量的存储位置，v0,v1,v2表示填充attribute变量的分量的值
gl.vertexAttrib3f(location,v0,v1,v2)
```

update：程序流程图

![](http://ojv7mano6.bkt.clouddn.com/17-12-11/1663928.jpg)

相关示例的GitHub地址：https://github.com/wdkkk/WTFWebGL