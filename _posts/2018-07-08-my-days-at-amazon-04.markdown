---
layout: my_post
title:  我在科技巨头亚马逊的日子 4 值日生
date:   2018-07-08 10:28:00 -0700
categories: book2
permalink: /2018/07/08/my-days-at-amazon-04.html
---

[回到目录]({{ site.url }}/my-days-at-amazon/)

修了几周bug之后，对我们的技术有了个大致的了解，于是开始提出并实现一些复杂写的feature。与此同时，我也进入了组里值班的rotation list，开始轮流on-call。

在互联网公司当中，on-call duty应该说是十分普遍的。意思是指某个工程师在一段时间内（通常是一周）需要一直处于战备状态，一旦所负责的系统出现问题，需要在几十或者十几分钟内响应并解决问题。而且这种on-call是24小时的，也就是说如果大半夜里你被电话吵醒，你是需要立即停止睡眠，打开电脑响应突发情况的。对于用户遍及全球各个时区的amazon.com，半夜被呼叫的概率不低于白天被呼叫的概率。一般大公司中，会有专门的工程师负责on-call，称为Site Reliability Engineer（SRE）。然而对于刚刚deploy，并且正在不断迭代的新系统，开发人员往往会承担on call的责任相当长一段时间，直到项目进入维护阶段。

这一周轮到我第一次on-call。之前我shadow（手机会接收到请求，但不需要做出相应）了一次on-call，见识了一下这项任务的基本流程。根据请求的严重程度和响应速度，从轻到重分为1，2，3三个级别。shadow的时候基本都是级别为3的请求，同事们解决起来也都很顺利。所以根据shadow的经验，我对on-call没有给与特殊的重视。却为此付出了惨重的代价。

周一早上天还没亮，就被手机发出的on-call警报吵醒。起床摸到手机打开一看，居然是2级的请求。尽管心里有一万个不乐意，还是强忍着睡意努力起了床，毕竟这是自己的工作。打开电脑开始对着邮件开始研究，看了好几遍也没弄明白到底是什么问题。十几分钟过去了，问题显然没有得到解决，因为我压根没弄清楚问题是什么。突然发现经理在内部工具内上了线，我意识到由于我没有在规定时间内解决，请求被escalate到了我的经理那里，很明显他的美梦也被警报声打破了。

这次事件使我自信心严重受挫。本来以为已经顺利进入工作状态，可以独立完成任何分内的工作，可这件事情却展示了我在技术能力方面的欠缺。此刻的我十分后悔之前shadow时的疏忽和侥幸心理。如果on-call之前可以研究一下常见问题以及对应的解决方案，也许第一个问题就会迎刃而解。

跟生活当中很多事情一样，工作当中的事情也不给人机会后悔。on-call的第一天就收到了多个请求，在同事的帮助下，在紧张的氛围中度过了一百天。晚上回到家，压力下也继续不断的研究各种可能出现的问题，以防明天上班之前再次出现问题，再次把经理从睡梦中惊醒。

随后的一周内，睡眠不足的我强撑着处理着各种各样的问题，好不容易挨到了周末。在去超市购物的过程中，我也带着笔记本电脑，随时查看手机看是否有请求。这次经历后，我才意识到这份工作的困难，也为自己未来的工作提出了新的要求。
