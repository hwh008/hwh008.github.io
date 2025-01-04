---
layout: post
title: 'angular.js 笔记'
date: 2015-01-02
author: huang
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: angular
---

## 页面和路由定义优先设计

所有页面模板可以包含在一个文件中：<br>
```
<script type="text/ng-template" id="index.html"></script>
<script type="text/ng-template" id="list.html"></script>
<script type="text/ng-template" id="content.html"></script>
```

使用路由系统需要导入ngRoute：<br>
```
app.config(['$routeProvider', function($routeProvider){}]);
```

当你的操作想跳转到别的地址时，使用 $location.path=。<br>


## 控制器和服务

理论上一个页面一个控制器。控制器里面定义模型数据值，可以直接渲染成页面结果，非常直接方便。<br>
```
app.controller('index_controller', function($scope){
	$scope.some_model = value;
});
```

一般模型的数据来源都是服务器端，从服务器端请求数据的抽象包装成服务的样子。一些在多个控制器共享的全局数据也可以装进服务里。<br>
```
app.factory('some_svr', function(){});
```

## 常用指令和服务

1. ng-app
1. ng-init 初始化一些模型值，调试或者配置的时候用；
1. ng-repeat dom元素重复
1. ng-show/ng-hide
1. ng-class
1. ng-click 控件点击行为
1. ng-controller
1. $location
1. $http ajax接口
1. $timeout 定时器
1. $q 异步委托promise

## 单机测试

因为有promise，所以可以无服务器的时候用promise模拟服务器响应结果，做单机demo很方便。以后接入服务器只需要修改服务的内部代码，而不需要调整结构。<br>
```
var df = $q.defer();
df.notify(value); //通知用户更新状态
df.resolve(value); //通知用户委托正常结束
df.reject(value); //通知用户委托异常结束

var p = df.promise;
p.then(fn1, fn2, fn3);
```

模拟的时候用$timeout延时后给调用者反馈结果。实际调用服务器接口的时候也可以通过promise将多个原子接口包装成为一个业务。