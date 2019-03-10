# Web Worker

### 简介 & 背景

众所周知「浏览器的的每个每个网页在单进程中执行，而网页中的 JS 的普通运算在单线程中执行」  
那么 JS 高负荷运算的时候，单线程就吃不消  

那么 **救世主** 来了

Web Worker 独立与当前网页的 JS 单线程，自己独占一个线程  
Web Worker 可以理解为，`网络的苦力工`，可以处理繁重的「任务、运算」  
Web Worker 线程和 JS 线程，二者之间可使用 `postMessage` 双向通信

### 一个乘法 DEMO

[点我](http://hangyangws.win/demos/src/web_worker/)

### 关闭 worker 实例（杀掉线程）

> 官方推荐是，使用 `self.close` 进行内部的自动关闭，这样能防止, 意外关闭正在运行的 `worker`

1. workerObj.terminate(): 在外部终结该 worker
1. self.close(): 在 worker 内部自动终结

### 作用域

全局索引就是 `self` 和 `this` ，来获取 worker 自带的方法

### 注意事项

worker 引用的就是 js 文件  
由于 worker 是独立的线程，他和 main js threading 有很大区别的

worker **访问权限** 有

1. window.navigator 相关属性和方法
1. 只读的 window.location 内容.
1. window.XMLHttpRequest （卧槽… 可以访问这个那就不得了， worker 就可以利用 ajax 来和后台进行通信了）
1. setInterval 相关时间函数

**同域限制**

worker 在访问时, 只能是在同一 host 下才行. 即, 你的 worker 只能处于指定目录下的 path 中  
所以：本地调试 file://xxx 的话, 也不能使用 worker

在当前的 worker 加载其他库文件

可以使用 **importScripts** 来导入（自动缓存文件）  
`importScripts('example1.js','example2.js')`

### 使用场景

根据 worker 独立线程特性，可以把大规模数据并发， I/O 操作都可以交给他  
总的来说有这些场景

- 懒加载数据
- 文本分析
- 流媒体数据处理
- web database 的更新

---

# Service Worker

[参考链接1](https://juejin.im/post/591028fc2f301e006c291c4b)  
[参考链接2](https://segmentfault.com/a/1190000006678016)  

### 概述

使用 Service Worker 可以使你的应用先访问本地缓存  
所以在离线状态时，在没有通过网络接收到更多的数据前，仍可以提供基本的功能体验  
这称之为 Offline First

它使得开发者可以支持非常好的离线体验，给予开发者完全控制离线数据的能力  
service worker 是一段运行在浏览器后台进程里的脚本，它独立于当前页面  
原生 APP 有 Web 通常所不具备的富离线体验、定时的静默更新、消息通知推送……  
而 Service workers 标准让在 Web 拥有这些功能成为可能

Service workers 由 cache 和 [Worker](https://www.villainhr.com/page/2016/08/22/Web%20Worker)二部分组成  
不会阻碍当前 js 进程的执行，确保性能第一  
service worker 函数像一个代理服务器一样，允许你修改请求和响应，用他们的缓存替代它们等等

SW 是基于 HTTPS 的，如果你的网站不是 HTTPS，那么基本上你也别想了 SW  
这估计造成了一个困难，即，我调试 SW 的时候咋办  
解决办法也是有的，使用 charles 或者 fildder 完成域名映射即可

serviceWorker 是挂载到 navigator 下的对象


### 注册失败原因

1. 你没有在 HTTPS 下运行你的程序
1. service worker 文件的地址没有写对（需要相对于 origin , 而不是 app 的根目录）
1. service worker 注册的 scope 大于他自己的作用域

### 注意事项

- service worker，它工作在 worker context，所以它不能直接操作 DOM ，但是可以通过 postMessage 与页面之间通信，如果需要的话，让页面自己去操作DOM
- Service worker 是一个可编程的**网络代理**，允许开发者控制页面上处理的网络请求
- 在不被使用的时候，会自己终止，而当它再次被用到的时候，会被重新激活

> 所以你不能依赖于 service worker 的 onfetch 和 onmessage 的处理函数中的全局状态  
如果你想要保存一些持久化的信息，你可以在 service worker 里使用 IndexedDB API

- service work 会收到他域下的所有fetch事件，所以注册时，要注意路径

- 打开浏览器支持（设置后都需要重启浏览器）

> Firefox Nightly: `about:config` 设置 dom.serviceWorkers.enabled 的值为 true  
Chrome Canary: `chrome://flags` 开启 experimental-web-platform-features (有些特性在Chrome中没有默认开放支持）  
Opera: `opera://flags` 开启 ServiceWorker 支持

- 需要通过HTTPS来访问你的页面代码（安全）[可以通过git pages 测试]
- localStorage 跟  service worker 的 cache 工作原理很类似，但是它是同步的，所以不允许在  service workers 内使用
-  IndexedDB 可以在  service worker 内做数据存储
- 还不能做到完全的离线，比如需要更新数据库的请求

### 生命周期

![lifycycle](../img/lifycycle.png)

要让一个 service worker 在你的网站上生效，你需要先在你的网页中**注册**它  
浏览器会在后台默默启动一个 service worker 的**安装**过程  
安装过程中，浏览器会加载并缓存一些静态资源，如果**所有**的文件被缓存成功，则安装成功，如果有**任何**文件加载或缓存失败，则安装失败  
失败后会**重新尝试**

### 请求拦截之 fetch 事件

sw 内部的 fetch事件可以拦截 sw 作用域下面的所有请求  
然后我们可以随便发挥，然后使用 event.respondWith 返回请求响应

![fetch](../img/fetch.png)

SW 支持的事件

1. install
1. activate
1. message
1. fetch
1. sync
1. push

后台进程: SW 就是一个 worker 独立于当前网页进程。  
网络代理: SW 可以用来代理请求，缓存文件  
灵活触发: 需要的时候吊起，不需要的时候睡眠（这个是个坑）  
异步控制: SW 内部使用 promise 来进行控制。

### 开始使用

浏览器的资源、网页的内存有限，再开个 SW（这个很大的），易闪退  
为减少性能损耗，一般直接在 onload 里注册 SW

```javascript
if ('serviceWorker' in navigator) {
  window.addEventListener('load', function() {
    // 注册（告诉浏览器你的 service worker 脚本在哪里）
    // 如果某个 service worker 已经被注册过，再次注册浏览器会自动忽略
    navigator.serviceWorker.register('/sw.js').then(function(registration) {
      // 注册成功
      console.log('SW作用域：', registration.scope);
    }).catch(function(err) {
      // 注册失败
      console.log('SW注册失败：', err);
    });
  });
}
```

### 一个 DEMO

[点我](https://hangyangws.github.com/demos/src/service_worker/)

### cache

[link](https://developer.mozilla.org/zh-CN/docs/Web/API/Cache)

### 查看

打开 `chrome://inspect/#service-workers` 查看当前浏览器中正在注册的 SW  
`chrome://serviceworker-internals` 用来查看当前浏览器中所有注册好的 SW

本地 https 简单构建 `python` 脚本

python 2  
`python -m SimpleHTTPServer 8001`
python 3  
`python3 -m http.server 8001`

### 浏览器支持

[点我](https://jakearchibald.github.io/isserviceworkerready/)

### 使用分析

什么时候开启 SW ? window.onload 可靠吗？  
例如，[Google I/O 2016 网络应用](https://events.google.com/io2016/)在过渡到主屏幕前先显示一个简短的动画。然后发现，在显示动画期间启动 SW 会导致低端移动设备出现卡顿
