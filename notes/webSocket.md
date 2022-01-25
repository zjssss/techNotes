### 定义

是一种网络通信协议，是 `HTML5` 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议

WebSocket用于在Web浏览器和服务器之间进行任意的双向数据传输的一种技术。WebSocket协议基于TCP协议实现，包含初始的握手过程，以及后续的多次数据帧双向传输过程。其目的是在WebSocket应用和WebSocket服务器进行频繁双向通信时，可以使服务器避免打开多个HTTP连接进行工作来节约资源，提高了工作效率和资源利用率。

### 特点

解决了http协议中不适用于实时通信的缺点

建立在tcp协议的基础上，服务端的实现比较容易

与http协议有良好的兼容性，并且握手阶段采用http协议，因此握手不容易被屏蔽，能通过各种http代理服务器

没有同源限制，客户端可以 与任意服务器通信

可以发送文本，也可以发送二进制

协议标识符是`ws`（如果加密，则为`wss`），服务器网址就是 URL

### webscoket和http的区别

都是一样基于TCP的，都是可靠性传输协议。都是应用层协议

WebSocket是双向通信协议，模拟Socket协议，可以双向发送或接受信息。HTTP是单向的。WebSocket是需要握手进行建立连接的。

WebSocket在建立握手时，数据是通过HTTP传输的。但是建立之后，在真正传输时候是不需要HTTP协议的。

### webscoket和socket的区别

Socket其实并不是一个协议，而是为了方便使用TCP或UDP而抽象出来的一层，是位于应用层和传输控制层之间的一组接口。
当两台主机通信时，必须通过Socket连接，Socket则利用TCP/IP协议建立TCP连接。TCP连接则更依靠于底层的IP协议，IP协议的连接则依赖于链路层等更低层次。

Socket是传输控制层协议，WebSocket是应用层协议。

### 相似的技术

sse

spdy（被http2取代）

webRTC（直播）

### 通信原理

浏览器发出webSocket的连线请求，服务器发出响应，这个过程称为`握手`,握手的过程只需要一次，就可以实现持久连接	

### 客户端API

当 Browser 和 WebSocketServer 连接成功后，会触发 onopen 消息；如果连接失败，发送、接收数据失败或者处理数据出现错误，browser 会触发 onerror 消息；当 Browser 接收到 WebSocketServer 发送过来的数据时，就会触发 onmessage 消息，参数 evt 中包含 Server 传输过来的数据；当 Browser 接收到 WebSocketServer 端发送的关闭连接请求时，就会触发 onclose 消息。我们可以看出所有的操作都是采用异步回调的方式触发，这样不会阻塞 UI，可以获得更快的响应时间，更好的用户体验。

```js
var ws = new WebSocket(“ws://echo.websocket.org”); 
 ws.onopen = function(){ws.send(“Test!”); }; 
 ws.onmessage = function(evt){console.log(evt.data);ws.close();}; 
 ws.onclose = function(evt){console.log(“WebSocketClosed!”);}; 
 ws.onerror = function(evt){console.log(“WebSocketError!”);};
```



### 已经有的webscoket库

#### Java Websocket规范

#### SockJS

SockJS是一个浏览器JavaScript库，对Websocket进行了抽象

#### Socket.IO

一个基于 Node.js 的实时应用程序框架，在即时通讯、通知与消息推送，实时分析等场景中有较为广泛的应用，但是它提供基于**Netty**的服务端实现以及客户端实现，同时支持**Websocket**和长轮询。除了**Websocket**的常用场景外，我们可以通过该组件实现安卓和**IOS**的消息推送。

需要自行封装同Spring的集成，服务端并非社区维护，资源消耗大

#### ReactiveStream

#### GoEasy

GoEasy是一款在国内比较流行的websocket开发框架，目前GoEasy提供完整的websocket前后端解决方案。据了解，GoEasy目前支持比较多的前端技术/框架比如小程序、react、vue、uniapp等的消息发送和接收，另外还支持php、java、python等服务端语言通过调用Restful API实现服务端的消息推送。有websocket使用需求的开发者可以来注册GoEasy账号进行测试使用

### webSocket应用场景

社交聊天、弹幕、多玩家游戏、协同编辑、股票基金实时报价、体育实况更新、视频会议/聊天、基于位置的应用、在线教育、智能家居等需要高实时的场景。