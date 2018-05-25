# Axe

Axe is all the reinforcement this army needs.

## Axe 框架

`Axe`是一个iOS的业务组件化框架，通过三大基础组件，来规定所有业务组件之间的交互方式， 三大基础组件为 ： 

* Router : 路由模块，根据URL获取界面或者进行跳转
* Event : 事件通知， 是业务组件之间的主要交互方式
* Data :  公共数据，以及业务组件之前传递数据的媒介

相对于一般的组件化方案，我们多了一个`Event`和`Data`, 这两个组件的出现，使跨语言的统一业务开发模式的统一成为了可能 ，即当我们规定好三种路由、事件和数据后，`h5`的模块和`react-native`的模块可以和原生模块一样通过`Axe`框架与其他业务模块进行交互。

## Axe系统

`Axe`系统分为三层：

* 组件化 ： 使用`Axe`框架，进行业务组件拆分，实现组件化
* 动态化 ： 基于`Axe`框架，做动态化的扩展，以提高应用的组件动态化能力
* 平台化 ： 基于`Axe`框架，实现组件开发管理的平台化。

## 生态系统

| Project  | Description |
|---------|--------|
| [axe-react](https://github.com/axe-org/axe-react)          | 为React Native提供的`Axe`接口 |
| [axe-js](https://github.com/axe-org/axe-js)          | 为H5页面提供的`Axe`接口 |
| [offline-pack-server](https://github.com/axe-org/offline-pack-server)          | 离线包服务 |
| [offline-pack-ios](https://github.com/axe-org/offline-pack-ios)          | 离线包的iOS 实现 |
| [dynamic-router-server](https://github.com/axe-org/dynamic-router-server)          | 动态路由服务 |
| [axe-admin-server](https://github.com/axe-org/axe-admin-server)          | 组件化管理平台的后端 |
| [axe-admin-web](https://github.com/axe-org/axe-admin-web)          | 组件化管理平台的前端 |
| [axe-admin-docker](https://github.com/axe-org/axe-admin-docker)          | 使用docker镜像进行发布 |
| [fastlane](https://github.com/axe-org/fastlane)          | 组件化管理的辅助脚本 |