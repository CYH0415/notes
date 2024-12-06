- [9] HTML 是构成 Web 世界的一砖一瓦，它定义了网页内容的含义和结构。其基本结构是如下的标签以及内容：

```html
<opening_tag attribute_name="value">
    content
</closing_tag>
```
---
## `<html>`
包含所有网页内容的标签
-  `lang="zh-CN"`
## `<head>`
包含了网页的头部数据，例如标题，引入 CSS 等
### 标题
```html
<title>Title of page<title/>
```
### 元数据
#### 文字编码
```html
<meta charset="utf-8" />
```
#### 作者和描述
- `author`：作者
- `description`：描述，用于搜索引擎等
```html
<meta name="author" content="Van" />
<meta name="description" content="This is a site." />
```
### 图标
```html
<link rel="icon" href="favicon.ico" type="image/x-icon" />
```
### CSS和JavaScript
```html
<link rel="stylesheet" href="a.css" />
<script src="a.js" defer></script>
```
## `<body>`
### 文本
#### 语义元素
- `<p>`：段落
- `<h1>`~`<h6>`：标题
- `<ul>`：无序列表
- `<ol>`：有序列表
    - 列表元素用`<li>`包裹
    - 可嵌套列表
- `<strong>`：粗体强调
- `<em>`：斜体强调
#### 表象元素
无语义
- `<b>`：粗体
- `<i>`：斜体
- `<u>`：下划线
#### 链接
```html
<a href="test.html#article" title="jump">
    <p>jump<p/>
<a/>
<p id="article">
    here.
<p/>
```
可以包含块级内容，如图片等
- `href`：指向的链接
    - `mailto:xxx@xxx.com`：邮件
    - `xxx#id`：文档片段链接（在相应的板块加入`id="id_name"`）
- `title`：鼠标悬浮时显示的信息
- `download`：（下载文件）默认保存文件名
#### 其他文本
- `<dl>`：描述列表
    - `<dt>`：术语
    - `<dd>`：描述
    - 一个术语可以有多个描述
- `<blockquote>`：块级引用。浏览器会缩进
- `<q>`：行内引用
    - `cite=：用URL指向引用的资源
- `<cite>`：引用元素，应使用`<a>`链接到引用源。默认为*斜体*
- `<abbr>`：缩略
    - `title`：完整解释（鼠标悬浮显示）
- `<sup>`, `<sub>`：上标与下标
- 代码：
    - `<code>`：用于标记计算机通用代码。
    - `<pre>`：用于保留空白字符（通常用于代码块）——如果文本中使用了缩进或多余的空白，浏览器将忽略它，你将不会在呈现的页面上看到它。但是，如果你将文本包含在 `<pre></pre>` 标签中，那么空白将会以与你在文本编辑器中看到的相同的方式渲染出来。
    - `<var>`：用于标记具体变量名。
    - `<kbd>`：用于标记输入电脑的键盘（或其他类型）输入。
    - `<samp>`：用于标记计算机程序的输出。
- `time`：日期
    - `datetime`
### 文档架构
#### 结构
- `<header>`：页眉
- `<nav>`：导航
- `<form>`：搜索栏
- `<main>`：主内容
    - `<article>`
    - `<section>`
    - `<aside>`：侧边栏
- `<footer>`：页脚
#### 无语义元素
- `<span>`：内联
- `<div>`：块级
#### 分隔
- `<br>`：换行
- `<hr>`：水平线
### 多媒体
#### 图片
```html
<img src="test.png" alt="A picture" />
```
- `src`：图片地址
- `alt`：替换文字
- `width`, `height`：图片未加载时也可以生效
#### 视频
```html
<video src="test.mp4" controls>
    <p>Can't play.</p> //无法播放的后备内容
</video>
```
- `controls`：使用浏览器自带的播放器
- `width`, `height`：不拉伸
- `autoplay`
- `loop`
- `muted`
- `poster="xx.jpg"`
- `preload=`：缓冲
    - `none`
    - `auto`：页面加载后才缓冲
    - `metadata`：仅缓冲元数据
- 提供多种格式：
 ```html
<video controls>
  <source src="van.mp4" type="video/mp4" />
  <source src="van.webm" type="video/webm" />
</video>
```
#### iframe 嵌入
```html
<iframe 
    src="//player.bilibili.com/player.html?isOutside=true&aid=19390801&bvid=BV1bW411n7fY&cid=31621681&p=1"
    scrolling="no"
    border="0"
    frameborder="no"
    framespacing="0"
    allowfullscreen>
</iframe>
```
一些常用属性：
- `src`
- `sandbox`：尚不清楚，总之安全性
- `allowfullscreen`
- `border: none`
#### embed 与 object 嵌入
| 属性                         | `<embed>`             | `<object>`                           |
| -------------------------- | --------------------- | ------------------------------------ |
| 嵌入内容的 URL                  | `src`                 | `data`                               |
| 嵌入内容的_准确_媒体类型              | `type`                | `type`                               |
| 由插件控制的盒子高度和宽度（以 CSS 像素为单位） | `height`, `width`     | `height`, `width`                    |
| 名称和值，作为参数提供给插件             | 具有这些名称和值的 ad hoc 属性   | 单标签 `<param>` 元素，包含在 `<object>` 元素里面 |
| 用作后备资源的独立的 HTML 内容，以防资源不可用 | 不受支持（`<noembed>` 已过时） | 包含在 `<object>` 中，在 `<param>` 元素之后    |
使 用 例：
```html
<object data="mypdf.pdf" type="application/pdf" width="800" height="1200">
  <p>
    You don't have a PDF plugin, but you can
    <a href="mypdf.pdf">download the PDF file. </a>
  </p>
</object>
```
### 表格
- `<tr>`：存放一行的元素
- `<td>`：表格的一个 cell，`<th>`：同性质，只是作为表头，应用不一样的样式
    - `colspan`, `rowspan`：拓展 cell 大小
- `<colgroup>`：统一制定列样式，写在`<table>`的开头（其他的地方也可，毕竟是HTML
```html
<colgroup>
  <col /> // 无样式的列需要这样跳过
  <col style="background-color: yellow" span="2" />
</colgroup>
```
- `<thead>`, `<tbody>`, `<tfoot>`：结构化表格，方便应用 CSS