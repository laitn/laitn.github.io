---
layout: my_post
title:  "走进chromium project - 头号开源浏览器"
date:   2017-11-17 22:36:00 -0700
categories: 
permalink: /chrome/browser/2017/11/17/chromium-project.html
---

2008年9月，Google推出了chrome浏览器以及其源代码库 - [chromium project](https://www.chromium.org/Home)。该项目的横空出世一举打破了IE和Firefox在桌面浏览器领域的垄断，在一个已经crowded的市场当中创造了奇迹。目前chrome浏览器的[市场占有率](https://www.netmarketshare.com/browser-market-share.aspx?qprid=2&qpcustomd=0)超过40%，第二名是已经日薄西山的IE（12%）。而当初的firefox，目前的市场占有率仅有6%。chrome浏览器背后的开源项目chromium project也成为了历史上最为成功的开源项目之一。

![chromium-logo]({{ site.url }}/assets/chromium-project-logo.png)

{% include adsense-article1.html %}

如今已经走过9个年头的chromium project，凝聚了Google软件工程师以及开源社区的智慧，对开源社区、编程语言以及项目管理领域造成了巨大的影响。使用webkit作为render engine，以及当时的独辟蹊径的multi process架构使得chrome浏览器效率与众不同，单个tab崩溃丝毫不会影响其他tab的效率。chrome浏览器简单明了的设计吸引了大批用户，突破了下载量记录。Google为了使用户了解chrome浏览器优越的性能，聘请了知名漫画家Scott McCLoud创造了[The Chrome Comic Book](http://www.scottmccloud.com/googlechrome/)。随着项目的深入以及产品的迭代，chromium project包含了越来越多的功能，吸引了大批的贡献者。

![the-chrome-comic-book](https://www.google.com/googlebooks/chrome/images/small/1.jpg)

chromium project主力的贡献者依旧是Google的工程师。包括每条代码的code review以及问题追踪，整个开发的过程完全开源以及透明。代码code review发生在https://chromium-review.googlesource.com/。 问题追踪都可以在crbug.com查询。很多网络科技媒体密切追踪这些工具，用来猜测以及揭露新的特性以及未来web发展的趋势。

开源软件爱好者们可以在自己的电脑上下载源码，编译一个完整的chromium浏览器(开源版本chrome浏览器)。建议第一次编译在Linux上，可以根据这个[官方教程](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_build_instructions.md)。chromium还为开发者们提供了一个逆天的工具 - [code search](https://cs.chromium.org/)。code search会不停在最新的chromium代码库上建立索引，并通过web的方式提供给开发者及时的代码搜索功能。

如果大家有兴趣，不妨尝试一下chromium project。如果有任何问题，可以在下方评论或者twitter上问我。同时我在另一篇post中介绍了chromium的兄弟项目 - [chromium os项目](https://laitn.github.io/chromium/os/linux/2017/12/07/chromium-os-project.html)。