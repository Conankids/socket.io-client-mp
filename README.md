
## 微信小程序 socket.io 客户端

这是一个基于 socket.io 官方版本修改的，可以在微信小程序上使用的 socket.io 客户端，方便利用 socket.io 的更多特性，以及与 [egg.js](https://eggjs.org/) 等暂时没有提供原生 WebSocket 实现的服务器程序对接.

使用 [engine.io-client-mp](https://github.com/mdluo/engine.io-client-mp) 作为底层传输协议来支持小程序的 [WebSocket API](https://mp.weixin.qq.com/debug/wxadoc/dev/api/network-socket.html).

- 大小仅为 12 KB (gziped)，完整保留 [socket.io-client](https://github.com/socketio/socket.io-client) 的 WebSocket 版本的功能
- 支持二进制格式传输（Binary/ArrayBuffer）、保持长连接（小程序默认约 60s 自动关闭连接）
- 仅修改了引用的 `engine.io-client` 的地址，业务代码无任何修改，所以可以跟官方版本保持同步更新

## 使用方法

下载 `dist/socket.io.slim.js` 到小程序目录（建议使用 slim 版本，移除了跟小程序平台无关的库，尺寸更小）:

```js
const io = require('../../libs/socket.io-client-mp/socket.io.slim')

const socket = io('ws://192.168.1.100:7001', {
  transports: ['websocket'], // 此项必须设置
  transportOptions: {
    websocket: {
      extraHeaders: { // 可选，小程序 WebSocket API 允许设置的 header
        'x-client-id': 'abc',
      },
      protocols: ['custom-protocol'] // 可选，子协议数组
    },
  },
})

socket.on('connect', () => {
  console.log('socket.io connected!')
  socket.emit('hello/world', 'hello world!')
})
```

初始化时必须设置 transports 为 `['websocket']` ，为了保持跟原生 API 一致，[小程序 API](https://mp.weixin.qq.com/debug/wxadoc/dev/api/network-socket.html#wxconnectsocketobject) 里的 `method` 暂不支持，其他选项参考上面的示例代码.

## API

See [API](/docs/API.md)

## License

[MIT](/LICENSE)
