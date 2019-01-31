+++
date = "2016-10-30T11:40:57+08:00"
title = "Typescript vs Flow"
draft = false
image = "ts-flow.png"

+++

_Update 2019-01-30_

More than two years have passed since I wrote this. Right now the Typescript had grown a lot since then. I honestly think Typescript is a better choice today.

## My experience with Typescript and Flow
When it comes to type safety in Javascript projects, we have two choices
 available: Typescript and Flow.  
  
People argued that Javascript does not have type checks and it often lead
 to some very hard to catch bugs. A common example of that would be
 a property of an object became undefined somewhere and you try to use that
 property.  
  
Smart people at Microsoft and Facebook developed their own open source
 project that promised to catch those bugs for you.  

### Typescript
Typescript is advertised as "a typed superset of JavaScript that
 compiles to plain JavaScript."  
  
It is one of the most popular language on github and downloads
 on npm is 2M at the time of writing.  
I can assume that it is very popular and I can get help If I'm stuck.  
  
Although all Javascript are valid Typescript, I cannot just rename
 my existing .js files to .ts and hope it works like what Less and Sass
 works with .css files. So there is no easy way to convert an existing
 project to Typescript project and I have to start from scratch.  

To start with Typescript, I have to read their documentation
 [here](https://www.typescriptlang.org/docs/tutorial.html).
 I just want a Javascript flavored Typescript project, and
 things seem to be relatively easy. I add simple type annotations
 and call it a day.  
  
However, the difficult part comes when I want to do something more
 complicated like test and publish to npm.  
  
First, I have to pull in some type definition, or Typescript will
 refuse to compile because he does not understand and think it's unsafe to use

> import assert from 'assert';

And then I went through the painful process of add type definition and
 it does not work out very well. The typings and tsd module confused me
 and I have no idea how it works at the end. What stopped me from
 continue my Typescript journey is test. In order to run my mocha test,
 I cannot just

> mocha --compilers ts:typescript

It won't work for me even with the require-typescript. I looked at popular
 Typescript repository and it looks like everybody went with compile and
 then test workflow. This is where I stopped.

### Flow
Flow is "A STATIC TYPE CHECKER FOR JAVASCRIPT".  
It have 1.8M downloads on npm at the time of writing.  

All of existing Javascript files can be checked by Flow as it is a
 simple matter of adding

> // @flow

at the top of a Javascript file.  
  
However, to make most out of Flow's static type check, you have to add
 type annotations to your Javascript file. This is will be difficult
 for some of the existing projects, but can a breeze for projects that
 use babel.  
Flow's syntax looks like this:

> const str: string = 'Hello, world!';

which can be stripped by a [babel plugin](https://babeljs.io/docs/plugins/transform-flow-strip-types/)
.  
  
Flow based project can be setup like any babel project as I expect. Most of
 Flow's type annotations are similar or most of the time, exactly the same
 as Typescript one. So I have no problem writing Flow.  
When comes to test and build, again, it's still the same babel workflow.  

> mocha --compilers js:babel-register

This is will get my mocha test running.  
  
The best part of it, which I like the most, is that babel will compile as
 long as it's syntax is correct. In other words, Flow is my additional lint
 tool that does not stop my code from running. Also, I can remove @flow
 check directive at the top at any time if I feel their check is not right.
  
## Conclusion
Both Typescript and Flow are great tools. My approach to Typescript was
 probably wrong and I'll try again with a c# mind. Flow is a great
 addition to Javascript and works best for babel projects.
  
For me, I will need babel's new shiny minifier babili no matter what I do
 in near future. So I'll go with Flow personally. Maybe I'll try
 Typescript with Angular 2 project sometime, but I'll recommand anyone
 who want type safety with Flow.
