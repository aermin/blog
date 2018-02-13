---
title: react版的抽奖程序&滚动字幕(入门小demo)
type: tags
date: 2017-07-18 20:21:32
tags: 
     - React
     - 学习总结
---

因项目需求，需要写一个react的抽奖程序和字幕滚动。
本来之前用的是vue，没用过react，而项目组框架用的是react，萌新只能弃vue改学用react了

写完这个入门小demo，也算初识了下react。

>老习惯，写了详细的代码注释。

[demo地址](https://github.com/hxvin/draw)

更新 ======================

<!-- more -->

a：增加了抽奖前的判断：

1.当天是否已抽过奖， ‘否’的话进入下一条判断 
（这里用localstorage储存，获取当天24点时间戳，过了当天24点localstorage储存清除，时间都是获取本地时间，实际应用应采用服务器时间，防止用户修改本地时间来作弊）


2.现在抽奖时间是否在16-17点之间，‘是’的话则开始抽奖

b：增加了react版的字幕滚动


[最初版简陋抽奖gif图](http://ooytyiziz.bkt.clouddn.com/QQ20170715-193001-HD.gif)

[抽奖&字幕滚动图](http://ooytyiziz.bkt.clouddn.com/draw.png)

### 下载运行

git  clone https://github.com/hxvin/draw.git

cd draw

npm install

npm start

有错的或者不完善的地方恳请指出纠正。