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
### 布局
   - 学习盒模型，包括内容区域、内边距、边框和外边距。
   - 掌握常用的布局方法，如浮动、定位、Flexbox和Grid布局。:wq


3. **响应式设计**：
   - 学习响应式设计的原理和方法。
   - 掌握媒体查询和视口单位的使用。

### 中级阶段：
4. **CSS预处理器**：
   - 学习使用CSS预处理器，如Sass、Less或Stylus，提高CSS的可维护性。
   - 掌握变量、嵌套、Mixin等概念。

5. **模块化CSS**：
   - 学习模块化的CSS方法，如BEM、OOCSS、SMACSS等。
   - 掌握组件化开发的思想，提高代码的复用性和可维护性。

6. **性能优化**：
   - 学习CSS性能优化的方法，包括减少HTTP请求、压缩代码、减少选择器嵌套等。
   - 掌握提高页面加载速度的技巧。

### 高级阶段：
7. **动画和过渡**：
   - 学习使用CSS实现动画和过渡效果。
   - 掌握关键帧动画、动画属性和过渡属性的使用。

8. **CSS框架**：
   - 学习使用现有的CSS框架，如Bootstrap、Foundation等。
   - 掌握框架的使用方法和定制化。

9. **现代CSS**：
   - 学习新的CSS特性和规范，如CSS变量、自定义属性、网格布局等。
   - 掌握未来CSS的发展方向和趋势。

### 大师阶段：
10. **创新和实践**：
    - 探索CSS的创新应用，如3D动画、变形效果等。
    - 参与开源项目或社区，分享经验和知识。

11. **深入研究**：
    - 深入研究CSS规范和标准，了解其内部原理和设计思想。
    - 参与标准制定或贡献代码。

12. **教学和分享**：
    - 成为CSS领域的专家，撰写博客、教程或出版书籍。
    - 在行业会议或讲座上分享经验和见解。





























# javascript


# ref
MDN;\
pink前端 黑马
