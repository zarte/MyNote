##获取以xx开头的元素
```

jquery 中 $("[id^='ds']") 
id^='ds'表示查找id以ds开头的所有元素，例如id="dsdd" id="dshhhh" 都是符合的元素
```
##jquery 相邻元素
```
实例一：
<div>
<p id="1">1</p>
<p id="2">2</p>
<p id="3">3</p>
</div>
<script language="javascript">
$("#2").next();//这个获取的等同于$("#3")
$("#2").prev();//这个获取的等同于$("#1")
$("#2").siblings();//这个获取的等同于$("#1,#3")
</script>

jquery中的相邻元素，就是同级的标签元素的意思~
jquery中相邻元素可以通过 以下方式获取
 next()相邻下一个同辈元素
 prev()相邻上一个同辈元素
 siblings()相邻所有同辈元素


实例二：检索每个段落，找到类名为 "selected" 的前一个同胞元素：
$("p").prev(".selected")
 .prev() 方法允许我们在 DOM 树中搜索这些元素的前一个同胞元素，并用匹配元素构造一个新的 jQuery 对象。
.prev()方法找到的是当前元素的同级的前一个元素




```
##jquery选中以什么开头的元素

```
$("[id^=percent]").size()
^=：表示以什么开头


$=：表示以什么结尾


~=：表示包含什么


id：表示按id选择




$("input[id^=giftNum]").each(function(i,obj){
giftNum += $(obj).val() + ",";
});
```
##jquery根据class获取第一个元素
```
<ul>
<li>11</li>
<li>22</li>
<li>33</li>
<li>44</li>
<li>55</li>
</ul>
要取到11 所在的 li 元素，有一下几种方法：


$('ul').find('li:first');
$('ul li:first');
$('ul li').eq(0);
var obj = $('#xx')[0] 获得dom对象
2 对于数组 
var obj = $('.xx').each(function(){
alert(this)//这里 this获得的就是每一个dom对象 如果需要jquery对象 需要写成$(this)
});
```


##jquery 如何用eq的选择器写，除第一个元素
```
除第一个就不要用eq了,用gt(大于): 
$('.firs  li:gt(0)').removeClass('hover')
```


##jquery同时对多个对象设置事件
```
// 点击全部内容中的取消按钮，返回列表
$("#btn-back1,#btn-back2,#btn-back3").on("click", function() {
    $.CommonUtils.redirect(basePath + '/message/main.do'); });  

```
##jquery选择器总结
```
1.通过name获取对象
使用jQuery获取name="nw"的input对象：$('input[name="nw"]');
使用$('input[name="nw"]').val()方法或$('input[name="nw"]').html()方法来获取其值

 
$("#myELement")    选择id值等于myElement的元素，id值不能重复在文档中只能有一个id值是myElement所以得到的是唯一的元素 
$("div")           选择所有的div标签元素，返回div元素数组 
$(".myClass")      选择使用myClass类的css的所有元素 
$("*")             选择文档中的所有的元素，可以运用多种的选择方式进行联合选择：例如$("#myELement,div,.myclass") 
 
层叠选择器： 
$("form input")         选择所有的form元素中的input元素 
$("#main > *")          选择id值为main的所有的子元素 
$("label + input")     选择所有的label元素的下一个input元素节点，经测试选择器返回的是label标签后面直接跟一个input标签的所有input标签元素 
$("#prev ~ div")       同胞选择器，该选择器返回的为id为prev的标签元素的所有的属于同一个父元素的div标签 
 
基本过滤选择器： 
$("tr:first")               选择所有tr元素的第一个 
$("tr:last")                选择所有tr元素的最后一个 
$("input:not(:checked) + span")   
 
过滤掉：checked的选择器的所有的input元素 
 
$("tr:even")               选择所有的tr元素的第0，2，4... ...个元素（注意：因为所选择的多个元素时为数组，所以序号是从0开始） 
 
$("tr:odd")                选择所有的tr元素的第1，3，5... ...个元素 
$("td:eq(2)")             选择所有的td元素中序号为2的那个td元素 
$("td:gt(4)")             选择td元素中序号大于4的所有td元素 
$("td:ll(4)")              选择td元素中序号小于4的所有的td元素 
$(":header") 
$("div:animated") 
内容过滤选择器： 
 
$("div:contains('John')") 选择所有div中含有John文本的元素 
$("td:empty")           选择所有的为空（也不包括文本节点）的td元素的数组 
$("div:has(p)")        选择所有含有p标签的div元素 
$("td:parent")          选择所有的以td为父节点的元素数组 
可视化过滤选择器： 
 
$("div:hidden")        选择所有的被hidden的div元素 
$("div:visible")        选择所有的可视化的div元素 
属性过滤选择器： 
 
$("div[id]")              选择所有含有id属性的div元素 
$("input[name='newsletter']")    选择所有的name属性等于'newsletter'的input元素 
 
$("input[name!='newsletter']") 选择所有的name属性不等于'newsletter'的input元素 
 
$("input[name^='news']")         选择所有的name属性以'news'开头的input元素 
$("input[name$='news']")         选择所有的name属性以'news'结尾的input元素 
$("input[name*='man']")          选择所有的name属性包含'news'的input元素 
 
$("input[id][name$='man']")    可以使用多个属性进行联合选择，该选择器是得到所有的含有id属性并且那么属性以man结尾的元素 
 
子元素过滤选择器： 
 
$("ul li:nth-child(2)"),$("ul li:nth-child(odd)"),$("ul li:nth-child(3n + 1)") 
 
$("div span:first-child")          返回所有的div元素的第一个子节点的数组 
$("div span:last-child")           返回所有的div元素的最后一个节点的数组 
$("div button:only-child")       返回所有的div中只有唯一一个子节点的所有子节点的数组 
 
表单元素选择器： 
 
$(":input")                  选择所有的表单输入元素，包括input, textarea, select 和 button 
 
$(":text")                     选择所有的text input元素 
$(":password")           选择所有的password input元素 
$(":radio")                   选择所有的radio input元素 
$(":checkbox")            选择所有的checkbox input元素 
$(":submit")               选择所有的submit input元素 
$(":image")                 选择所有的image input元素 
$(":reset")                   选择所有的reset input元素 
$(":button")                选择所有的button input元素 
$(":file")                     选择所有的file input元素 
$(":hidden")               选择所有类型为hidden的input元素或表单的隐藏域 
 
表单元素过滤选择器： 
 
$(":enabled")             选择所有的可操作的表单元素 
$(":disabled")            选择所有的不可操作的表单元素 
$(":checked")            选择所有的被checked的表单元素 
$("select option:selected") 选择所有的select 的子元素中被selected的元素 
 
  
 
选取一个 name 为”S_03_22″的input text框的上一个td的text值
$(”input[@ name =S_03_22]“).parent().prev().text() 
 
名字以”S_”开始，并且不是以”_R”结尾的
$(”input[@ name ^='S_']“).not(”[@ name $='_R']“) 
 
一个名为 radio_01的radio所选的值
$(”input[@ name =radio_01][@checked]“).val(); 
 
  
 
  
 
$("A B") 查找A元素下面的所有子节点，包括非直接子节点
$("A>B") 查找A元素下面的直接子节点
$("A+B") 查找A元素后面的兄弟节点，包括非直接子节点
$("A~B") 查找A元素后面的兄弟节点，不包括非直接子节点 
 
1. $("A B") 查找A元素下面的所有子节点，包括非直接子节点 
 
例子：找到表单中所有的 input 元素 
 
HTML 代码: 
 
<form>
<label>Name:</label>
<input name="name" />
<fieldset>
      <label>Newsletter:</label>
      <input name="newsletter" />
</fieldset>
</form>
<input name="none" /> 
jQuery 代码: 
 
$("form input") 
结果: 
 
[ <input name="name" />, <input name="newsletter" /> ] 
 
2. $("A>B") 查找A元素下面的直接子节点 
例子：匹配表单中所有的子级input元素。 
 
HTML 代码: 
 
<form>
<label>Name:</label>
<input name="name" />
<fieldset>
      <label>Newsletter:</label>
      <input name="newsletter" />
</fieldset>
</form>
<input name="none" /> 
jQuery 代码: 
 
$("form > input") 
结果: 
 
[ <input name="name" /> ] 
 
3. $("A+B") 查找A元素后面的兄弟节点，包括非直接子节点 
例子：匹配所有跟在 label 后面的 input 元素 
 
HTML 代码: 
 
<form>
<label>Name:</label>
<input name="name" />
<fieldset>
      <label>Newsletter:</label>
      <input name="newsletter" />
</fieldset>
</form>
<input name="none" /> 
jQuery 代码: 
 
$("label + input") 
结果: 
 
[ <input name="name" />, <input name="newsletter" /> ] 
 
 
4. $("A~B") 查找A元素后面的兄弟节点，不包括非直接子节点 
例子：找到所有与表单同辈的 input 元素 
 
HTML 代码: 
 
<form>
<label>Name:</label>
<input name="name" />
<fieldset>
      <label>Newsletter:</label>
      <input name="newsletter" />
</fieldset>
</form>
<input name="none" /> 
jQuery 代码: 
 
$("form ~ input") 
结果: 
 
[ <input name="none" /> ] 
```


##jquery中获取属性值为变量的元素
```

JQuery中根据属性或属性值获得元素一般是这样写的:
var curriculumNode = $("input[id='password']");
如果属性值是个变量，则：
var curriculumNode = $("input[name='"+password+"']");
```