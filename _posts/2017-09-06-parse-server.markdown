---
layout: my_post
title:  "Parse Server - 1分钟搭建专业API服务"
date:   2017-09-06 21:34:00 -0700
categories: 
permalink: /parse/server/api/2017/09/06/parse-server.html
---

[Parse Server](http://parseplatform.org/)是一项opensource项目，帮助快速搭建API服务。Parse最早由Facebook推出的一项hosted service，后来被关闭，演变成为了现在的Parse Server。Parse platform目前提供了Parse Server，还有丰富的客户端SDK支持移动网页以及嵌入式应用。

#### **简而言之**

简而言之，Parse Server是一个基于REST API的数据库。通过发送类似下面例子的http请求来查询以及修改后台的mongodb数据库：

{% highlight bash %}
$ curl -X POST \
-H "X-Parse-Application-Id: APP_ID" \
-H "Content-Type: application/json" \
-d '{"firstname":"peter",";lastname":"Park"}' \
http://localhost:1337/parse/names

# APP_ID 在配置Parse Server时产生（下面会有介绍）
# 返回的结果如下：

{
  "objectId": "3kF9k1pTn",
  "createdAt": "2017-9-1T13:42:54.240Z"
}
{% endhighlight %}

查询刚刚存储的记录可以通过以下请求：

{% highlight bash %}
$ curl -X GET \
  -H "X-Parse-Application-Id: APP_ID" \
  http://localhost:1337/parse/names/3kF9k1pTn

# 返回的结果如下：
{
  "objectId": "3kF9k1pTn",
  "firstname", "peter",
  "lastname", "Park",
}
{% endhighlight %}

而搭建起Parse Server开发服务器则只需要以下几步：

{% highlight bash %}
$ sh <(curl -fsSL https://raw.githubusercontent.com/parse-community/parse-server/master/bootstrap.sh)
# 按照提示设置App id, Master key, Mongodb uri。建议使用默认值（按回车选择默认）
$ npm install -g mongodb-runner
$ mongodb-runner start
$ npm start
{% endhighlight %}

#### **Parse Server设置**

Parse Server在根本上是一个node.js express的应用。在index.js中初始化各种参数：

{% highlight bash %}
var api = new ParseServer({
  databaseURI: 'mongodb://your.mongo.uri', // mongodb的URI
  cloud: './cloud/main.js',                // API文件
  appId: 'myAppId',                        // app ID
  fileKey: 'myFileKey',                    // 文件服务器的key
  masterKey: 'mySecretMasterKey',          // 主key，可以override所有已经设置的权限
  clientKey: 'myClientKey',                // （非必需）
  restAPIKey: 'myRestAPIKey',              // （非必需）
  javascriptKey: 'myJavascriptKey',        // （非必需）
  dotNetKey: 'myDotNetKey',                // （非必需）
  push: { ... },                           // 设置推送消息
  filesAdapter: ...,                       // 设置文件服务器适配器 （用于从文件服务器例如S3读取文件）
  auth: ...,                               // 设置第三方认证
});
{% endhighlight %}

#### **客户端SDK**

Parse Server客户端SDK初始化极为容易。目前Parse提供iOS (OS X, tvOS), Android, Javascript, .NET+Xamarin，Unity，PHP，Arduino以及Embedded C的SDK。客户端设置仅仅需要提供Parse serverURL以及Application ID。Javascript SDK初始化客户端代码如下：

{% highlight bash %}
Parse.initialize("APP_ID");
Parse.serverURL = 'http://localhost:1337/parse'
{% endhighlight %}

#### **Production Deployment**

实际应用中，Parse Server布置一般选择现成的Production Environment（[Heroku](https://www.heroku.com/)，[Glitch](https://glitch.com/), Amazon AWS等）。选择这些现成的环境可以保证service的安全以及availability，并且维护起来更加容易。后端的mongodb数据库也可以选择现成的服务提供商，例如mlab。

#### **消息推送**

Parse Server消息推送是借助第三方服务完成的。对于Android的推送是通过Google Cloud Message完成的，而对于iOS（OS X，tvOS）的推送则通过Apple Push Notification Service。正由于通过Google以及Apple的服务来进行消息推送，因此Parse Server的配置相当简单。以Android为例，仅仅需要提供Google Cloud Console中创建的apiKey即可:

{% highlight bash %}
    ...
    push: {
        android: {
        senderId: '...',
        apiKey: '...'
        },
    }
    ...
{% endhighlight %}

推送消息实际的发送既可以通过API调用Parse Server也可以Parse Server直接发送：

{% highlight bash %}
# 通过调用API发送，推送发送给所有ios以及android设备，注意必须提供master key
$ curl -X POST \
  -H "X-Parse-Application-Id: APP_ID" \
  -H "X-Parse-Master-Key: yourMasterKey" \
  -H "Content-Type: application/json" \
  -d '{
        "where": {
          "deviceType": {
            "$in": [
              "ios",
              "android"
            ]
          }
        },
        "data": {
          "title": "测试消息题目",
          "alert": "测试消息内容."
        }
      }'\   http://localhost:1337/parse/push
# 在Parse Server上直接发送.
Parse.Push.send({
  where: { ... },
  data: { ... }
}, { useMasterKey: true })
.then(function() {
  // Push sent!
}, function(error) {
  // There was a problem :(
});
{% endhighlight %}

#### **API权限**

Parse Server为数据操作提供了权限支持。通过REST API来对每一个数据表设置权限。例如设置/users数据表的权限为admin可以通过以下的API调用：

{% highlight bash %}
curl -X POST \
  -H "X-Parse-Application-Id: APP_ID" \
  -H "X-Parse-Master-Key: yourMasterKey" \
  -H "Content-Type: application/json" \
  -d '
{
  classLevelPermissions:
  {
    "find": {
      "requiresAuthentication": true,
      "role:admin": true
    },
    "get": {
      "requiresAuthentication": true,
      "role:admin": true
    },
    "create": { "role:admin": true },
    "update": { "role:admin": true },
    "delete": { "role:admin": true },
  }
}'\ http://localhost:1337/schemas/users
{% endhighlight %}

#### **Live Query(即时查询)**

Live Query是基于event（create，enter，update，leave，delete）的实时数据查询。服务器端通过简单的初始化可以使得一个数据表支持Live Query：

{% highlight bash %}
let api = new ParseServer({
  ...,
  liveQuery: {
    classNames: ['users']
  }
});
{% endhighlight %}

在客户端只要subscribe一个数据表之后，就可以接受任何这个数据表的变化：

{% highlight bash %}
// 当users表中添加新条目时，该函数体会执行，user即为新添加条目
Parse.Query('/users').subscribe().on('create', (user) => {
});
{% endhighlight %}

#### **用户认证(Authentication)支持**

Parse Server支持Oauth以及第三方的authentication（Twitter， Meetup，Linkedin，Google，Instagram，Facebook）。仅需设置中填写相应条目就可以。同时也支持自定义（自行开发）的authentication。

#### **其他功能**

Parse Server支持Cache adapter（例如redis），以及file adapter（Google Cloud Storage以及Amazon AWS S3）。通过这些支持，极大的提高了Parse Server处理production当中高负载的情况。