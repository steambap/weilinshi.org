+++
date = "2016-07-22T11:16:29+08:00"
draft = false
title = "svg captcha"
image = "svg-captcha.png"

+++

## svg验证码

在node中生成验证码可以有很多选择，但没有一个能用的，因为我使用的是windows，无法编译c++模块。 

在经过了一定的实验后，我发现可以生成svg格式的验证码，不需要依赖任何c++模块。

项目地址：[https://github.com/lemonce/svg-captcha](https://github.com/lemonce/svg-captcha)

安装方法：  

> npm i --save svg-captcha

使用方法

{{< highlight JavaScript >}}
var svgCaptcha = require('svg-captcha');
// 生成由4个字符的随机字符串
var text = svgCaptcha.randomText();
// 由随机字符串生成svg格式验证码
var captcha = svgCaptcha(text);
{{< /highlight >}}
