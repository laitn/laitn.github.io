---
layout: my_post
title:  "博客是如何产生的"
date:   2017-08-07 22:30:00 -0700
categories: 
permalink: /jekyll/web/2017/08/07/jekyll-blog.html
---

博主之前偶尔在门户或问答网站上写过技术性小文章，但总归不是自己的地方，少不了各种限制。今天开了一个小博客，第一篇文章来写写如何搭建这个站点吧。

起初打算从无到有建一个站，但稍微一查就发现根本make economical sense。前段时间痴迷于前端开发，使用鄙司的firebase+polymer做了一个[IM应用](http://youchat-58061.firebaseapp.com)。学习的过程还算顺利，但深觉制作一个站点（或者应用）非常耗费时间跟精力。在内容管理（content management）领域有如此多的工具，应该利用一下的。对比了几家，发现jekyll是比较有个性的，坚持全静态，无数据库。现在网站即app的年代，jekyll还能横行于世，应该是做的够好吧。而且我的目的是为了发布内容，应该对数据库的需求不高。何况根据搜索引擎优化（SEO）的原理，静态内容是高权重的。唯一的后台需求应该是文章评论，但貌似可以使用[disqus](https://disqus.com/)来撑一下。

仔细阅读了一下jekyll的文档，了解了一下基本状况。以下命令就可以从无到有快速创建一个博客了：

{% highlight bash %}
$ gem install jekyll bundler #安装jekyll
$ jekyll new my-awesome-site #快速创建一个mimima主题的静态站点
$ cd my-awesome-site
$ bundle exec jekyll serve   #预处理并serve站点
{% endhighlight %}

在刚创建好的站点内可以发现一下目录结构：

{% highlight bash %}
my-awesome-site
|--_posts #存放所有文章的文件夹
|   |--2017-08-07-xxx.markdown #每个文章需要类似的格式
|--_site #jekyll自动产生的最终站点
|--_config.yml #配置文件
|--404.html #404页面
|--about.md #关于页面
|--Gemfile #开发环境配置文件
|--Gemfile.lock #开发环境配置文件
|--index.md #自动生成的主页
{% endhighlight %}

其中需要关注的就是_config.yml了，也就是配置文件,也是目前我唯一需要修改的文件(除去文章本身)。

{% highlight bash %}
# Site settings
title: #
description: #
url: # the base hostname & protocol for your site, e.g. http://example.com
github_username:  #
# Build settings
markdown: kramdown # markdown processor https://kramdown.gettalong.org/
theme: minima # 博客主题
plugins:
  - jekyll-feed #插件
{% endhighlight %}

设置好这些基本的内容之后就博客就配置完成了。

在阅读文档的时候发现github pages是对jekyll天然支持的，那么deploy也就自然选择github page了。
把我原来的github page的内容转移到about页面，commit+commit刚建立的站点到github。大功告成。