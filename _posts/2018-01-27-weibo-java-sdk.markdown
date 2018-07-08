---
layout: my_post
title:  "全解使用官方Weibo微博SDK开发客户端"
date:   2018-01-27 15:13:00 -0700
categories: 
permalink: /weibo/java/sdk/2018/01/27/weibo-java-sdk.html
---

Weibo提供了十分方便的SDK可以供开发者使用。SDK支持包括网页应用，手机移动应用，以及桌面应用，基本涵盖了所有平台。目前提供的[SDK支持的语言以及平台](http://open.weibo.com/wiki/SDK)包括Java, PHP, Flash, Python, Javascript, nodejs, C++, C#, Android, iOS, Windows 7，Ruby，Delphi，Windows 8。Weibo的SDK支持Oauth2，对于开发十分方便。

本文通过描述如何开发一个自动发微博的bot，来介绍一下如何利用Weibo的SDK。

#### 环境配置

首先需要在Weibo的开发者[网站上注册你的app](http://open.weibo.com/development/mobile)，这样才可以获得接入Weibo API的权限。

![weiboapp]({{ site.url }}/assets/weibo_app.png)

下面来配置开发环境，在这里我们使用Java SDK来作展示，可以在这里[下载](http://open.weibo.com/wiki/SDK)。下载后解压缩可以看到下面的文件结构：

![weibojavafolder]({{ site.url }}/assets/weibo_java_folder.png)

打开config.properties文件，修改client_ID，client_SERCRET，redirect_URI三个值（第一步注册app后可以获得前两个值，redirect_URI需要在注册app后的控制面板里设置）。然后用使用[eclipse](https://www.eclipse.org/) java IDE，打开下载的Weibo SDK（文件夹weibo4j-oauth2）。接下来就可以进行实际的开发了。

#### 客户端开发

{% include adsense-article1.html %}

通过下面的代码可以完成账户登录，获取AccessToken。每次调用Weibo API时需要附加AccessToken来完成与服务器的认证，调用才可以成功。

{% highlight java %}
Oauth oauth = new Oauth();
BareBonesBrowserLaunch.openURL(oauth.authorize("code")); // 弹出默认浏览器，登陆个人微博帐户后会出现一个code
System.out.println(oauth.authorize("code"));
System.out.print("Hit enter when it's done.[Enter]:");
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
String code = br.readLine(); // 读取用户输入的code（前面的浏览器中获取的）
try{
	AccessToken at = oauth.getAccessTokenByCode(code).getAccessToken(); // 获取到AccessToken
} catch (WeiboException e) {
}
{% endhighlight %}

由于我注册的app还没有通过申请，因此我没有访问statuses/update.json API的权限。我使用statuses/share.json API来发送微博，可惜目前的SDK并不直接支持该API。这正好给了我机会自己实现这个功能，直接调用API接口。

{% highlight java %}

HttpClient client = new HttpClient(); // 创建一个HttpClient，用来发送http请求

Timeline tm = new Timeline(accessToken);
try {
    // 访问statuses/share.json API，传送参数status为要分享的内容（注意内容必须包含网页链接，不然调用会失败），并且附加accessToken来与服务器认证。
	Status st = new Status(client.post(WeiboConfig.getValue("baseURL")
			+ "statuses/share.json",
			new PostParameter[] { new PostParameter("status", status) },
			accessToken)); 
} catch (WeiboException e) {
}
{% endhighlight %}

所涉及的关键技术细节都已调试完毕，整理打磨一下代码就完成了一个可以自动发送微博的脚本bot。欢迎留言以及分享哦。