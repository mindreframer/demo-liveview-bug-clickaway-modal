# Demonstration of a bug with Phoenix 1.6.6

```bash
## phx_new 1.6.6
mix archive.install hex phx_new
mix phx.new demo --live
mix phx.gen.live Accounts User users name:string age:integer roles:array:string
```

### Issue:

With visible modal dialog for user creation, clicking away results in following JS error in the browser console:

```js
phoenix_html.js:56 Uncaught RangeError: Maximum call stack size exceeded.
    at phoenix_html.js:56:20
    at Object.dispatchEvent (dom.js:255:12)
    at Object.exec_dispatch (js.js:28:9)
    at js.js:14:22
    at Array.forEach (<anonymous>)
    at js.js:13:40
    at Array.forEach (<anonymous>)
    at Object.exec (js.js:9:14)
    at live_socket.js:610:16
    at live_socket.js:389:33
```

The same error happens when pressing the `ESC` button on keyboard.

Removing `phx-click-away={JS.dispatch("click", to: "#close")}` from DemoWeb.LiveHelpers - module fixes the `ESC` button issue. It seems that dispatch is getting caught in a recursive loop, yet I could not figure out how.
