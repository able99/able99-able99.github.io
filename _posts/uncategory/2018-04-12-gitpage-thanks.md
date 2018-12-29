---
layout: post
title: 免费Blog 与 gitpages 和 jekyll 
desc: 小站搭建完毕，非要感谢下 gitpages 和 jekyll，算是给想搭建 blog 的伙伴小福利
category: cuncategory
weight: 0
tags: [gitpages, jekyll]
---
小站搭建完毕，非要感谢下gitpage 和 jekyll，算是给想搭建blog 的伙伴小福利

{% raw %} 
# 博客


# GitHub 与 GitHub Pages


# 深入 jekyll

jekyll是一个简单的免费的Blog生成工具，类似WordPress。但是和WordPress又有很大的不同，原因是jekyll只是一个生成静态网页的工具，不需要数据库支持。但是可以配合第三方服务,例如Disqus。

jekyll 创造了一些规则与概念（[参考文档](https://jekyllrb.com/docs/home/)），并采用liquid ([参考文档](https://shopify.github.io/liquid/)) 语言开发

## 配置信息

根目录下的_config.yml 文件，可配置一些站点信息， 比如：

```yaml
title: blog
content:
  title: content title
  color: #cbcbcb
```

文件采用yaml 语法，可阅读[阮一峰老师的精彩介绍](http://www.ruanyifeng.com/blog/2016/07/yaml.html) 快速入门，同时可以混合使用JSON 格式

```yaml
title: blog
content: {
  title: 'content title'
  color: '#cbcbcb'
}
```

配置信息也可以分散到多个文件中，比如在根目录下建立_data 文件夹，并建立info.yml 文件：

```yaml
title: info title
```

并用如下方式访问

```html
<div>{{site.data.info.title}}</div>
```

## 站点文件

### 特殊文件

下划线开头的文件夹及其中的文件，包括:
- _config.yml: 配置信息文件
- _data: 配置信息文件夹
- _layouts: 模板文件夹
- _include: 其中文件可被包含到其他文件中
- _posts: 文章文件夹，其中文件可由site.posts 变量遍历，名字必须是 `YEAR-MONTH-DAY-title.md|html`
- _site: 编译后的缓存文件夹
- _drafts: 草稿文件夹，需要启动参数 --drafts，GitHub Pages 未开启

### 页面文件

页面文件，在文件包含yaml 信息的文件，此类文件将被jekyll 预处理，比如：

```html
---
layout: post
title: xxx
---
<div>{{page.title}}</div>
```

- 页面文件，可以包含jekyll 语句
- 页面文件，可以是html 文件或者是markdown(md后缀) 文件
- 页面文件中layout 参数指定，该页面的模板，指定后，页面会以指定模板渲染，同时将页面的内容，以 content 变量传递

页面文件的参数：
- layout：指定模板文件
- published：是否对外可见
- [name]：自定义变量，可通过page[name]访问

### 静态文件

其他类型文件为静态文件，可被直接访问

## 模板

- 模板文件，可以包含jekyll 语句
- 模板文件，可以是html 文件或者是markdown(可以是md 后缀)文件
- 模板文件可以使用content 全局变量，改变变量是预处理后的页面文件内容，page.content 变量是原始文件内容

## include

_include 文件夹中的文件，可被其他模板文件和页面文件包含，形如：

_includes/head.html
```html
<title>jekyll</title>
```

include 支持变量
```html
{% include {{ page.my_variable }} %}
```

index.html
```html
<html>
{% include head.html %}
<body></body>
</html>
```


## 预定义全局变量

### site - 站点变量
- site.time: 运行 jekyll 的时间
- site.pages: 所有页面
- site.posts: 所有文章
- site.related_posts: 类似的10篇文章,默认最新的10篇文章,指定lsi为相似的文章
- site.static_files: 没有被 jekyll 处理的文章，包括如下属性
  - file.path：相对路径
  - file.modified_time：修改事件
  - file.name：文件名
  - file.basename：文件名去掉扩展名
  - file.extname：文件扩展名

- site.html_pages: 所有的 html 页面
- site.collections: 新功能,没使用过
- site.data: _data 目录下的数据对象
- site.documents: 所有 collections 里面的文档
- site.categories: 文章中categorie 参数定义过的categorie 数组
- site.tags: 文章tag 参数定义过的tag 数组
- site.[name]: _config.yml 中自定义变量

### page - 页面变量
- page.content: 页面的内容
- page.title: 标题
- page.excerpt: 摘要，即文章的第一个段落
  - 段落可以指定，通过设置变量excerpt_separator，该变量可以设置在页面，或者设置在_config.yaml 全局有效
    ```html
    ---
    excerpt_separator: <!--more-->
    ---

    Excerpt
    <!--more-->
    Out-of-excerpt
    ```

  - 一般需要去除标示语言，`{ { post.excerpt | strip_html } }`
  
- page.url: 链接
- page.date: 时间
- page.id: 唯一标示
- page.categories: 分类
- page.tags: 标签
- page.path: 源代码位置
- page.next: 下一篇文章
- page.previous: 上一篇文章

### paginator - 分页数据变量
- paginator.per_page: 每一页的数量
- paginator.posts: 这一页的数量
- paginator.total_posts: 所有文章的数量
- paginator.total_pages: 总的页数
- paginator.page: 当前页数
- paginator.previous_page: 上一页的页数
- paginator.previous_page_path: 上一页的路径
- paginator.next_page: 下一页的页数
- paginator.next_page_path: 下一页的路径

### layout

### content


## 语句
- 模板转移：

  { 字符需要转义 `\{`

  也可以使用快转义方法
  ```
  \{% raw %\}
  console.log('{}');
  \{% endraw %\}
  ```
  
- 输出变量：`{{ page.title }}`
- 赋值：`{ % assign index = 1 % }`
- 输出格式化后的文本
  ```html
  {% highlight ruby %}
    console.log('hello world');
  {% endhighlight %}
  ```

## 流程控制
- if判断：`{% if page.title=="xxx" %} xxx {% endif %}`
- for循环：`{% for post in site.posts %} {{post.title}} {% endfor %}`
- 限制数量：`{% for post in site.posts limit:20 %}{% endfor %}`
- 判断第一个文章：`{% if post == site.posts.first %}{% endif %}`

## 常用函数

### 链接
- 相对地址： `{{ "/assets/style.css" | relative_url }}` /my-baseurl/assets/style.css
- 绝对地址： `{{ "/assets/style.css" | absolute_url }}` http://example.com/my-baseurl/assets/style.css
- cgi 编码：`{{ "foo, bar; baz?" | cgi_escape }}` foo%2C+bar%3B+baz%3F
- uri 编码：`{{ "http://foo.com/?q=foo, \bar?" | uri_escape }}` http://foo.com/?q=foo,%20%5Cbar?

### 字符串
- 删除指定文本：`{ { post.url | remove: 'http' } }`
- 去掉html 标签：`{ { post.excerpt | strip_html } }`
- 单词的个数：`{ { page.content | number_of_words } }`

### 时间格式化
- `{ { site.time | date_to_xmlschema } }` => 2008-11-07T13:07:54-08:00
- `{ { site.time | date_to_rfc822 } }` => Mon, 07 Nov 2008 13:07:54 -0800
- `{ { site.time | date_to_string } }` => 07 Nov 2008
- `{ { site.time | date_to_long_string } }` => 07 November 2008

### 数组与对象
- 长度：`{ { array | size } }`
- 排序：`{ { site.pages | sort: 'title', 'last' } }`
- 搜索：`{ { site.members | where:"graduation_year","2014" } } `
- 对象序列化为字符串：`{ { page.tags | array_to_sentence_string } }`
- 字符串转JSON：`{ { site.data.projects | jsonify } }`

## scss

Sass为web前端开发而生，可以使用最高效的方式，以少量的代码创建复杂的设计。它改进并增强了CSS的能力，增加了变量，局部和函数这些特性。[快速上手](https://www.sass.hk/guide/)

Jekyll 3 已经自带Sass编译器，使用方法：

- 新建sass 文件夹，但是应该以下划线开头，否则会成为静态文件，比如 _sass，并在其中新建编辑sass 文件，比如 _sass/sytle.scss
- 修改_config.yml开启sass
  ```yaml
  sass:
    sass_dir: _sass
    style: compressed
  ```

- 建立或编辑站点中的静态css 文件，比如： /css/styles.css
  ```html
  ---
  ---

  @import "style";
  ```

  **注意：横线不要去掉，否则不会被jekyll 预处理**

- 正常使用该css 文件，比如

  ```html
  <link rel="stylesheet" href="/css/styles.css">
  ```
{% endraw %}
