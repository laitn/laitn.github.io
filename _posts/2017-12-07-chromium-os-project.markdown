---
layout: my_post
title:  "揭秘chromium os - Google背后的开源操作系统"
date:   2017-12-07 18:57:00 -0700
categories: 
permalink: /chromium/os/linux/2017/12/07/chromium-os-project.html
---

[chromium os](https://www.chromium.org/chromium-os)（开源版本chrome os）是Google开发的基于Linux的操作系统。chromium os于2011年上线，同期与Samsung以及Acer推出了两款搭载chrome os的笔记本电脑chromebook。目前chromebook在美国以及全球的教育市场中稳居第一，美国的绝大多数k12学校中学生每天使用chromebook来完成课上教学以及课下作业。

chromium os在[Gentoo Linux](https://www.gentoo.org/)发行版与[chromium](https://www.chromium.org/Home)浏览器的基础之上，在简便，快速，安全（simple，speed，security，a.k.a. 3S）方面进行了强势改进。chromium os借助了Gentoo的包管理工具portage来支持各种系统user工具，支持多种Linux内核。chromium os的UI完全借助于chromium，系统启动时加载一个强化版的支持window manager（窗口管理）的chromium浏览器。

自2017年开始新推出的chromebook上搭载的chrome os都会同时支持Google Play，也就是所有Play里面的Android app都可以在chrome os上运行,极大提高了chrome os的app丰富性。该技术通过container达成chrome os与android共同存在，同时保证chrome os的安全（100%无病毒）。

{% include adsense-article1.html %}

如果对chromium os的兄弟项目 - chromium有兴趣，可以参见另一篇[文章介绍chromium项目](https://laitn.github.io/chrome/browser/2017/11/17/chromium-project.html)。

![the-chrome-comic-book](https://cdn.vox-cdn.com/thumbor/IzZfuLppKXSUq2UZiPeQXqdpas8=/0x0:1000x517/1200x800/filters:focal(409x157:569x317)/cdn.vox-cdn.com/uploads/chorus_image/image/56758501/google_pixelbook1.0.jpg)