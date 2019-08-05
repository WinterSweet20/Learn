# HTML基础教程

## HTML简介

```html
<html>
<body>
<h1>我的第一个标题</h1>
<p>我的第一个段落。</p>
</body>
</html>
```

HTML 不是一种编程语言，而是一种*标记语言*

## HTML基础

```html
<h1>
    这是一级标题
</h1>
<h2>
    这是二级标题
</h2>
<p>
    这是一个段落
</p>
<img src="http://5b0988e595225.cdn.sohucs.com/images/20180402/5d060aea1d8a4c83a428eb809b7b4255.jpeg" />
<a href = "www.baidu.com">这是一个指向百度的链接</a>
```

HTML基础的四种标签

## HTML元素

HTML 元素指的是从开始标签（start tag）到结束标签（end tag）的所有代码。

HTML标签对大小写不敏感

## HTML属性

一个列出了大多数属性的例子

```html
<p class="" id="" style="" title="">
    
</p>
```

## HTML标题

| 标签         | 描述     |
| ------------ | -------- |
| <h1> to <h6> | HTML标题 |
| <hr/>        |定义水平线          |
|<!--  -->|定义注释|

## HTML段落

```html
<p>
    这是一行
    <br />
    这是新的一行
</p>
```

用于产生新的段落时候分行

## HTML样式

```html
<html>
    <body style="background-color:red">
        <p style="font-family:times;color:blue;font-size:20px;text-align:center">
            这是一个有格式的段落
        </p>
    </body>
</html>

```

<p style="font-family:times;color:blue;font-size:20px;text-align:center">
            这是一个有格式的段落
        </p>

上面的就是应用格式之后的样子

## HTML文本格式

## 文本格式化标签

| 标签     | 描述           |
| :------- | :------------- |
| <b>      | 定义粗体字     |
| <big>    | 定义大号字。   |
| <em>     | 定义着重文字。 |
| <i>      | 定义斜体字。   |
| <small>  | 定义小号字。   |
| <strong> | 定义加重语气。 |
| <sub>    | 定义下标字。   |
| <sup>    | 定义上标字。   |
| <ins>    | 定义插入字。   |
| <del>    | 定义删除字。   |

实例:

<b>粗体字</b><big>大号字</big><em>着重文字</em><i>斜体字</i><small>小号字</small><strong>加重语气</strong><sub>下标</sub><sup>上标</sup><ins>插入</ins><del>删除</del>

“计算机输出“标签和引用、引用和术语定义[链接](http://www.w3school.com.cn/html/html_formatting.asp)

## HTML引用

HTML短的引用<sub>接下来直接这上面上样式了

<p>比如<q>这是一个短的引用</q></p>
<p>比如<blockquote cite="">这是一个长的引用</blockquote></p>
<p><abbr title="WinterSweet.Tang">WS.T</abbr>是一个缩略词</p>
<p><dfn title="WinterSweet.Tang">WS.T</dfn>是一个缩略词定义项目<i>Define</i></p>
### 用于联系信息的HTML

<address>
    WinterSweet<br/>
    QQNum:2416478812<br/>
    PhoneNum:13458724058<br/>
</address>

正如上面看到的，通常以斜体显示，并且会在前后换行

### 用于著作的标题

<p><cite>Lovely</cite> WinterSweet</p>
通常以斜体显示

### 用于双向重写的HTML

<bdo dir="rtl">WinterSweet</bdo>
就是反过来写，看起来怪怪的

## 计算机代码元素

### HTML键盘格式

<p><kbd>File  | Open</kbd></p>
### HTML样本格式

<samp>demo.example.com login:Apr 12 09:10:17</samp>

### HTML代码格式

```html
<code>
	var person = {
    	firstName:"Tang",
    	lastName:"WinterSweet"
    }
</code>
```



不会保留多余的空格和换行

如需保留，可以

```html
<code>
<pre>
	代码在这里
</pre>
</code>
```

### HTML变量格式化

<p><var>E=mc<sub>2</sub></var></p>
## HTML注释

<!-- 这就是注释啦 -->

### 条件注释

```html
<!-- [if IE 8]>

​	只有在Internet Explorer才会执行的HTML标签

<![endif]-->
```



### 软件程序标签

各种 HTML 软件程序也能够生成 HTML 注释。

例如 <!--webbot bot--> 标签会被包围在由 FrontPage 和 Expression Web 创建的 HTML 注释中。

作为一项规则，这些标签的存在，有助于对创建这些标签的软件的支持。

## HTML CSS

### 外部样式表

当样式需要被应用到很多页面的时候，外部样式表将是理想的选择。使用外部样式表，你就可以通过更改一个文件来改变整个站点的外观。

```html
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```

### 内部样式表

当单个文件需要特别样式时，就可以使用内部样式表。你可以在 head 部分通过 <style> 标签定义内部样式表。

```html
<head>

<style type="text/css">
body {background-color: red}
p {margin-left: 20px}
</style>
</head>
```

### 内联样式

当特殊的样式需要应用到个别元素时，就可以使用内联样式。 使用内联样式的方法是在相关的标签中使用样式属性。样式属性可以包含任何 CSS 属性。以下实例显示出如何改变段落的颜色和左外边距。

```html
<p style="color: red; margin-left: 20px">
This is a paragraph
</p>
```

==好像没怎么看懂==

| 标签                                                 | 描述                                                         |
| :--------------------------------------------------- | :----------------------------------------------------------- |
|<style>  | 定义样式定义。                                               |
|<link>| 定义资源引用。                                               |
| <div>  | 定义文档中的节或区域（块级）。                               |
|<span>   | 定义文档中的行内的小块或区域。                               |
|<font>   | 规定文本的字体、字体尺寸、字体颜色。不赞成使用。请使用样式。 |
|<basefont>| 定义基准字体。不赞成使用。请使用样式。                       |
|<center> | 对文本进行水平居中。不赞成使用。请使用样式。                 |

## HTML链接

我们通常用<a>标签在HTML中创建链接

<a href="www.baidu.com">baidu</a>

<font>在新窗口打开<a href="www.baidu.com" target="_blank">baidu</a></font>

### 锚

> 大概就是页面内跳转

```html
<a name="tips">跳转之后的地方</a>
<a href="#tips">跳转</a>
<a href="localhost:address#tips">其它页面也可以跳转</a>
```

> 如果浏览器找不到锚，会跳转到顶部，不会报错
>
> 在写链接的时候最好加上最后的`/`，例如`www.baidu.com/`不然会产生两次HTML请求

### 创建电子邮件链接

看不懂，查看[示例](http://www.w3school.com.cn/tiy/t.asp?f=html_mailto2)

## HTML图像

<img src="" alt="无法显示图片" />

src是图片的地址，alt是在图片无法显示的时候的提示信息

==慎用图片==

一个比较全的例子：

```html
<html>
    <body background="image.jpg">
        <p>
            文本中的<img src="image.jpg" />图片
        </p>
        <p>
            <img src="imag.jpg" align = "left" width="50" height="50" alt="提示"/>图片在左边
        </p>
    </body>
</html>
```
>第一个是文字中间的图片
>第二个是浮在文字左边的宽高为50，没显示出来提示`提示`的图片

| 标签                                             | 描述                         |
| :----------------------------------------------- | :--------------------------- |
| <img> | 定义图像。                   |
|<map> | 定义图像地图。               |
| <area> | 定义图像地图中的可点击区域。 |

`<map>`和`<area>`没看懂，[原文](http://www.w3school.com.cn/html/html_images.asp)

## HTML表格

<table border="2" cellpadding = "10" cellspacing = "10"
       bgcolor = "blue">
    <caption>这是一个表格标题</caption>
<tr>
<th>表头</th>
<th>另一个表头</th>
</tr>
<tr>
<td>row 1, cell 1</td>
<td>row 1, cell 2</td>
</tr>
<tr>
<td>row 2, cell 1</td>
<td>row 2, cell 2</td>
</tr>
    <tr>
    <td>
        &nbsp;</td>
    <td>
        </td></tr>
</table>
|选项|效果|
|------|------|
| `border=1` | 表示带边框 |
| `cellpadding="10"` | 表示格边距 |
| `cellspacing="10"` | 表示格间距 |
| `bgcolor="blue"` | 表示背景颜色 |
|`background="image.jpg"`|表示背景图片|
|`<td>`|也可以添加背景颜色和背景图片|
|`<td align="center"`|对齐方式|
|`<table frame="box"></table>`|定义边框格式||

| frame | 效果     |
| ----- | -------- |
| `box  ` | 四周边框 |
|`above`|上边框|
|`below`|下边框|
|`hsides`|上下边框|
|`vsides`|左右边框|


`<th>`是表头，大部分表现为加粗居中

如果一个单元格没有数据，可能会边框都显示不出来，所以可以添加占位符`&nbsp`记得加分号`&nbsp;`

使用`<caption>`添加标题

使用例`<th colspan="2">`来使一个表头包含两列

单元格内可以使用标签，例：`<td><p>包含了一个段落</p></td>`

## HTML列表

<ul>
    <li>无序列表第一行</li>
    <li>无序列表第二行</li>
</ul>



<ol>
    <li>有序列表第一行</li>
    <li>有序列表第二行</li>
</ol>



<dl>
    <dt>自定义列表dt</dt>
    <dd>自定义列表dd</dd>
</dl>

定义列表的列表项内部可以使用段落、换行符、图片、链接以及其他列表等等。



还可以嵌套列表：

<ul>
    <li>Car</li>
    	<ul>
            <li>SportsCar</li>
            <ul>
                <li>Porsche</li>
            </ul>
    </ul>
</ul>



| 无序表符号      | 含义     |
| --------------- | -------- |
| `type="disc"`   | 实心圆   |
| `type="circle"` | 空心圆   |
| `type="square"` | 实心方块 |

| 有序表符号 | 含义  |
| ---------- | ----- |
| 缺省       | 123   |
| `type="A"` | ABC   |
| `type="a"` | abc   |
| `type="i"` | i,v,x |

## <font>HTML`<div>`和`<span>`</font>

`<div>&<span>`就是一种块级元素和内联元素

块级元素还包括：`<h1>, <p>, <ul>, <table>`

内联元素还包括：`<b>, <td>, <a>, <img>`

块级元素会以新行显示，内联元素不会

`<div>`可用于组合HTML其他元素，在设置css的时候可以很大一块设置  *我想这就是它的作用*

`<span>`可以作为文本的容器，可以通过css设置样式

## HTML类

```html
<!DOCTYPE html>
<html>
    <head>
        <style>
            .mydiv{
                background-color:red;
                color:blue;
            }
            span.myspan{color:red;}
        </style>
    </head>
    <body>
        <div class="mydiv">
            <p>一个段落<span class="myspan">中的特殊</span></p>
        </div>
        <div class="mydiv">
            <p>另一段落</p>
        </div>
    </body>
</html>
```

上面的效果：

> 一个段落和另一个段落的格式是一样的
>
> > 都是背景红色，字体蓝色
>
> `span`中的文字是特殊的
>
> >红色



## HTML布局

|HTML5元素|含义|
| ------- | ------------------------------ |
| header  | 定义文档或节的页眉（上边那个） |
| nav     | 定义导航链接的容器（左边那个）       |
| section | 定义文档中的节（中间的文章）          |
| article | 定义独立的自包含文章           |
| aside   | 定义内容之外的内容 |
| footer  | 定义文档或节的页脚（下边那个）       |
| details | 定义额外的细节                 |
| summary | 定义 details 元素的标题        |

==<i>其他的没试过，空了一定要试</i>==

