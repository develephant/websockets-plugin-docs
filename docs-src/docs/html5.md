The __[WebSockets](https://marketplace.coronalabs.com/corona-plugins/websockets)__ plugin is for device builds only and does not support HTML5 builds. If you would like to add support for HTML5 builds you can use the __[Corona HTML5 WebSockets Plugin](https://github.com/develephant/corona-html5-websockets-plugin)__.

## Hybrid Builds

You can create hybrid builds, supporting both devices and HTML5. The following demonstrates how to do this in your Corona project.

First make sure to download the __[Corona HTML5 WebSockets Plugin](https://github.com/develephant/corona-html5-websockets-plugin)__ and add it to your project as per the instructions found at the link.

## Integration

You can handle the slight differences between the device and HTML5 plugin using a "platform" check. Both plugin types support the same listener function setup.

<i class="fas fa-info-circle fa-fw" style="color: yellowgreen;"></i> __The major difference between the device and HTML5 plugin methods is the use of the colon (:) versus dot (.) syntax.__

### Hybrid Setup

```lua
local platform = system.getInfo("platform")
local ws

--require the proper plugin
if platform == "html5" then
  ws = require("websockets")
else
  ws = require("plugin.websockets")
end

--create the websockets listener
local function WsListener(event)
  if event.type == ws.ONOPEN then
    print('connected')
  elseif event.type == ws.ONMESSAGE then
    print('message')
  elseif event.type == ws.ONCLOSE then
    print('disconnected')
  elseif event.type == ws.ONERROR then
    print('error')
  end
end

--add the event listener
if platform == "html5" then
  ws.addEventListener( WsListener )
  ws.connect('ws://<websocket-endpoint>')
else
  ws:addEventListener(ws.WSEVENT, WsListener)
  ws:connect('ws://<websocket-endpoint>')
end
```

### Sending Data

To send data over the WebSocket connection in a hybrid build, it is easiest if you create a function specifically for sending.

```lua
function send( data )
  if platform == "html5" then
    ws.send( data )
  else
    ws:send( data )
  end
end

send("Hello WebSockets!")
```
