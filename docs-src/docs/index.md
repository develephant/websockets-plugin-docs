# ![logo-sm](img/logo192.png)

<h2>WebSocket Client Plugin for Corona Games and Apps</h2>

The __WebSockets__ plugin provides a WebSocket client for your [Corona](https://coronalabs.com/) projects with a simple to use API. Supports both secure and standard connections*.

<small>*Supports the WebSocket protocol (RFC6455)</small>

## Get the Plugin

If you don't already have it, get the __WebSockets__ plugin from the [Corona Marketplace](https://marketplace.coronalabs.com/plugin/websockets).

## Enable the Plugin

Enable the plugin by adding the following entries to the __plugins__ table of your projects __build.settings__ file:

```
settings =
{
    plugins =
    {
      ["plugin.websockets"] = {
          publisherId = "com.develephant"
      },
      ["plugin.openssl"] = {
        publisherId = "com.coronalabs",
      },
      ["plugin.bit"] = {
        publisherId = "com.coronalabs",
      }
    },
}
```

## Require the Plugin

In your code file make sure to `require` the plugin.

```lua
local WebSockets = require("plugin.websockets")
```

<i class="fas fa-thumbs-up fa-fw" style="color: yellowgreen;"></i> You're ready to start using WebSockets! Move on to the __[API](/api/)__ section to learn more.