SkyRTC
======
##简介
一个Node.js编写的WebRTC服务器端库，为服务器端库，需要配合客户端库[SkyRTC-client](https://github.com/LingyuCoder/SkyRTC-client)共同使用，用于搭建基于WebRTC和WebSocket技术的在线音频、视频聊天室

##SkyRTC前端库SkyRTC-client
[SkyRTC-client](https://github.com/LingyuCoder/SkyRTC-client)

##简单示例
###NPM安装
执行如下命令从npm进行安装：
```
$ npm install skyrtc
```
###监听服务器
```javascript
var express = require('express');
var app = express();
var server = require('http').createServer(app);
var SkyRTC = require('skyrtc').listen(server);
var port = process.env.PORT || 3000;
server.listen(port);
```

###监听WebRTC事件
SkyRTC继承自EventEmitter, 可以通过如下语法监听事件：
```javascript
SkyRTC.rtc.on('eventName', function(params) {
  //...
});
```

##内置事件
* new_connect
* new_peer
* remove_peer
* socket_message
* ice_candidate
* offer
* answer

###new_connect
新用户与服务器建立WebSocket连接时触发

参数：
* socket——新建立的WebSocket连接实例

###new_peer
用户加入房间后触发

参数：
* socket——用户使用的WebSocket连接实例
* room——房间名称

###remove_peer
用户关闭连接后触发

参数：
* socket——用户使用的WebSocket连接实例

###socket_message
客户端向服务器端发送消息，且非自定义事件格式时触发

参数：
* socket——用户使用的WebSocket连接实例
* msg——发送的消息内容

###ice_candidate
接收到ice candidate信令时触发

参数：
* socket——用户使用的WebSocket连接实例
* candidate——ice candidate信令数据

###offer
接收到offer信令时触发

参数：
* socket——用户使用的WebSocket连接实例
* offer——offer信令数据

###answer
接收到answer信令时触发

参数：
* socket——用户使用的WebSocket连接实例
* answer——answer信令数据

##接口
* getRooms
* broadcastInRoom
* broadcast
* getSocket
* on

###getRooms
用户获取当前服务器上所有房间信息

参数：
无

返回值：
* rooms——所有房间名称的数组

###getSocket
通过socket的id获得socket实例

参数：
* id——socket的id

返回值：
* socket——WebSocket实例

###broadcastInRoom
在房间中广播消息

参数：
* room——被广播消息的房间名称
* data——消息的具体内容
* errorCb——广播失败时的回调函数

返回值：
无

###broadcast
向服务器上的所有用户广播消息

参数：
* data——消息的具体内容
* errorCb——广播失败时的回调函数

返回值：
无

###on
向服务器上的事件绑定处理器

参数：
* eventName——被绑定的事件名称
* callback——被绑定的事件触发时的回调函数

返回值：
无

##自定义事件
在SkyRTC中可以自定义事件，在前端页面使用WebSocket发送信息时，以如下JSON格式发送信息：
```javascript
{
    "eventName": "yourOwnEventName",
    "data": {
        //自定义事件数据
    }
}
```

在后台通过监听同名事件来进行处理：
```javascript
SkyRTC.rtc.on("yourOwnEventName", function(data){
    //data将是前台所传输的数据
});
```

自定义事件请不要与上述SkyRTC原生事件重名

##项目完整实例
[SkyRTC-demo](https://github.com/LingyuCoder/SkyRTC-demo)
