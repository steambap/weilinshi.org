+++
date = "2016-07-22T11:16:29+08:00"
draft = false
title = "svg captcha"
image = "svg-captcha.png"

+++

## svg captcha
I use a windows laptop at works and it won't compile any native c++ addons.  
However, all of node's captcha generation modules dependend on c++ addons in some ways.
So I wrote a SVG captcha generation module.

github repository：[https://github.com/lemonce/svg-captcha](https://github.com/lemonce/svg-captcha)

Install：  

> npm i --save svg-captcha

Usage:
```Javascript
var svgCaptcha = require('svg-captcha');
// generate random text of length 4
var text = svgCaptcha.randomText();
// generate svg image
var captcha = svgCaptcha(text);
```
