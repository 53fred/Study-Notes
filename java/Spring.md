# 1. Bean作用域
Spring 容器在初始化一个 Bean 实例时，同时会指定该实例的作用域。Spring 5 支持以下 6 种作用域。   
## 1）singleton  
默认值，单例模式，表示在 Spring 容器中只有一个 Bean 实例，Bean 以单例的方式存在。   
## 2）prototype
原型模式，表示每次通过 Spring 容器获取 Bean 时，容器都会创建一个 Bean 实例。
## 3）request
每次 HTTP 请求，容器都会创建一个 Bean 实例。该作用域只在当前 HTTP Request 内有效。
## 4）session
同一个 HTTP Session 共享一个 Bean 实例，不同的 Session 使用不同的 Bean 实例。该作用域仅在当前 HTTP Session 内有效。
## 5）application
同一个 Web 应用共享一个 Bean 实例，该作用域在当前 ServletContext 内有效。
## 6）websocket
websocket 的作用域是 WebSocket ，即在整个 WebSocket 中有效。

# 2. Bean生命周期
