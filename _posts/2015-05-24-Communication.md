---
layout:     post
title:      "web通信技术比较"
date:       2015-05-24 12:00:00
author:     "sdq"
header-img: "img/post-bg-06.jpg"
---

<p>##内容整理自<a href = "http://stackoverflow.com/questions/11077857/what-are-long-polling-websockets-server-sent-events-sse-and-comet">stackoverflow</a>。</p>
<h3>常规HTTP</h3>
<ol>
  <li>Client向Server请求网页。</li>
  <li>Server处理请求。</li>
  <li>Server将请求数据返回给Client。</li>
</ol>
<img src = "http://i.stack.imgur.com/TK1ZG.png">

<h3> 轮询 </h3>
<ol>
  <li>常规http请求。</li>
  <li>在请求网页中执行js，固定时间间隔向Server发送请求。</li>
  <li>Server每接收一个请求后返回数据。</li>
</ol>
<img src = "http://i.stack.imgur.com/qlMEU.png">

<h2>实时通信</h2>
<h3> 长轮询 </h3>
<ol>
  <li>常规http请求。</li>
  <li>请求页面执行js向Server请求数据。</li>
  <li>Server不马上返回数据，而是在Server端等候新数据。</li>
  <li>当Server收到新数据后立刻向Client返回数据。</li>
  <li>Client收到Server返回的新数据后，重新发起一个请求，回到步骤3。</li>
</ol>
<img src = "http://i.stack.imgur.com/zLnOU.png">
<p>使用长轮询的公司举例：</p>
<li>Dropbox</li>
<li>Amazon SQS</li>
<li>Livefyre</li>

<h3> SSE </h3>
<p>SSE的全称是HTML5 Server Sent Events</p>
<ol>
  <li>常规http请求。</li>
  <li>请求页面执行js向Server开启一个连接。</li>
  <li>当有Server新的消息时，Server向Client发送一个事件(event)</li>
</ol>
<img src = "http://i.stack.imgur.com/ziR5h.png">
<p>可以参考的文章有如下几篇：<a href = "https://developer.mozilla.org/en-US/docs/Server-sent_events/Using_server-sent_events">article1</a>,<a href = "http://html5doctor.com/server-sent-events/#api"> article2</a>,<a href = "http://www.html5rocks.com/en/tutorials/eventsource/basics/"> article3</a>,<a href = "http://jaxenter.com/tutorial-jsf-2-and-html5-server-sent-events-42932.html"> article4</a></p>
<p>使用SSE的公司举例：</p>
<li>Twitter</li>
<li>Datasift</li>
<li>Gitter</li>

<h3> Websockets </h3>
<ol>
  <li>常规http请求。</li>
  <li>请求页面执行js向Server开启一个连接。</li>
  <li>这个时候Server和Client可以相互发送信息。(双向)</li>
</ol>
<img src = "http://i.stack.imgur.com/CgDlc.png">
<p>使用WebSocket的公司举例：</p>
<li>Coinbase</li>
<li>Slack</li>
<li>Trello</li>

<h3> Comet </h3>
<p>Comet是一种用于web的推送技术，能使服务器实时地将更新的信息传送到客户端，而无须客户端发出请求，目前有两种实现方式，长轮询和iframe流。</p>

<h3> Webhook </h3>
<p>Webhook就是用户通过自定义回调函数的方式来改变Web应用的一种行为，这些回调函数可以由不是该Web应用官方的第三方用户或者开发人员来维护，修改。通过Webhook，你可以自定义一些行为通知到指定的URL去。Webhook的“自定义回调函数”通常是由一些事件触发的，比如推送代码到代码库或者博客下新增一个评论，源站点会为Webhook进行HTTP请求的URI配置。用户通过配置，就可以使一个网站上的事件调用在另一个网站上表现出来，这些事件调用可以是任何事件，但通常应用的是系统集成和消息通知。(<a href= "https://worktile.com/club/tutorial/d5003cd3d8ae4033a75cc285fee60d04">webhook小科普</a>)</p>
<p>使用WebSocket的公司举例：</p>
<li>Github</li>
<li>Facebook</li>
