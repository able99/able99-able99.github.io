---
layout: post
title: 初识 bnorth
category: cbnorth
weight: 3
tags: [react, bnorth, 大前端]
---

首先了解下 bnorth 工具集的组成和几个重要概念。

## 组成成分

<svg width="480" height="320" preserveAspectRatio="none" viewBox="0 0 640 480" xmlns="http://www.w3.org/2000/svg" xmlns:svg="http://www.w3.org/2000/svg">
 <g>
  <title>Layer 1</title>
  <rect fill="#ffffff" stroke="#000000" x="38.5" y="26" width="71" height="58" id="svg_1"/>
  <line id="svg_2" y2="103" x2="501" y1="102" x1="20" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke="#000000" fill="none"/>
  <line stroke="#000000" id="svg_3" y2="342.99999" x2="500" y1="104" x1="500" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" fill="none"/>
  <text xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" id="svg_4" y="64" x="74" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">cli</text>
  <rect id="svg_5" fill="#ffffff" stroke="#000000" x="526.5" y="109" width="71" height="58"/>
  <text style="cursor: move;" id="svg_6" xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" y="147" x="558.5" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">build</text>
  <rect stroke="#000000" id="svg_7" fill="#ffffff" x="38.5" y="396" width="88" height="58"/>
  <text id="svg_8" xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" y="431" x="80.5" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">browser</text>
  <line id="svg_9" y2="341.5" x2="499.5" y1="340.5" x1="18.5" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke="#000000" fill="none"/>
  <rect stroke="#000000" id="svg_10" fill="#ffffff" x="154.49999" y="396" width="92" height="58"/>
  <text id="svg_11" xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" y="431" x="200.5" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">pc</text>
  <rect stroke="#000000" id="svg_12" fill="#ffffff" x="272.5" y="396" width="84" height="58"/>
  <text id="svg_13" xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" y="431" x="314.5" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">android</text>
  <rect stroke="#000000" id="svg_14" fill="#ffffff" x="381" y="396" width="89" height="58"/>
  <text transform="matrix(1,0,0,1,0,0) " id="svg_15" xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" y="431" x="427.5" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">ios</text>
  <text xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" id="svg_16" y="428" x="494" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">...</text>
  <rect stroke="#000000" id="svg_17" fill="#ffffff" x="161.5" y="121" width="127" height="128.00001"/>
  <text id="svg_18" xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" y="158" x="207.5" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">core</text>
  <rect stroke="#000000" id="svg_19" fill="#ffffff" x="38.5" y="121" width="84" height="128"/>
  <text id="svg_20" xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" y="158" x="74.5" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">plugin</text>
  <rect stroke="#000000" id="svg_22" fill="#ffffff" x="332" y="121" width="138" height="58"/>
  <text id="svg_23" xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" y="158" x="400.8125" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">components</text>
  <rect id="svg_24" stroke="#000000" fill="#ffffff" x="332" y="191" width="138" height="58"/>
  <text transform="matrix(1,0,0,1,0,0) " id="svg_25" xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" y="228" x="377" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">rich.css</text>
  <rect stroke="#000000" id="svg_26" fill="#ffffff" x="161.5" y="265" width="122.99999" height="58"/>
  <text style="cursor: move;" id="svg_27" xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" y="302" x="217.5" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">eletron</text>
  <rect stroke="#000000" id="svg_28" fill="#ffffff" x="332" y="264" width="134.99999" height="58"/>
  <text id="svg_29" xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" y="301" x="381.10938" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">cordova</text>
 </g>
</svg>

- core，components，rich.css 是工具集中应用构建的主要部分
 
    + core：是构建应用的核心，是应用的入口。core 实际上是一个事件驱动的 mvc 框架，实现了 App 类。提供了事件管理，页面管理，数据管理，插件管理，等核心功能
    + components：提供了丰富的组件，包括布局组件 View，Panel 等，小组件 Button，Icon等，动画组件 AnimationFade，AnimationCollapse 等，以及复杂组件 modal，picker 等等
    + rich.css：是一个可定制的，全面覆盖的 css 样式库

- cli，build，doc-reader，是 bnorth 工具集中的工具，提供了内部使用的功能

    + cli：bnorth 脚手架
    + build：build 是 bnorth 体系内部使用同一的编译和运行库，实现了代码编译，运行调试服务器等功能。
    + doc-reader：为 bnorth 工具集提供统一的 api 文档生成方案。

- cordova，eletron，是 bnorth 工具集中的打包工具，提供了不同平台本地应用生成的能力

    + cordova：提供移动端（android，ios） hybird app 打包的相关功能
    + eletron：提供 pc 端 app 打包的相关功能，暂未完成

- plugin 指 bnorth 格式的插件，无限扩展了 bnorth 能力


## 核心的基本构造

core 是应用构建的核心，也是应用的入口，应用首先需要实例化 App 类，然后通过 start 方法启动应用。start 方法触发一系列的事件，各个模块和插件在收到事件后，完成初始化和相关任务，最终完成应用启动。core 是由 App 类和几个由 App 初始化并挂载在 App 上的模块组成。
<svg width="480" height="320" preserveAspectRatio="none" viewBox="0 0 640 480" xmlns="http://www.w3.org/2000/svg" xmlns:svg="http://www.w3.org/2000/svg">
 <g>
  <title>Layer 1</title>
  <rect stroke="#000000" fill="#ffffff" x="114" y="22" width="72" height="437.99999" id="svg_1"/>
  <text fill="#000000" stroke="#000000" stroke-width="0" x="149" y="60" id="svg_2" font-size="24" font-family="serif" text-anchor="middle" xml:space="preserve">App</text>
  <rect fill="#ffffff" stroke="#000000" x="234" y="24" width="107" height="46" id="svg_4"/>
  <text fill="#000000" stroke="#000000" stroke-width="0" x="269.42188" y="55" id="svg_5" font-size="24" font-family="serif" text-anchor="middle" xml:space="preserve">Event</text>
  <rect id="svg_3" fill="#ffffff" stroke="#000000" x="234.5" y="76" width="107" height="46"/>
  <text transform="matrix(1,0,0,1,0,0) " id="svg_6" fill="#000000" stroke="#000000" stroke-width="0" x="273.79688" y="107" font-size="24" font-family="serif" text-anchor="middle" xml:space="preserve">Router</text>
  <rect id="svg_9" fill="#ffffff" stroke="#000000" x="234.5" y="129" width="107" height="46"/>
  <text id="svg_10" fill="#000000" stroke="#000000" stroke-width="0" x="275.32813" y="160" font-size="24" font-family="serif" text-anchor="middle" xml:space="preserve">Render</text>
  <rect id="svg_11" fill="#ffffff" stroke="#000000" x="234.5" y="184" width="107" height="46"/>
  <text transform="matrix(1,0,0,1,0,0) " id="svg_12" fill="#000000" stroke="#000000" stroke-width="0" x="278.96875" y="215" font-size="24" font-family="serif" text-anchor="middle" xml:space="preserve">Context</text>
  <rect id="svg_13" fill="#ffffff" stroke="#000000" x="235.5" y="240" width="107" height="46"/>
  <text id="svg_14" fill="#000000" stroke="#000000" stroke-width="0" x="271.98438" y="271" font-size="24" font-family="serif" text-anchor="middle" xml:space="preserve">Plugin</text>
  <rect id="svg_15" fill="#ffffff" stroke="#000000" x="235.5" y="296" width="107" height="46"/>
  <text id="svg_16" fill="#000000" stroke="#000000" stroke-width="0" x="259.09375" y="327" font-size="24" font-family="serif" text-anchor="middle" xml:space="preserve">Log</text>
  <rect id="svg_17" fill="#ffffff" stroke="#000000" x="236.5" y="358" width="107" height="46"/>
  <text transform="matrix(1,0,0,1,0,0) " id="svg_18" fill="#000000" stroke="#000000" stroke-width="0" x="288.5" y="389" font-size="24" font-family="serif" text-anchor="middle" xml:space="preserve">Keyborad</text>
  <rect id="svg_19" fill="#ffffff" stroke="#000000" x="396.5" y="76" width="107" height="46"/>
  <text id="svg_20" fill="#000000" stroke="#000000" stroke-width="0" x="424.79688" y="107" font-size="24" font-family="serif" text-anchor="middle" xml:space="preserve">Page</text>
  <rect id="svg_21" fill="#ffffff" stroke="#000000" x="397.5" y="184" width="107" height="46"/>
  <text id="svg_22" fill="#000000" stroke="#000000" stroke-width="0" x="425.79688" y="215" font-size="24" font-family="serif" text-anchor="middle" xml:space="preserve">State</text>
  <rect id="svg_23" fill="#ffffff" stroke="#000000" x="236.5" y="415" width="107" height="46"/>
  <text id="svg_24" fill="#000000" stroke="#000000" stroke-width="0" x="263.73438" y="446" font-size="24" font-family="serif" text-anchor="middle" xml:space="preserve">Utils</text>
 </g>
</svg>

- App：应用的主类，其他模块都是挂载在 app 的实例上
- Event：事件管理模块
- Router：路由管理模块
- Render：负责 react 根组件管理和 dom 操作能力 
- Context：数据管理模块
- Plugin：插件管理模块
- Log：日志管理模块
- Keyboard：键盘事件管理模块
- Utils：工具函数集
- Page：关于具体页面的类
- State：关于具体数据单元的类

## 几个概念

1. 事件

    bnorth 以事件驱动的方式运行。应用启动中有一系列启动事件，地址变化时有路由事件，页面管理有页面事件等等。

1. route 与路由表

    路由是页面配置的静态信息，包括指定路由对应的页面代码，指定页面标题等信息。路由表是地址与路由的一一对应关系。

1. page

    page 即页面，由路由管理根据地址和路由信息构造并显示的动态内容。页面与路由是多对一的关系，比如地址中多次出现同一路由时，页面将被构造多次。页面由 ViewComponent 和 controller 组成，负责页面的显示和页面的逻辑。每个页面管理自己的页面事件和数据单元

1. ViewComponent

    ViewComponent 是被注入了页面相关属性的普通 React 组件，由用户根据具体业务编写。注入的信息包括，app，page，controller，路由信息和指定的数据单元

1. controller

    controller 是一个对象或者返回对象的函数，由用户根据业务需要编写。controller 为 page 设置了 数据单元，事件处理函数，action 函数。也可以设置普通的变量和函数，供 ViewComponent 调用

1. state 与 context

    context 在 bnorth 中完成数据管理的功能。state 是数据处理的单元。对数据单元操作，可能会引起数据管理器处理数据流动，并使响应页面刷新

1. action

    action 是页面响应用户操作的函数，可能触发数据流动或者路由变更
