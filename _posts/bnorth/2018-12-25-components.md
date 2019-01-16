---
layout: post
title: bnorth react 组件库
category: cbnorth
weight: 5
tags: [react, bnorth, 大前端, 组件库]
---

bnorth react 组件库是普通的 react 组件库，可以用在任何 react 工程中。
但是在技术上有自己的特点，bnorth 组件库是 bnorth 工具集的组成部分。
在样式上使用 bnorth rich.css 样式库。
在设计上使用层次化的设计思想，基础组件提供共同的基本属性，面板组件提供统一的布局方法，方便实现主题替换。

## 使用方法

首先引入 rich.css 样式库，具体参见 [rich.css]() 。之后与使用普通 react 组件相同。

```jsx
import ReactDom from 'react-dom';
import rich.css
import Button from '@bnorth/components/lib/Button';
ReactDOM.render(<Button onClick={()=>alert('hello')}>click</Button>, document.querySelector('#root'));
```

bnorth react 组件库提供了丰富的组件和插件，可以轻松开发复杂的 h5 移动端应用和 pc 端应用。

### 布局组件

布局组件提供页面组织方式和容器，比如：

1. Panel ：通用的小面板组件，实现基本样式风格属性，样式方便的定制，是布局中常用组件
1. Panel.Container ：扩展了小面板组件的功能，可以包含子组件，并可以统一和分别设置子组件的样式和属性
1. Tabs ： 标签页组件是 web 应用常用的标签分页布局组件

### 小组件

小组件提供了具体的功能，比如：

1. Button ： 按钮组件
1. Carousel ：轮播组件
1. Icon ：字体图标组件，该组件使用 svg 图标库，同时支持字符图标，图片图标和形状图标

### 弹出组件

弹出组件提供了悬浮于页面之上的组件，比如：

1. Fab ： 悬浮按钮组件
1. Modal ： 对话框组件
1. Popover ： 弹出组件，可以设置弹出组件的触发方式和相对于目标组件的位置

### 动画组件

实现动画效果的组件，比如：

1. AnimationCollaplse ：折叠动画
1. AnimationFade ：淡入淡出动画
1. AnimationSlider ：滑动组件

### 功能组件

react 一切皆为组件的思想逻辑下，也实现了一些十分有用的功能性组件，比如：

1. ScrollSpy ：滚动监控组件，可监控带有滚动的条的父组件的滚动事件和当前组件在父组件中的位置关系改变的事件
1. Panel.Touchable ：扩展了小面板组件，实现了手势操作的功能

### 插件

bnorth 组件库，包装了一些组件，实现了 bnorth 接口的插件，为 bnorth 提供了特定 ui 功能的扩展，比如：

1. modal ：扩展 bnorth app 功能，提供创建对话框和管理对话框的函数
1. notice ：扩展 bnorth app 功能，提供了通知消息显示的功能
1. loading ：扩展 bnorth app 功能，提供了加载进度显示的功能

## 通用属性

组件库采用层次式设计，各个组件的基础结构相同，而实现了一些通用的基础组件属性。

### 样式库属性

任何组件都支持通过若干样式库属性增加样式库的方式，样式库属性是以 bc- 开头的属性，例如：

```jsx
<Panel 
  bc-text-color="primary" 
  bc-text-weight-bold={true} 
  className="text-size-lg" /> // class: 'text-colori-primary bc-text-weight-bold text-size-lg'
```

样式库属性的好处在于，react 组件的 className 属性只有一个，需要对这个属性进行拼接，而使用样式库属性，组件将自动完成拼接，而且支持变量的方式设置样式。对于同一类的样式库，后面的样式库将替换前面的样式库。

### 样式表属性

任何组件都支持通过若干样式表属性增加样式表条目的方式，样式表属性是以 bs- 开头的属性，例如：

```jsx
<Panel bs-width="10px" style={{height: 10}}/> // style: 'width: 10px, height: 10px'
```

多个样式表属性也是追加方式。

### 样式表函数

对于复杂的样式表，rich.css 提供了一些函数方便使用，比如背景函数，阴影函数等。
在组件上使用样式表函数的方式是，首先添加到样式表函数集合中，然后以 bf- 开头的属性去调用。例如：

```jsx
import { backgroundImage } from '@bnorth/rich.css/lib/styles/background';
import { addFunctions } from '@bnorth/components/lib/utils/props';
addFunctions({ backgroundImage });
<Panel bf-background={'bg.jpg'} /> // style: {backgroundImage: url(bg.jpg)}
```

多个样式表函数也是追加方式。

### 样式风格属性

大部分组件显示了样式风格通用属性，或者大部分组件的映射组件是基于支持样式风格属性的组件，因此可以通过样式风格属性定制组件样式风格。
样式风格属性只是一种约定，不同组件的实现略有不同。

样式风格属性包括：

1. b-theme ：设置组件的主题色
1. b-size ：设置文字大小样式风格
1. b-style ：设置组件的背景与边框样式风格，比如 solid 是带有实心背景的白色字体颜色的组件，hollow 是带有主题色文字和边框的白色背景组件，underline 是带有下划线的组件等等

```jsx
<Button b-style="hollow" b-theme="alert" /> // 警告颜色的镂空按钮
```

### 映射组件属性

大部分组件都实现了映射组件属性，映射组件是 bnorth 组件库的特点。
组件库的组件实现了组件样式的定义和具体属性与事件的定义，有的还规定了组件的子组件。但是组件本身却提供了默认组件，同时支持设定。
映射组件的属性可以设置为 dom 元素或者是其他组件。
比如，按钮组件 Button 的默认映射组件是 dom 元素 button，可以通过设置映射组件属性，修改为 dom 元素 a。
这样将保留按钮组件的按钮样式，但是实际描画的是 a 元素，可以设置 a 元素的 href 等属性。

```jsx
<Button component="a" b-style="hollow" b-theme="alert" /> 
```

## 主题与样式

bnorth 组件可以在不改变使用组件处的代码，而统一的定制组件的通用样式。

### rich.css

bnorth 组件本身没有开发 css 样式，而是使用 rich.css 提供的样式库，比如按钮的默认背景颜色为 bg-color-component ，可以在引入 rich.css 时修改 component 颜色，实现 Button 组件的统一样式修改。再比如项目中很多组件设置了 b-theme="primary" 属性，表示使用主要的主题色，修改了 rich.css 的 primary 颜色，可实现更换主题。除了颜色，文字大小，边框样式等都可以通过 rich.css 修改，实现全意义下的换肤功能。

### 组件通用样式

bnoth 的组件都可以通过属性定制功能，样式。因此如果给组件设置了默认的属性，就可以实现默认的样式的修改。设置组件的默认样式的方法是，修改组件的静态属性 props ，各个组件还可以通过设置自己的相关属性，覆盖掉组件的默认属性。

```jsx
import Button from '@bnorth/components/lib/Button'
Button.props = { 'b-theme': 'primary' } // 所有组件默认都使用主要的主题色
ReactDOM.render(<Button onClick={()=>alert('hello')}>click</Button>, document.querySelector('#root'));
```

### 内部组件样式

对于一些复杂的组件，是由多个组件组合嵌套而成，比如 tabs 组件，由 tabs 组件，tabs 的 导航按钮组件 tabs.nav 和 tabs 的每个页的容器组件 tabs.container 组成。
此时对 tabs 的通用属性设置都是修改的 tabs 组件，如果想设置 tabs.nav 组件的样式，有多种方法。

1. 按照 bnorth 组件的一般约定，给组件设置子组件的属性。比如设置导航按钮的组件下边框与容器组件分离开

    ```jsx
    <Tabs navProps={{className:'border-set-bottom-'}}>
      <Tabs.Item title="A">page A</Tabs.Item>
      <Tabs.Item title="B">page B</Tabs.Item>
    <Tabs>
    ```

1. 设置子组件的默认属性，tabs.nav.props = {className:'border-set-bottom-'}

1. 直接替换掉现有的子组件，实现先的子组件 tabs.nav = NewTabsNav;


