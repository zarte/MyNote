[TOC]
###介绍
```
Handlebars 是 JavaScript 一个语义模板库，通过对view和data的分离来快速构建 web 模板。它采用"Logic-less template"（无逻辑模版）的思路，在加载时被预编译，而不是到了客户端执行到 代码 时再去编译，
这样可以保证模板加载和运行的速度。Handlebars兼容Mustache，你可以在Handlebars中导入Mustache模板。
```
###使用
```
目前handlebars. JS 已经被许多项目广泛使用了，handlebars是一个纯 js 库，因此你可以像使用其他JS脚本一样用script标签来包含handlebars.js
`<script type="text/javascript" src=".js/handlebars.js"></script> `
```
###基本语法
```
Handlebars expressions 是handlebars模板中最基本的单元，使用方法是加两个花括号 {{value}} , handlebars模板会自动匹配相应的数值，对象甚至是函数。

<div class="demo">  
    <h1>{{name}}</h1>
    <p>{{content}}</p>
</div>
```
###Handlebars的内置块表达式（Block helper） 
```
1.each block helper

你可以使用内置的 {{#each}} helper遍历列表块内容，用 this 来引用遍历的元素
例如：
<ul>  
    {{#each name}}
        <li>{{this}}</li>
    {{/each}}
</ul> 

对应适用的json数据
{
    name: ["html","css","javascript"]
};
这里的this指的是数组里的每一项元素，和上面的Block很像，
但原理是不一样的这里的name是数组，而内置的each就是为了遍历数组用的，更复杂的数据也同样适用。

2.if else block helper

`{{#if}}`
就你使用JavaScript一样，你可以指定条件渲染DOM，如果它的参数返回
false，undefined, null, "" 或者 [] (a "falsy" value),
Handlebar将不会渲染DOM，如果存在`{{#else}}`则执行`{{#else}}`后面的渲染

例如：

{{#if list}}
<ul id="list">  
    {{#each list}}
        <li>{{this}}</li>
    {{/each}}
</ul>  
{{else}}
    <p>{{error}}</p>
{{/if}}

对应适用json数据
var data = {  
    info:['HTML5','CSS3',"WebGL"],
    "error":"数据取出错误"
}
这里`{{#if}}`判断是否存在list数组，如果存在则遍历list，如果不存在输出错误信息.
```