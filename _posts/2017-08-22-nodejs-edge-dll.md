---
layout: post
title: Node.js + Edge.js + DLL
tags: [nodejs]
description: >
  說明如何利用Edge.js套件在Node.js呼叫.net的C# function，並可跨平台在windows、ubuntu、mac os等OS上執行。
author: bruce
comments: true
---

## Introduction
[DLL][dll]是微軟實現共享函式庫的一種實作方式，也就是說它把c#開發出來的function封裝起來，並且達到共用、reuse的效果。

[Edge.js][edgejs]是npm上的一個套件，透過與它的介接，可幫助你在[Node.js][nodejs]使用別人提供DLL中的function。

在windows的環境只要安裝.net framework即可，如果是在Mac、Linux則還要另外裝sdk。依據Edge.js在github上的說明有兩種選擇方式，可以安裝[Mono][mono]或者安裝Mono和[.NET Core][netcore]。Mono和.Net Core的目的都是為了windows作業系統以外的環境能夠開發、執行.net程式，只是Mono由community發展的開源版本，而.Net Core則是微軟在2016年推出的官方版本。

本篇文章的目的是在說明如何利用Edge.js套件和Mono SDK在Node.js呼叫.net的function，並可在mac os、ubuntu上執行。

## Prerequisite
這邊只安裝Edge.js官方預設使用的Mono，而如果只看官方文件，會覺得它實作方式並不困難，但特別強調Edge.js和Mono彼此會有相容性的問題，並不代表所有版本都能正常運作。以下為這次開發環境使用的版本：

|OS   |Node.js   |Mono   |Edge.js   |
|:---------|:----------|:----------|:----------|
| macOS Sierra|v.6.11.0| 4.6.2 |7.10.1|


## Instruction
安裝mono
:  在mac上非常簡單，只要在官網找到archive list，找出特定版本的pkg安裝即可。[download][momo4.6.2]可由此下載。

安裝Edge.js
: 利用npm安裝指定的版本。
~~~shell
$ npm install --save edge@7.10.1
~~~

Testing Code
: 若以下程式能順利執行，代表一切安裝都正常。
~~~js
var edge = require('edge');
var helloWorld = edge.func(`
    async (input) => {
        return ".NET Welcomes " + input.ToString();
    }
`);
helloWorld('JavaScript', function (error, result) {
    if (error) throw error;
    console.log(result);
});
~~~

Sample Code
: 利用外部的DLL。
~~~js
var edge = require('edge');
var sayGreeting = edge.func({
    assemblyFile: 'Sample.dll', // DLL name
    typeName: 'SampleLib.MainClass', // namespace + class name
    methodName: 'sayGreeting' // method name
});
var name = 'Bruce Hsu'
console.log(sayGreeting(name)); // print "Hello world, Bruce Hsu!"
~~~

在ubuntu 16.04安裝Mono
: 
~~~sh
$ apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
$ echo "deb http://download.mono-project.com/repo/debian wheezy/snapshots/4.6.2.16 main" | sudo tee /etc/apt/sources.list.d/mono-official.list
$ apt-get update
$ apt-get install mono-complete
~~~

## Reference

* [https://stackoverflow.com/questions/33763177/install-older-version-of-mono](https://stackoverflow.com/questions/33763177/install-older-version-of-mono)

* [http://no2don.blogspot.com/2015/10/nodejs-nodejs-edge-dll-c-function.html](http://no2don.blogspot.com/2015/10/nodejs-nodejs-edge-dll-c-function.html)

[dll]: https://zh.wikipedia.org/wiki/%E5%8A%A8%E6%80%81%E9%93%BE%E6%8E%A5%E5%BA%93
[edgejs]: https://github.com/tjanczuk/edge
[nodejs]: https://nodejs.org/
[mono]: http://www.mono-project.com/
[netcore]: https://www.microsoft.com/net/core
[momo4.6.2]: https://download.mono-project.com/archive/4.6.2/macos-10-universal/MonoFramework-MDK-4.6.2.macos10.xamarin.universal.pkg