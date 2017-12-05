---
title: WebGL从入门到放弃03
date: 2017-12-05 19:32:31
tags:
---

之前成功的使用WebGl画点，这次在加点颜色，使每个点的颜色随机

<!--more-->

由于每次都要使用相同的代码生成shader等，我将一些常用的函数封装成一个简单的库，一开始只封装了几个简单的函数，慢慢在完善吧

要给点赋予颜色，需要将之前的片元着色器中的颜色改为可赋值的

```c++
precision mediump float;
uniform vec4 u_Color;//声明一个变量
void main() {
gl_FragColor = u_Color; }
```



片元着色器对应的是`uniform`类型的变量,获取它的存储位置和传值的方式与之前`attribute`变量不一样

```c++
//功能：获取uniform变量的存储区域
//说明：参数program,表示着色器程序，u_var着色器代码中表示uniform类型的变量
gl.getUniformLocation(program,u_var)
//功能：向uniform变量传值
//说明：u_location表示变量的存储位置,f1,f2,f3,f4表示float类型的值  
gl.uniform4f(u_location,f1,f2,f3,f4)  
```

相关示例的GitHub地址：https://github.com/wdkkk/WTFWebGL