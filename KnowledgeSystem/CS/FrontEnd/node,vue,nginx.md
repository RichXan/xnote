node：node.js是服务器端的语言，是基于Chrome V8引擎的JavaScript运行环境，是开源的开平台独立于浏览器的，可以在服务器端直接运行JavaScript代码的运行时环境。

vue：而vue是前端的JavaScript框架，基于HTML的模板语法，是一个轻量级易学习的框架。它提供了一套完整的学习方案，用于构建用户界面，包括模板，数据绑定，路由，组件等功能。它的目标是通过尽可能简单的API实现数据的绑定和视图效果的组件。

nginx：node自己本身可以作为[服务器](https://cloud.tencent.com/product/cvm?from=10680)进行驱动，但是node本身对文件的处理能力并不是很好，所以当我们的生产环境中应尽量使用nginx来处理静态的资源以及反向代理，同时也解决了node分布式以及[负载均衡](https://cloud.tencent.com/product/clb?from=10680)的相关问题。