---
title: HTML basic
categories:
  - web
  
abbrlink: 2701
date: 2024.7.11
tags: 
   - html 

---

# HTML

## 基础结构

标记：标记是什么东西

```
<标签名>aaaa</标签名>
```

**一对标签**

**aaaa标签的内容**

**标签有开始和结束,每一个标签都有自己的作用**

```html
<h1>题目</h1>#一级标题
<h2>作者</h2>#二级标签
<p>aaaaaa</p>#内容
<p>aaaaaa</p>
<p>aaaaa</p>

```

```html
<html>
	<head>#头部，网页中的源数据，给浏览器看，搜索引擎看的，网页中不会显示
		<title>诗</title>
	</head>
	<body>#网页的主题，可见的内容
		<h1>题目</h1>#一级标题
		<h2>作者</h2>#二级标签
		<p>aaaaaa</p>#内容
		<p>aaaaaa</p>
		<p>aaaaa</p>
	</body>
</html>
```

## 自结束标签和注释

```html
<img>
<input>



<img />
<input />


<!--
这里是注释，注释是不能嵌套的。因为会解析到前一个-->导致缺少一个解析
-->

```

上面是标准标签，两种写法都是对的。

## 标签中的属性

```html
<html>
    <head>
        <title>My First HTML Page</title>
    </head>
    <body>
        <h1>Hello World!</h1>
        <p>This is my <font color='red' size='10'>first</font> HTML page.</p>
    </head>
</html>
<!--
属性，在开始标签中设置属性，是一个名值对（x=y）这样一个类似的结构。属性用来设置标签中的内容如何显示
属性名不能瞎写，属性名和标签名或者其他属性应该使用空格隔开，有的属性有值，有的没有值。属性值应该被引号引起来。
-->
```

**print:**

<h1>Hello World!</h1>
<p>This is my <font color='red' size='5'>first</font> HTML page.</p>

## 文档声明

```html
<html>
    <head>
        <title>My First HTML Page</title>
    </head>
    <body>
        <h1>Hello World!</h1>
        <p>This is my <font color='red' size='10'>first</font> HTML page.</p>
    </head>
</html>
```

文档声明：告诉浏览器当前网页的版本

html5的文档说明:

```html
<!doctype html>
```

在网页的最开头，所以代码变成

```html
<!doctype html>
<html>
    <head>
        <title>My First HTML Page</title>
    </head>
    <body>
        <h1>Hello World!</h1>
        <p>This is my <font color="'red">first</font> HTML page.</p>
    </head>
</html>

```

## 字符编码

读取的时候将二进制转变为字符

编码和解码

编码解码的规则叫做字符集（charset)

如果编码和解码所采用的字符集不同会出现乱码问题

常见的字符集

- ASCII **128**
- ISO88591 **256欧洲**
- GBK    **国标**
- UTF-8   **万国码**
- GB2312    **国标**

默认utf-8

```html
<!doctype html>
<html lang='en'><!--语言为英文-->
    <head>
    <meta charset = 'utf-8'>
        <title>My First HTML Page</title>
    </head>
    <body>
        <h1>Hello World!</h1>
        <p>This is my <font color="'red">first</font> HTML page.</p>
    </head>
</html>
```

**设置字符集**

## 实体

多个空格会自动解析第一个空格

```
<b>加粗
```

在html里，不能直接书写特殊的符号

书写特殊符号，则需要使用实体

```
&nbsp	空格实体
&gt	大于
&lt	小于
&copy	版权
```

| 显示结果 | 描述              | 实体名称          | 实体编号 |
| :------- | :---------------- | :---------------- | :------- |
|          | 空格              | &nbsp;            | &#160;   |
| <        | 小于号            | &lt;              | &#60;    |
| >        | 大于号            | &gt;              | &#62;    |
| &        | 和号              | &amp;             | &#38;    |
| "        | 引号              | &quot;            | &#34;    |
| '        | 撇号              | &apos; (IE不支持) | &#39;    |
| ￠       | 分（cent）        | &cent;            | &#162;   |
| £        | 镑（pound）       | &pound;           | &#163;   |
| ¥        | 元（yen）         | &yen;             | &#165;   |
| €        | 欧元（euro）      | &euro;            | &#8364;  |
| §        | 小节              | &sect;            | &#167;   |
| ©        | 版权（copyright） | &copy;            | &#169;   |
| ®        | 注册商标          | &reg;             | &#174;   |
| ™        | 商标              | &trade;           | &#8482;  |
| ×        | 乘号              | &times;           | &#215;   |
| ÷        | 除号              | &divide;          | &#247;   |

## meta标签

设置网页的源数据

```
<meta>
name	指定的数据的名称
content	指定的数据的值
charset	字符集
```

```
<meta name='keyword' content='前端'>
```

表示网页的关键字（搜索应用的）

```
<meta name='descripton' content='描述'>
```

网站的描述,会显示搜索引擎的页面

```
<meta http-equiv='refresh' content='3;url=https://mengnankk.asia'>
```

将页面3秒后重定向到另一个网站

## 语义化的标签

html只是一个结构，表现通过css控制

应该关注的是标签的语义

### 标题标签

```
<h1></h1>
<h2></h2>
<h3></h3>
<h4></h4>
<h5></h5>
<h6></h6>
```

一共六级标题

h1-h6重要性依次递减，h1的重要性仅次于title，一般情况下只有一个

h1-h3一般常用剩下的用的很少

标题标签会独占一行，独占一行的元素成为块元素



### 段落标签

```
<p>
啊啊啊啊
</p>

```

也是独占一行，为块元素

### hgroup标签

```
<hgroup>
	<h1></h1>
	<h2></2>
</hgroup>
```



用来为标题分组，可以将一组相关的标题同时放入hgroup内

### em标签

```
<p>aaaaa<em>a</em>aa</p>
```

em标签表示语音语调的加重，**不会独占一行，叫做行内元素**

<p>aaaaa<em>卧槽</em>aa</p>

### strong标签

表示强调，和em用法一样

<p>aaaaa<strong>wwww</strong>aa</p>

### blockquote标签

blockquote表示引用，也是块元素

<blockquote> aaaaaaa </blockquote>

### q引用

q表示一个短引用

### br标签

br标签表示换行

<p>aaaaa<em>a</em>a
    <br>a</p>

### 块元素和行内元素

块元素用来进行网页的布局，是一块一块的，宏观的布局

行内元素是用来包裹文字，文字需要设置什么样的效果

- 一般会在块元素中放行内元素，不会在行内元素中放块元素
- p元素中不能放任何的块元素
- 块元素中一般什么都能放

浏览器会在一些特殊情况下进行纠正，会在代码在内存的结构

### 布局标签/结构化标签

```
<header></header>网页的头部
<main></main>头部往下的主体部分（一个页面中只有一个
<footer></footer>网页的页脚
<nav></nav>网页中的导航
<aside></aside>表示和主题相关的其他类，侧边狼啥的
<article></article>表示文章
<section></section>表示一个独立的区块，表示其他
<div></div>表示一个区块，可以代替上述的所有
<span></span>m表示行内元素，一般用于选中文字
```

<div></div>表示一个区块，可以代替上述的所有

## 列表

list列表

1.有序列表

ol

```
<!doctype html>
<html>
    <head>
        <title>My First HTML Page</title>
    </head>
    <body>
        <h1>Hello World!</h1>
        <ol>
            <li>
                结构
            </li>
            <li>
                aaa
            </li>
        </ol>
        <p>This is my <font color="'red">first</font> HTML page.</p>
    </head>
</html>

```

2.无序列表

ul

```
<!doctype html>
<html>
    <head>
        <title>My First HTML Page</title>
    </head>
    <body>
        <h1>Hello World!</h1>
        <ul>
            <li>
                结构
            </li>
            <li>
                aaa
            </li>
        </ul>
        <p>This is my <font color="'red">first</font> HTML page.</p>
    </head>
</html>

```

3.定义列表

```
    </dl>
        <dl>
            <dt>
                aa
            </dt>
            <dd>
                aaa
            </dd>
        </dl>
```

dt表示定义的内容

dd表示定义的解释

**ul是用的比较多的**

**列表之间是可以互相嵌套的**

## 超链接

可以从一个界面跳转到另一个界面，是一个行内元素，可以嵌套任何元素，除了他自身。

使用<a>，

```
<!doctype html>
<html>
    <head>
        <title>My First HTML Page</title>
    </head>
    <body>
        <h1>Hello World!</h1>
        <p>This is my <font color="'red">first</font> HTML page.</p>
        <a href="https://mengnankk.asia">mengnankk</a>
        <a href="https://mengnankk.asia">mengnankkk</a>
    </head>
</html>

```

**herf属性存放跳转到链接（内部外部都可以）,这是<a>标签最重要的属性**，**也能跳转到页面的其他位置**

相对链接

**./默认不写../是上个文件路径**

不是跳转是新建

target属性

_self是默认值，在当前页面中打开链接

_blank是在新的页面中打开链接

回到顶部的效果：

```
<a href="#" target="_blank" >wwwww</a>
```

回到页面的任意位置效果：

使用id属性，是唯一不重复

```
<!doctype html>
<html>
    <head>
        <title>My First HTML Page</title>
    </head>
    <body>
        <h1>Hello World!</h1>
        <p>This is my <font color="'red">first</font> HTML page.</p>
        <a href="#botter">wwwww</a>
        
        <a href="../"></a>
        <a href="../"></a>
        <a href="../"></a>
        <a href="../"></a>
        <a href="../"></a>
        <a href="../"></a>
        <a href="../"></a>
        <a id="botter" href="#">???</a>
    </head>
</html>

```

在开发中#作为超链接的占字符

什么用也没有的超链接

```
<a href="javascript:;"></a>
```

## 图片标签

引入外部的图片，属于替换元素，介于块元素和行内元素中间，具有两种元素的特点

使用<img>标签

```
<img src="#">
```

<img src="https://imgbed.mengnankk.asia/202407131915645.jpg">

alt属性是对图片的描述，描述默认情况下不会显示，无法加载的时候会出现。搜索引擎会根据这个搜索的

width宽度属性

height高度属性

值改变一个属性的时候，宽度和高度会等比例的改变。**在pc端中******一般不建议在浏览器中修改，在移动端中就需要修改****

## 图片的格式

jpg:支持的颜色丰富，不支持透明，不支持动图 	一般用来显示照片

gif：支持的颜色比较少，支持简单的透明，支持动图	颜色单一的图片/动图

png：支持的颜色丰富，支持复杂透明，不支持动图	颜色丰富，颜色复杂的图片/专为网页而生

webp:专门用来表示网页，有其他图片格式的所有有点，文件还特别小

**效果一样用占的小的。**

也可以用base64来快速加载

## 内联框架

用于向当前页面中引入别的网站



```
<iframe src="aaaaaa" width="500" height="500" frameborder="0"></iframe>
```



<iframe src="http://mengnankk.asia" width="500" height="500" frameborder="0"></iframe>



## 音乐视频播放

```
<audio src="" controls autoplay loop></audio>
<video src="" controls></video>
```

音视频文件引入时，不允许用户播放停止的

引入controls用来控制

loop属性用来控制循环播放

**autoplay用来自动播放，如果设置了，则音乐在打开页面时会进行播放，但大部分浏览器不会自动播放**

还可以通过



```
<audio controls>
浏览器不支持请更换浏览器
            <source src="">
            <source src="">
            <source src="">
        </audio>
```

<audio controls>
浏览器不支持请更换浏览器
            <source src="">
            <source src="">
            <source src="">
        </audio>

```
<embed src="" type="">
```

<embed src="" type="">

还可以通过这个标签进行使用
