---
title: WebGL从入门到放弃01
date: 2017-11-21 20:04:22
tags:
---

是的，决定开始学WebGL

WebGL的编程过程很扯淡，有点把大象放进冰箱的感觉。

<!-- more -->

先来画个点吧

- 简单的shader

```c++
//顶点着色器
void main(){
  gl_Position = vec4(0,0,0,1);
  gl_PointSize = 10.0;
}
//片元着色器
void main(){
  gl_FragColor = vec4(1,0,0.5,1);
}
```

`gl_Psition` 、`gl_PointSize`、`gl_FragColor` 都是WebGL 内置的变量

- 编译shader

```javascript
 //创建着色器对象,参数为着色器类型，gl_VERTEX_SHADER gl_FRAGMENT_SHADER
const shader = gl.createShader();
//装填着色器代码 参数为着色器对象和着色器代码
gl.shaderSource(); 
//编译着色器，参数为已经装填着色器代码的着色器对象
gl.compileShader(); //编译生成着色器
```

- 创建着色器程序

```javascript
//创建着色器程序
const program = gl.createProgram();
//装填顶点着色器
gl.attachShader(program, vshader);
//装填片元着色器
gl.attachShader(program, fshader);
//链接编译着色器程序
gl.linkProgram(program);
```

- 绘画

```javascript
//参数type表示绘制对象的类型，如这里画点为gl.POINTS ,第二个参数表示从哪个顶点开始绘制,这里为0，第三个参数表示绘制多少个顶点，这个为1
gl.drawArrays(type, first, count);
```



- 设置背景色

```javascript
gl.clearColor(0.0, 0.5, 0.0, 1.0); //预设背景色
gl.clear(gl.COLOR_BUFFER_BIT); //清空颜色缓冲区(设置背景色)
```

这里只是做个简单的测试，相关函数的说明日后在说

update:程序流程图

![](http://ojv7mano6.bkt.clouddn.com/17-12-11/40845817.jpg)

相关示例的GitHub地址：https://github.com/wdkkk/WTFWebGL