---
layout: post
title: bnorth 从零开始开发 h5 与 bybird app
category: bnorth
weight: 1
---

从环境搭建到工具选择，从开发实例到基本概念，用简单的方式给 bnorth 编程一个快照。仅为初次接触开发准备的教程，大牛请绕行。

编程没有那么难，做了15年的码农，写代码真的和搬砖一样的水到渠成了。但是编程真的很难，没写一句都涉及了太多的语法，知识点，约定，设计思路，新手很容易望而却步。甚至都不知道怎么开始编写，需要搭建编译环境，运行环境，调试环境。《从零开始》就是希望从一步步的步骤式的引入门，再逐步补充知识。只想做到从入门到熟练而不是从入门到放弃。

***注：*** 简单了解 html js css 知识即可阅读

## 选个趁手的工具

1. ***nodejs***，nodejs 将作坊式的 h5 开发，带进了有组织的，自动化的，先进的开发体验时代。[下载地址](https://nodejs.org/en/download/)
1. ***vscode***，也许听说 vi 才是程序员的利器, 但是那是配置定制好的 vi，vscode 已经配置了文件类型，自动完成，标签导航，工程浏览等基本功能。轻量级的IDE，友好的界面，强大的插件，配置 vi 插件，两全其美。[下载地址](https://code.visualstudio.com/Download)
1. ***chrome***，标准，强大，调试方便是作为运行和调试环境的首选。 [下载地址](https://www.google.cn/chrome/)

## 开发不要从零开始

开发要从底层开始，基础牢固。但是老程序员知道，站在前人肩膀上，将一部分工作外包给开源社群，让一部分代码是稳定的，是可以升级，然后集中注意力在业务上，稳定输出，高效完成任务。有了余力再去补充知识疆界。而补充知识疆界最好的方式，依然是读前人代码和分享自己设计的代码。编程是一个社群行为，而不是一个个人行为。

## 起步

仅仅是跟随步骤321：

1. 安装 nodejs
1. 安装 vscode
1. 新建文件夹比如 demo，在 vscode 中点击 【文件->打开工作区】，选择 demo 打开。然后 【查看->终端】打开 console 窗口进入命令行状态
1. 执行如下 2 条命令

    ```sh
    npx @bnorth/cli create
    npm start
    ```

几个步骤下来后，将自动启动 chrome 显示页面内容，并等待调试。
![demo preview]({{ "images/demo_preview.png" | prepend: site.baseurl }})

### 这条命令做了什么

`npx @bnorth/cli create` 执行后，将下载并执行 bnorth 提供的脚手架命令并执行 create 子命令。create 子命令在当前目录按照模板建立了工程，工程结构如下：

![demo dir structor]({{ "images/demo_dir_structor.png" | prepend: site.baseurl }})

1. package.json：是应用的配置文件
1. src 目录：是源代码所在目录
1. .npmignore 和 .gitignore 是 npm 和 git 的配置，与 bnorth 无关，可以忽略
1. src/index.js 文件是应用的入口文件。
    ```js
    import App from '@bnorth/core';
    import routes from './routes';
    import initStyle from './style';
    import initPlugins from './plugins';

    let app = new App({
      plugin:{
        onAppStartConfig: ()=>{app.router.setRoutes(routes) },
      },
    })

    initStyle(app);
    initPlugins(app);
    app.start();
    ```

    - 构建了 bnorth 核心的 App 类的实例 app
    - 在 App 构建参数中，声明了对 onAppStartConfig 事件的处理函数，并在处理函数中设置路由表
    - 调用 app.start() 启动应用

1. src/routes.js 文件是路由表。是地址 '/' 与 Home 文件建立了联系

    ```js
    import Home from './pages/home';

    export default {
      '/': Home,
    }
    ```

1. src/pages 是约定的页面放置的路径，其中 Home 文件是 Home 页面的代码，页面用 jsx 语法返回了一个 html 元素

    ```jsx
    import React from 'react';

    let Component = props=><h1>Home Page</h1>;

    Component.controller = (app,page)=>({
      onPageStart: ()=>console.log('onHomePageStart'),
    })

    export default Component;
    ```

1. src/components 是约定的自定义组件的路径
1. src/plugins 和 src/style 分别设置了引入的插件和引入的 css 样式

`npm start` 命令执行后：

1. 在调用 @bnorth/build 的 dev 子命令，编译 jsx 和 es6 等文件，并启动 server 和 默认浏览器，在浏览器中运行，并等待调试
1. 在入口文件中，执行 app.start() 函数，将触发一系列启动事件
1. 在 onAppStartConfig 事件中，设置了路由表，配置了 '/' 页面
1. 在 onAppStartRouter 事件中，将地址栏地址与页面进行映射和装配，并在 onAppStartRender 事件中 render Home 页面
1. 在 Home 页面中，返回一个 react 组件。其中 controller 是该页面的 控制层代码，暂时忽略。

## 开发一个简单的 h5 应用

接下来开发一个具体的 h5 小应用，看看稍微有一点业务逻辑的应用如何开发

### 效果

应用由两个页面组成，主页面和城市选择页面。
主页面显示默认城市的天气信息，默认城市为北京，见下图：
城市选择页面，显示城市列表，点击后回到主页面并显示选择城市的天气信息，同时修改默认城市，见下图：

<svg width="640" height="480" xmlns="http://www.w3.org/2000/svg" xmlns:svg="http://www.w3.org/2000/svg">
 <!-- Created with SVG-edit - http://svg-edit.googlecode.com/ -->
 <g>
  <title>Layer 1</title>
  <rect id="svg_3" height="277" width="187" y="115.5" x="360.94453" fill-opacity="0" stroke="#000000" fill="#000000"/>
  <rect id="svg_2" height="277" width="187" y="117" x="70.5" fill-opacity="0" stroke="#000000" fill="#000000"/>
  <text id="svg_18" fill="#000000" stroke="#000000" stroke-width="0" stroke-dasharray="null" stroke-linejoin="null" stroke-linecap="null" x="134" y="190.5" font-size="18" font-family="serif" text-anchor="middle" xml:space="preserve">最高温度：18</text>
  <text id="svg_19" fill="#000000" stroke="#000000" stroke-width="0" stroke-dasharray="null" stroke-linejoin="null" stroke-linecap="null" x="134" y="221.5" font-size="18" font-family="serif" text-anchor="middle" xml:space="preserve">最低温度：10</text>
  <text id="svg_20" fill="#000000" stroke="#000000" stroke-width="0" stroke-dasharray="null" stroke-linejoin="null" stroke-linecap="null" x="120.78125" y="252.5" font-size="18" font-family="serif" text-anchor="middle" xml:space="preserve">风力：4级</text>
  <rect fill="#cccccc" stroke="#000000" stroke-width="null" stroke-dasharray="null" stroke-linejoin="null" stroke-linecap="null" x="361" y="199" width="187" height="38" id="svg_13"/>
  <path fill="#cccccc" stroke="#000000" stroke-width="null" stroke-dasharray="null" stroke-linejoin="null" stroke-linecap="null" d="m531.99895,178.60694l-5.42634,-5.43198l3.57367,0l5.42634,5.43198l-5.42634,5.43198l-3.57367,0l5.42634,-5.43198z" id="svg_15"/>
  <text fill="#000000" stroke="#000000" stroke-width="0" stroke-dasharray="null" stroke-linejoin="null" stroke-linecap="null" x="393" y="183" id="svg_17" font-size="18" font-family="serif" text-anchor="middle" xml:space="preserve">北京</text>
  <text id="svg_7" fill="#000000" stroke="#000000" stroke-width="0" stroke-dasharray="null" stroke-linejoin="null" stroke-linecap="null" x="393" y="223.5" font-size="18" font-family="serif" text-anchor="middle" xml:space="preserve">沈阳</text>
  <text xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" id="svg_8" y="83.87326" x="106.51094" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">主页面</text>
  <text xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" id="svg_6" y="81" x="122" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000"/>
  <text id="svg_9" xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" y="83.86392" x="432.95303" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">城市选择页面</text>
  <rect id="svg_10" height="41.91336" width="186.91093" y="117.11493" x="70.26155" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="null" stroke="#000000" fill="#ffffff"/>
  <text xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" id="svg_11" y="144.16919" x="162.01782" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">北京</text>
  <text xml:space="preserve" text-anchor="middle" font-family="serif" font-size="12" id="svg_12" y="141.9036" x="226.99353" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000">城市选择</text>
  <rect height="41.91336" width="186.91093" y="115.04332" x="361.54454" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="null" stroke="#000000" fill="#ffffff" id="svg_1"/>
  <text xml:space="preserve" text-anchor="middle" font-family="serif" font-size="24" y="142.09758" x="456.30081" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000" id="svg_4">城市选择</text>
  <text xml:space="preserve" text-anchor="middle" font-family="serif" font-size="12" y="141.89449" x="384.27652" stroke-linecap="null" stroke-linejoin="null" stroke-dasharray="null" stroke-width="0" stroke="#000000" fill="#000000" id="svg_5">返回</text>
  <path fill="#cccccc" stroke-width="null" stroke-dasharray="null" stroke-linejoin="null" stroke-linecap="null" d="m530.92632,217l-5.42634,-5.43198l3.57367,0l5.42634,5.43198l-5.42634,5.43198l-3.57367,0l5.42634,-5.43198z" id="svg_16" stroke="#000000"/>
  <text id="svg_21" fill="#000000" stroke="#000000" stroke-width="0" stroke-dasharray="null" stroke-linejoin="null" stroke-linecap="null" x="134.54688" y="284.5" font-size="18" font-family="serif" text-anchor="middle" xml:space="preserve">风向：西南风</text>
 </g>
</svg>

### 天气信息从哪里来

应用是对数据的获取，加工和显示。有些数据可以通过所在终端的 api 去获取，比如拍照，录音。有些数据需要通过网络请求，从服务器获取。天气预报信息，终端无法获取，而是从天气预报提供服务器获取。关于网络请求参见下面章节。
天气预报应用需要两个请求接口，可以直接在浏览器地址栏输入试试：
1. ***天气信息***，获取指定城市的天气信息，地址为 http://t.weather.sojson.com/api/weather/city/{city_code}，其中 {city_code} 是可变部分，可替换为指定城市的编码，比如北京为 101010100
2. ***城市列表***，返回城市列表信息 ，地址为 http://cdn.sojson.com/_city.json?attname=

### 工程的配置

创建工程并进行定制化的配置

1. 通过脚手架 `npx @bnorth/cli create` 建立模板工程
1. 在工程配置文件 package.json 文件，增加一个属性，displayName: weather，配置应用的名称，将显示在浏览器标题上
1. 在 console 中输入 `npm install --save @bnorth/rich.css @bnorth/components @bnorth/plugin-network @bnorth/plugin-request`， 安装几个 bnorth 工具集，分别是 样式库，组件库，网络请求库和网路请求数据单元库
1. 在 style.js 文件中，配置样式样式，其中 normal.css 是标准化样式，可消除不同浏览器差异。genCss 函数动态生成应用使用的样式库

    ```js
    import '@bnorth/rich.css/css/normalize.css';
    import genCss from '@bnorth/rich.css';
    export default function(app) {
      genCss();
    }
    ```

1. 在 plugins.js 文件中，配置需要使用的插件，其中 notice 是弹出提示信息的插件，loading 是指示网络正在请求中的插件，network 和 request 配合用来从服务器获取信息

    ```js
    app.plugins.add(require('@bnorth/components/lib/notice').default);
    app.plugins.add(require('@bnorth/components/lib/loading').default);
    app.plugins.add(require('@bnorth/plugin-network').default);
    app.plugins.add(require('@bnorth/plugin-request').default, {request: app.network.fetch.bind(app.network)});

1. 在 route.js 文件中，配置路由信息。即配置了两个页面路由，分别是对应 '/' 的主页面和对应 'cities' 的城市选择页面

    ```js
    export default {
      '/': require('../pages/home').default,
      'cities': require('../pages/cities').default,
    }
    ```

### 页面的开发

页面一般由页面的 view 组件和 controller 控制器组成，view 组件描述了页面的组成和样式，controller 描述的页面的逻辑。直接上代码，先有个直观的感受，然后在下面章节逐步说明。

***主页面***
```jsx
import React from 'react';
import View from '@bnorth/components/lib/View'
import Panel from '@bnorth/components/lib/Panel'
import NavBar from '@bnorth/components/lib/NavBar'

let Component = props=>{
  let { 
    app, 
    stateWeather:{cityInfo:{city}={}, 
    data:{
      forecast:[{type,high,low,fx,fl}]=[], 
      ganmao, quality
    }={}} 
  } = props;
  
  return (
    <View>
      <NavBar>
        <NavBar.Title>{city}</NavBar.Title>
        <NavBar.Item app={()=>app.router.push('citys')}>切换城市</NavBar.Item>
      </NavBar>
      <Panel main className="padding-a-">
        <Panel className="flex-display-block flex-align-center">
          <Panel b-theme="primary" b-size="3x"  className="flex-display-block flex-direction-v flex-justify-center flex-align-center">
            {type}
          </Panel>
          <Panel className="flex-display-block flex-direction-v flex-justify-center">
            <span>最高温度：{high}</span>
            <span>最低温度：{low}</span>
            <span>风向：{fl}</span>
            <span>风力：{fx}</span>
          </Panel>
        </Panel>
        <Panel className="border-set-a- padding-a-">
          <Panel>空气质量：<Panel inline b-size="xxxl" b-theme="notice">{quality}</Panel></Panel>
          <Panel>{ganmao}</Panel>
        </Panel>
      <Panel>
    </View>
  )
}

Component.controller = (app,page)=>({
  stateWeather: ()=>{
    state: app.Request, fetchOnStart: true, fetchOnActive: true, 
    url: 'http://t.weather.sojson.com/api/weather/city/'+(app.cityCode||'101010100'),
  },
})

export default Component;
```

***城市选择页面***

```jsx
import React from 'react';
import View from '@bnorth/components/lib/View'
import Panel from '@bnorth/components/lib/Panel'
import List from '@bnorth/components/lib/List'

let Component = props=>{
  let { app, stateCitys: [] } = props;
  
  return (
    <View>
      <NavBar>
        <NavBar.Item app={()=>app.router.back()}>返回</NavBar.Item>
        <NavBar.Title>城市选择</NavBar.Title>
      </NavBar>
      <Panel main>
        <List>
          {stateCitys.filter(v=>v.pid===0).map(v=>(
            <List.Item 
              onClick={()=>{app.cityCode=v.city_code; app.router.back()}}
              key={v.city_code} title={v.city_name} />
          ))}
        </List>
      <Panel>
    </View>
  )
}

Component.controller = (app,page)=>({
  stateCitys: ()=>{
    state: app.Request, fetchOnStart: true, initialization: [],
    url: 'http://cdn.sojson.com/_city.json?attname='),
  },
})

export default Component;
```

## 代码剖析

### 为什么运行起来了

1. pageage.json 是 nodejs 工程的配置文件，脚手架默认生成的文件配置了应用的依赖库：
    ```json
    "dependencies": {
      "@bnorth/build": "^3.1.6",
      "@bnorth/core": "^3.1.6"
    }
    ```
    同时也配置了运行子命令：
    ```json
    "start": "bnorth-build server",
    "build": "bnorth-build build",
    ```

    因此执行 `npm start` 相当于运行依赖库 `@bnorth/build` 的 `server` 命令。该命令编译了工程的代码，并启动的调试服务器，之后启动系统的浏览器，打开了调试服务器的代码。并守护着，一旦用户修改了代码，将自动重新编译，并刷新页面。

1. 启动后，将读取配置的路由表，读取指定文件的代码

1. 加载首页面后，首先加载 controller ，首页的 controller 中配置了一个 state 名为 stateWeather 。并设置了 `state: app.Request`， 说明不是一个普通的数据 state，而是 request 插件配置的网络请求的 state。其他参数 `fetchOnStart: true, fetchOnActive: true, `，告诉 request，进入和回到顶层时，触发请求。因此进入时，根据参数 url 发起网络请求，请求返回时，将更新 state。

1. 首页的组件是一个 react 无状态组件，看上去是一个普通的箭头函数(es6 定义的函数类型，具体参见后面章节)，只有一个参数，页面和组件属性以 object 形式传递进来。使用解构(es6 语法，具体参见后面章节)的方式获取数据。函数的返回值，是 html 与 js 混编的 jsx 语法的布局描述。

1. jsx 语法并不特殊，也不是真的混编，仅仅是语法糖，bnorth-build 会通过 babel 将其转换为标准 js。见下例：

    ```jsx
    return (<h1 className="greeting">Hello, world!</h1>);
    ```
    转换后：
    ```jsx
    return  React.createElement('h1', {className: 'greeting'}, 'Hello, world!');
    ```

    ```jsx
    let text = 'click me';
    return <button onClick={()=>alert('click')}>{text}</button>
    ```

    jsx 语法中，通过 `{}` 将 js 代码嵌入进去

1. 页面里除了标准 html 元素 <span> 外，还使用了很多 bnorth components 库提供的组件。比如：
    - View: 页面的跟元素
    - Panel: 小面板组件，main 属性，将使面板充满 View，并支持滚动
    - NavBar: 标题栏组件

1. 页面事件用来捕获用户的操作，使用标准 Html 事件的 jsx 语法，其中调用了 @bnorth/core 的一些模块。比如：
    - `app.router.push('citys')`，调用 router 模块的 push 函数，跳转到 citys 页面

1. 在城市选择页面，选择城市后，将城市代码保存在 `app.cityCode` 中，调用 `app.router.back()`， 返回后首页在层回到顶层页面，触发更新，显示所选城市的天气信息

## 几个简单的基础知识

### jsx

### es6 箭头函数

### es6 解构

### 服务器和与服务器通讯

当终端要处理的数据由服务器提供时，需要用到服务器通讯。移动互联网应用主要都是基于服务器通讯的应用。终端一般采集坐标，图像，声音和视频。而比如商品信息数据，天气数据，新闻资讯数据等都是由服务器以接口方式提供。

服务器通讯就是基于经典的 tcp/ip 协议，但是一般使用更高级的应用层协议 http。和一些使用或者平行于 http 的协议，比如 soap 和 websocket。现在服务器一般是轻量级的 http + json 通讯。作为大前端开发，仅需要了解基本的 http 通讯，就可以根据服务器规定的格式进行信息交互。(如果做实时通讯，比如即时通讯，邮件等还需要了解 websocket)。

与服务器通讯分为，通讯和数据承载两个方面。

在通讯方面，广泛使用的 Http 是单向通讯，即终端向服务器发出请求，服务器接收处理并返回结果给终端显示。发送的请求主要由以下几部分组成：

1. 请求方法：
1. 地址：
1. 请求头：
1. 请求体：

服务器处理后返回的结果，主要包括以下部分：

1. 状态码：
1. 响应头：
1. 响应体：

在数据承载方面，一般使用 存数据，xml 与 json。json 具体简单，体积小，与 js 结合紧密，因此一般使用 json 作为数据承载格式。

具有一个字符串属性 a，一个数值属性 b，一个布尔属性的对象 c， 一个嵌套对象的属性d， 一个嵌套数组的属性 e
```js
{
  a: 'hello',
  b: 1,
  c: true,
  d: {a: 1},
  e: [1, 2]
}
```

json 数组
```js
[1, 'a', true, {a:1}]
```

如果是文件数据，需要通过 formdata 来承载。

```js
let formdata = new FormData(document.getElementById("file-input"));
```

