Before using the __WebSockets__ plugin you must `require` it in your code file:

```lua
local WebSockets = require("plugin.websockets")
```

## Methods

### new

Creates a new WebSocket client.

```lua
WebSockets.new()
```

__Arguments__

_This method takes no arguments._

__Returns__

A new WebSocket client.

__Example__

```lua
local ws = WebSockets.new()
```

---

### addEventListener

Add the WebSocket event handler.

```lua
ws:addEventListener(event, handler)
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|event|The event to start listening for. Must be `WSEVENT`.|_Constant_|__Y__|
|handler|The function that will be called on a WebSocket event.|_Function_|__Y__|

__Example__

```lua
...

local function WsHandler(event)
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

ws:addEventListener(ws.WSEVENT, WsHandler)

...
```

<i class="fas fa-info-circle fa-fw" style="color: yellowgreen;"></i> See __[Event Constants](/api/#event-constants)__ for more details about each event type.

---

### removeEventListener

Remove the WebSocket event handler.

```lua
ws:removeEventListener(event, handler)
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|event|The event to stop listening for. Must be `WSEVENT`.|_Constant_|__Y__|
|handler|The function that is being called on a WebSocket event.|_Function_|__Y__|

__Example__

_Assuming the event handler in the `addEventListener` example._

```lua
ws:removeEventListener(ws.WSEVENT, WsHandler)
```

---

### connect

Connect the WebSocket object to a WebSocket endpoint.

```lua
ws:connect(uri[, options])
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|uri|The WebSocket endpoint to connect to. Supports `ws://` and `wss://` protocols.|_String_|__Y__|
|options|An optional table of options for the connection (see below).|_Table_|__N__|

__Options Table__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|port|The port for the WebSocket endpoint. Defaults to __80__ for `ws://` and __443__ for `wss://` protocols.|_Number_|__N__|
|tls|The TLS protocol version to use for a secure socket (`wss://`). Defaults to __TLS_1_2__.|[_Constant_](#tls-constants)|__N__|

__Example__

```lua
ws:connect('ws://demos.kaazing.com/echo')
```

<i class="fas fa-exclamation-triangle fa-fw" style="color: yellowgreen;"></i> Make sure your WebSocket event handler is set up before calling this method. See __[addEventListener](/api/#addeventlistener)__.

---

### disconnect

Disconnect the WebSocket object from the endpoint.

```lua
ws:disconnect()
```

__Arguments__

_This method takes no arguments._

---

### send

Send a message over the WebSocket connection.

```lua
ws:send(data)
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|data|The message data to send over the connection.|_String_|__Y__|

__Example__

```lua
ws:send("Hello WebSocket server")
```

---

## Event Constants

The following event constants are used in the WebSocket event listener. Some events will include addtional data that can be accessed from the `event` object. See also: __[addEventListener](/api/#addeventlistener)__.

### ONOPEN

The WebSocket connection is open and ready.

```lua
ws.ONOPEN
```

__Event Keys__

_This event contains no event keys._

__Example__

```lua
...

local function WsHandler(event)
  if event.type == ws.ONOPEN then
    print('connected')
  end
end

...
```

---

### ONMESSAGE

A WebSocket message has been received.

```lua
ws.ONMESSAGE
```

__Event Keys__

  - `data`

__Example__

```lua
...

local function WsHandler(event)
  if event.type == ws.ONMESSAGE then
    print(event.data) --> message data
  end
end

...
```

---

### ONCLOSE

The WebSocket connection has closed and is no longer available.

```lua
ws.ONCLOSE
```

__Event Keys__

  - `code`
  - `reason`

__Example__

```lua
...

local function WsHandler(event)
  if event.type == ws.ONCLOSE then
    print(event.code, event.reason) --> closure code and reason
  end
end

...
```

---

### ONERROR

A WebSocket error has occurred. This will generally close the connection.

```lua
ws.ONERROR
```

__Event Keys__

  - `code`
  - `reason`

__Example__

```lua
...

local function WsHandler(event)
  if event.type == ws.ONERROR then
    print(event.code, event.reason) --> error code and reason
  end
end

...
```

---

## TLS Constants

<i class="fas fa-exclamation-triangle fa-fw" style="color: yellowgreen;"></i> By default a secure (`wss://`) client connection uses __TLS 1.2__.

The following constants are available for backward compatibility or older systems, and are passed in through the `options` table (see [connect](#connect)).

### TLS_1_0

Use TLS version 1.0 as the secure protocol.

```lua
ws.TLS_1_0
```

---

### TLS_1_1

Use TLS version 1.1 as the secure protocol.

```lua
ws.TLS_1_1
```

---

## Setup Example

### Simple Connect

```lua
local WebSockets = require("plugin.websockets")

local ws = WebSockets.new()

local function WsHandler(event)
  if event.type == ws.ONOPEN then
    print('connected')
  elseif event.type == ws.ONMESSAGE then
    print('message', event.data)
  elseif event.type == ws.ONCLOSE then
    print('disconnected', event.code, event.reason)
  elseif event.type == ws.ONERROR then
    print('error', event.code, event.reason)
  end
end

ws:addEventListener(ws.WSEVENT, WsHandler)
ws:connect('ws://demos.kaazing.com/echo')
```

### Using Options

```lua
....

ws:connect('wss://demos.kaazing.com/echo', {
  tls = ws.TLS_1_0
})
```

<i class="fas fa-cloud-download-alt fa-fw" style="color: yellowgreen;"></i> Download the demo project from the __[Demo](/demo/)__ section for an even more detailed example.