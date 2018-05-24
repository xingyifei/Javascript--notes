## socket.io

* 客户端配置

  * `cnpm install socket.io-client -S`

  * ```javascript
    import io from 'socket.io-client`

    const socket = io('192.168.X.X.X') // 连接socket服务器
    export default function on(key, cb) {
      // todo 过滤数据
      socket.on(key, cb)
    }

    export default function emit(key, data) {
      // todo 过滤数据
      socket.emit(key, data)
    }
    ```

* 服务器配置

  * `cnpm install socket.io -S `

  * ```javascript
    var socketio = require("socket.io")
    var pubsocket = null

    var server = app.listen(port, function() {
      console.log('Server listening on http://localhost:' + port);
    })

    socketio.listen(server).on('connection', function (socket) {
      console.log('socket连接成功')
      pubsocket = socket
    })

    app.route('/update')
      .get(function(req, res) {
          pubsocket.emit('TypeOne', 'data')
      });

    ```

    ​