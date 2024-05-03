# html
## 文本
### 结构
<(opening tag)  (attribute)> content </closing tag>\
上述称为一个element

一个html文件大概布局
```html
<!doctype html> 声明文档类型
<html lang="ah-CN">
<html>
<head>
<meta charset="utf-8" /> 不能由其他html相关元素表示的元数据

<title>xxx</title> 页面的标题
</head>

<body>
<!-- 其中为注释内容 -->

</body>
</html>
```

### 标签类型
<a href="https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a">ref-html元素</a>\
<a href="https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/accesskey">ref-html全局属性</a>\
<a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/accept">ref-html属性</a>\
<a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/button">ref-html input</a>
```html
<meta
name="author" content="chris wu"
name="description" content="the docs is a test for xxx"
></meta>
```
```布局相关标签
<header>
<nav>
<main>
<aside>
<footer>
```
```html
<link href="main.css" rel="stylesheet" /> 链接外部样式表
rel="icon/stylesheet" //指定当前文档与被链接文档之间的关系
```
```html
<p>段落</p> 
<h1>标题</h1>

<ul> 无序列表
<li></li>
</ul>

<ol> 有序列表
<li></li>
</ol>

<em>强调</em>
<strong></strong>
```

```html
<span>容器 无语义</span>
<div></div>
<br>
<hr>

<a href="www.baidu.com" 
    title="补充信息，鼠标悬空时出现" 
    down="提供默认的保存名文件">
content</a>
//相对路径、绝对路径，参照linux
<img src="" alt=""></img>
```

### 网页实例
[html文件语义检测](https://validator.w3.org/#validate_by_upload)


## 表格
```html
<table id="xxx"> </table>
<caption>标题</caption>

<thead>
<tfoot>
<tbody> //不显示，样式相关


<th scope="col"> table header 
  scope:行还是列所在表头
<tr colspan="2" rowspan="2"colspan="2">
<td>hi, i`m your first cell</td>
</tr>

<colgroup>
<col style="background-color: yellow" span="2"/>
<col>
<col style="xxx">
</colgroup>

```
## 多媒体

# css


# ref
MDN;\
