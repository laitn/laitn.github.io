---
layout: my_post
title:  "基于JS的实时通信服务器 - Socket.io"
date:   2017-10-02 21:23:00 -0700
categories: 
permalink: /socket-io/2017/10/02/socket-io-example.html
---

上次介绍了[搭建microservice的利器parse server](https://laitn.github.io/parse/server/api/2017/09/06/parse-server.html)（提供包括general purpose的实时数据库，API等一系列支持），这次介绍[socket.io](https://socket.io/docs/) - 一个快速搭建多人实时通信服务器的开源平台。

注意到这个平台是很偶然的，看到一个网友通过不多的代码就实现了一个非常addicitive的[多人页游](https://github.com/sgoedecke/socket-io-game)：

![tanready](https://raw.githubusercontent.com/sgoedecke/socket-io-game/master/screenshot.png)

socket.io是一个js module，可以通过npm安装：

{% highlight bash %}
$ npm install socket.io
{% endhighlight %}

一个最基本的http+即时通信服务器可以在如下方法搭建：

{% highlight bash %}
//创建http服务器
var app = require('http').createServer(handler) 
//创建即时通信服务器
var io = require('socket.io')(app);                            
//初始化文件操作模块
var fs = require('fs');

//设置http服务器端口并开始监听
app.listen(80);

//http服务器逻辑：读取并返回主页。
function handler (req, res) {
  fs.readFile(__dirname + '/index.html',
  function (err, data) {
    if (err) {
      res.writeHead(500);
      return res.end('Error loading index.html');
    }

    res.writeHead(200);
    res.end(data);
  });
}

// 通信服务器监听client链接
io.on('connection', function (socket) {
  // client建立连接之后，立即发送对其news通道发送欢迎信息
  socket.emit('news', { hello: 'world' });
  // 监听client发送的other请求，并在服务器输出请求内容
  socket.on('other', function (data) {
    console.log(data);
  });
});
{% endhighlight %}

简易的client代码如下：

{% highlight bash %}
<script src="/socket.io/socket.io.js"></script>
<script>
  // 建立与socket.io服务器链接
  var socket = io('http://localhost');
  // 向服务器news通道发送请求
  socket.on('news', function (data) {
    // 显示news通道传回数据
    console.log(data);
    // 向other通道发送数据
    socket.emit('other', { my: 'data' });
  });
</script>
{% endhighlight %}

#### 命名空间（namespace）
Socket.io通过支持namespace来提供多通道通信。不同的client通过链接不同的namespace可以与socket.io服务器建立命名空间通道，接受该命名空间服务器发送的特殊信息。

{% highlight bash %}
const io = require('socket.io')();
const adminNamespace = io.of('/admin');

adminNamespace.to('level1').emit('an event', { some: 'data' });
{% endhighlight %}

#### 广播

{% highlight bash %}
const io = require('socket.io')();
// 向所有连接的client发送消息
io.emit('an event sent to all connected clients'); // main namespace

const chat = io.of('/chat');
// 向所有链接chat空间的客户发送消息
chat.emit('an event sent to all connected clients in chat namespace');
{% endhighlight %}