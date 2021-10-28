---
[TOC]

# 页面生命周期


|名称    |状态|
| --- | --- | 
|initState    |插入渲染树时调用，只调用一次|
|didChangeDependencies    |state依赖的对象发生变|化时调用|
|didUpdateWidget    |组件状态改变时候调用，可能会调用多次|
|build    |构建Widget时调用|
|deactivate    |当移除渲染树的时候调用|
|dispose|    组件即将销毁时调用|

生命周期是一个组件加载到卸载的整个周期

![aa447b821ec5ddbb321689a7110bbe73.jpeg](evernotecid://B7BD3DA3-2F70-41EF-B773-EB2AD734CB60/appyinxiangcom/27337826/ENResource/p1357)

大致可以分为3个阶段：

- 初始化
- 状态变化
- 组件移除

## 初始化

State初始化时会依次执行 ： 构造函数 > initState > didChangeDependencies > Widget build , 此时页面加载完成。
### 构造函数
### initState
### didChangeDependencies
### build函数

## 状态变化

### build

调用次数：多次

初始化之后开始绘制界面，当setState触发的时候会再次被调用

### didUpdateWidget

Called whenever the widget configuration changes.

祖先节点rebuild widget时调用 .当组件的状态改变的时候就会调用didUpdateWidget.

理论上setState的时候会调用，但我实际操作的时候发现只是做setState的操作的时候没有调用这个方法。而在我改变代码hot reload时候会调用 didUpdateWidget 并执行 build…

![566da0c7cfde63f6c1c95862427958ac.png](evernotecid://B7BD3DA3-2F70-41EF-B773-EB2AD734CB60/appyinxiangcom/27337826/ENResource/p1358)

实际上这里flutter框架会创建一个新的Widget,绑定本State，并在这个函数中传递老的Widget。
这个函数一般用于比较新、老Widget，看看哪些属性改变了，并对State做一些调整。

需要注意的是，涉及到controller的变更，需要在这个函数中移除老的controller的监听，并创建新controller的监听。


## 组件移除

组件移除，例如页面销毁的时候会依次执行：deactivate > dispose

### deactivate

Called when this object is removed from the tree.

在dispose之前，会调用这个函数。当组件卸载时会先一步dispose调用。

### dispose

Called when this object is removed from the tree permanently.

调用次数：1次

一旦到这个阶段，组件就要被销毁了，这个函数一般会移除监听，清理环境。

# App生命周期

需要指出的是如果想要知道App的生命周期,那么需要

通过 ` with WidgetsBindingObserver`
的 `didChangeAppLifecycleState 来获取`。

通过该接口可以获取是生命周期在AppLifecycleState类中。常用状态包含如下几个：
```dart

class _MyHomePageState extends State<MyHomePage> with WidgetsBindingObserver {


  //App 生命周期
  void didChangeAppLifecycleState(AppLifecycleState state) async {
    if (state == AppLifecycleState.resumed) {
      print("resumed");
    } else if (state == AppLifecycleState.inactive) {
      print("inactive");
    } else if (state == AppLifecycleState.paused) {
      print("paused");
    } else if (state == AppLifecycleState.detached) {
      print("detached");
    }
  }
}
```



|名称    |状态|
| --- | --- |
|resumed    |可见并能响应用户的输入|
|inactive    |处在并不活动状态，无法处理用户响应|
|paused    |不可见并不能响应用户的输入，但是在后台继续活动中|
