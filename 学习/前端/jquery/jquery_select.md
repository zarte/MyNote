[TOC]
## jquery 获取 select 的 disabled 属性
```
var status = $("#ruletype").attr("disabled");
if(typeof(status) == "undefined" || oper == 'add' && typeof(status) != "undefined"){
$("#ruletype").removeAttr("disabled");
}
```
##jquery select 第二个 option
```
1.如何用jquery选中select的第二个option
用一个select，但是里面的值不固定，想通过jquery实现默认选中第2个option，该如何操作呢
<select>  
  <option value="1">Flowers</option>  
  <option value="2">Gardens</option>  
  <option value="3">Trees</option>  
</select>


$(function(){  
            $("select option:nth-child(2)").attr("selected" , "selected");  
});


2.jquery 根据第一个select对象获取第二个select对象
<table>
        <tr>
            <td>
                <select class="1">
                    <option value='1'>第1个</option>
                    <option value='2'>第2个</option>
                </select>
            </td>
            <td>
                <select >
                    <option value='3'>第3个</option>
                    <option value='4'>第4个</option>
                </select>
            </td>
        </tr>
        <tr>
        </tr>
    </table>
如上面的代码所示，我拿到了第一个select元素的对象，怎么根据该对象拿到第二个select对象的元素呢？
<script type="text/javascript">
$(function(){  
            var first=$("select:eq(0)");
var second=first.parent().siblings().children();
console.log(second.val())//3
console.log(second.html())//<option value='3'>第3个</option><option value='4'>第4个</option>
});
</script>


3.jquery获取select第二个option的值与内容
<script type="text/javascript">
$(function(){  
            alert($('#moneyset').find('option:eq(2)').val());//7
alert($('#moneyset').find('option:eq(2)').html());//第7个
});
</script>
</head>
<body>
<table>
        <tr>
            <td>
                <select class="1" id="moneyset">
                    <option value='1'>第1个</option>
                    <option value='2'>第2个</option>
   <option value='7'>第7个</option>
                    <option value='8'>第8个</option>
                </select>
            </td>
        </tr>
        <tr>


        </tr>
    </table>
</body>
</html>


4.select选中 第三个 option
<script type="text/javascript">
$(function(){
//方式一：
$('select option:eq(2)').attr('selected','selected');
//方式二：
$('#moneyset option:eq(2)').attr('selected','selected');
//方式三：
$('select').find('option:eq(2)').attr('selected','selected');
});
</script>


<table>
        <tr>
            <td>
                <select class="1" id="moneyset">
                    <option value='1'>第1个</option>
                    <option value='2'>第2个</option>
<option value='7'>第7个</option>
                    <option value='8'>第8个</option>
                </select>
            </td>
        </tr>
    </table>
```
##获取 select 选中（索引） （选中的是第几个option）
```
<select  id="moneyset">
                    <option value='11'>第1个</option>
                    <option value='11'>第2个</option>
    <option value='11'>第7个</option>
                    <option value='11'>第8个</option>
                </select>
jquery select 选中值为 1 的选项： $("#monenset").val(1); 或  $('#ruletype option:eq(1)').attr('selected','selected');
选中第3个：$('select').find('option:eq(2)').attr('selected','selected');//让第3个option选中

获取第3个option的text：console.log($('select option:eq(2)').text());//第3个option的text , 第7个
获取选中的是第几个option：console.log($('select').get(0).selectedIndex);//2 （索引从0开始）
```
##获取select索引
```
$("#select_id").change(function(){//code...}); //为Select添加事件，当选择其中一项时触发
 
var checkText=$("#select_id").find("option:selected").text(); //获取Select选择的text
 
var checkValue=$("#select_id").val(); //获取Select选择的Value
 
var checkIndex=$("#select_id ").get(0).selectedIndex; //获取Select选择的索引值
 
var maxIndex=$("#select_id option:last").attr("index"); //获取Select最大的索引值
```
## jquery radio取值，checkbox取值，select取值，radio选中，checkbox选中，select选中，及其相关 
```
获 取一组radio被选中项的值 
var item = $('input[name=items][checked]').val(); 
获 取select被选中项的文本 
var item = $("select[name=items] option[selected]").text(); 
select下拉框的第二个元素为当前选中值 
$('#select_id')[0].selectedIndex = 1; 
radio单选组的第二个元素为当前选中值 
$('input[name=items]').get(1).checked = true; 
获取值： 
文本框，文本区域：$("#txt").attr("value")； 
多选框 checkbox：$("#checkbox_id").attr("value")； 
单选组radio：   $("input[type=radio][checked]").val(); 
下拉框select： $('#sel').val(); 
控制表单元素： 
文本框，文本区域：$("#txt").attr("value",'');//清空内容 
$("#txt").attr("value",'11');//填充内容 
多选框checkbox： $("#chk1").attr("checked",'');//不打勾 
$("#chk2").attr("checked",true);//打勾 
if($("#chk1").attr('checked')==undefined) //判断是否已经打勾 
单选组 radio：    $("input[type=radio]").attr("checked",'2');//设置value=2的项目为当前选中项 
下拉框 select：   $("#sel").attr("value",'-sel3');//设置value=-sel3的项目为当前选中项 
$("<option value='1'>1111</option><option value='2'>2222</option>").appendTo("#sel")//添加下拉框的option 
$("#sel").empty()；//清空下拉框


$('#demo option:eq(1)').attr('selected','selected');
这样就完成了jquery select对某个option的选中。
```
##jQuery通过remove()方法移除网页中的指定元素
```
1.移除选中的元素
$("#selectingsellers option:selected").remove();
2.移除option不等于3的其他所有元素
$( "#skuPartMode option[value!='3']" ).remove();   

```
##jquery select 置灰
$("select").attr("disabled", "disabled");
jquery 1.6以上
$("select").prop("disabled", true);


##jquery给div的innerHTML赋值 
$("#id").html()=""; 
//或者 
$("#id").html("test");

##jQuery获取单选按钮(Radio)当前选中项的值
```
<input type="radio" id="radio3" name="disType" value="1" class="md-radiobtn" checked>
<input type="radio" id="radio4" name="disType" value="2" class="md-radiobtn" checked>
var value  = $('input[name="radioName"]:checked').val(); //获取被选中Radio的Value值
```

##jquery 全选
```
<input type="checkbox" name="choose">跳舞
<input type="checkbox" name="choose">跳水
<input type="checkbox" name="choose"/>跳棋
<input type="checkbox" name="choose"/>跑步
<input type="checkbox" name="allChecked" id="allChecked" onclick="SelectAll()"/>全选/取消


<script>
function SelectAll() {
 var checkboxs=document.getElementsByName("choose");
 for (var i=0;i<checkboxs.length;i++) {
  var e=checkboxs[i];
  e.checked=!e.checked;
 }
}
</script>
```


##jQuery通过name获取对象
```
使用jQuery获取name="nw"的input对象：$('input[name="nw"]');
使用$('input[name="nw"]').val()方法或$('input[name="nw"]').html()方法来获取其值。
和JavaScript获取对象值一样，input、select、textarea等表单类对象用val()方法来获取其值；
div、span等对象用html()获取其值，如：$('input[name="nw"]').val();
设置对象的值，如：$('input[name="nw"]').val('123');
注意：
1.通过name获取对象值，获取的是第1个对象的值
name是可以重复的
2.通过name设置对象值，设置的是所有对象的值
```

##radio change事件
```
  $("input:radio[name='OperateType']").change(function () {
        var item = $("input[name='OperateType']:checked").val();
        if (item == 1) {
            $("#publishSoft").show();
        }
        else if (item == 2) {
            $("#publishSoft").hide();
        }
        else if (item == 3) {
             $("#publishSoft").hide();
         } 
 });
       通过radio的不同选择，隐藏/显示一个div元素。
```

##定位到select option中已存在的记录
```
方式一：
 <select autofocus="autofocus">。
定义和用法
autofocus 属性是逻辑属性。
当设置该属性后，它规定在【页面加载】后下拉列表应该自动获得焦点。
所以必须加上下面的刷新下拉框代码，才能在页面不重新加载的情况下实现自动定位到选中的option所在的位置。


方式二【推荐】：
< div   class = "col-md-2" >
< select   multiple   class = "form-control"   id = "selectingstores" > </ select >
</ div >
< div   class = "col-md-2" >
< select   multiple   class = "form-control"   id = "selectedstores" > </ select > </ div >   

alert( "该门店已存在" );
var  myselect = $( "select#selectedstores" );

myselect[0].focus();
myselect[0] .options[j].selected =  true ;  //选中
myselect[0].selectedIndex = j;   //定位
```
## 组装select中所有option的value和text.
```
var str = $("#selectedstores option").map(function(){return $(this).val()+"："+$(this).text();}).get().join(",");
var str = $("#selectedstores option").map(function(){return "<option value='"+$(this).val()+"'>"+$(this).text()+"</option>"}).get().join(", ")
```

##jquery 设置radio可用与不可用
```
设置为不可用：
$("#input").attr("disabled",true);
$("#input").attr("disabled","disabled");


设置为可用：
$("#input").attr("disabled",false);//
$("#input").removeAttr("disabled");  //移除属性
$("#input").attr("disabled",""); //相当于移除属性
```

##jQuery获取Select选择的Text和Value: 
```
$("#select_id").change(function(){//code...}); //为Select添加事件，当选择其中一项时触发 

var checkText=$("#select_id").find("option:selected").text(); //获取Select选择的Text 
var checkValue=$("#select_id").val(); //获取Select选择的Value 
var checkIndex=$("#select_id ").get(0).selectedIndex; //获取Select选择的索引值 
var maxIndex=$("#select_id option:last").attr("index"); //获取Select最大的索引值 
```
##jQuery设置Select选择的 Text和Value:
```
$("#select_id ").get(0).selectedIndex=1;  //设置Select索引值为1的项选中

$("#select_id ").val(4);   // 设置Select的Value值为4的项选中
$("#select_id option[text='jQuery']").attr("selected", true);   //设置Select的Text值为jQuery的项选中
```

##jQuery添加/删除Select的Option项： 
```
var tar = $("#ruleType option[value='4']");
if(tar.length == 0){
  $("#ruleType").append("<option value='4'>满金额返券</option>"); //为Select追加一个Option(下拉项) 
}

$("#select_id").prepend("<option value='0'>请选择</option>"); //为Select插入一个Option(第一个位置) 
$("#select_id option:last").remove(); //删除Select中索引值最大Option(最后一个) 
$("#select_id option[index='0']").remove(); //删除Select中索引值为0的Option(第一个) 
$("#select_id option[value='3']").remove(); //删除Select中Value='3'的Option 
$("#select_id option[text='4']").remove(); //删除Select中Text='4'的Option 
```

##内容清空： 
```
$("#charCity").empty(); 
```

##jquery为select动态赋值
```
<script type="text/javascript" src="jquery-1.8.3.min.js"></script>
   <script type="text/javascript">
$(function(){
$("#selector1").change(function() {
   var t1 = +$("#selector1").find("option:selected").text();
console.log(t1)
var t2 = $("#selector2 option[value='"+t1+"']").val();
console.log(t2)
$("#selector2 option[value='"+t1+"']").attr("selected","selected");


});
})
</script>
 </head>


 <body>
<a href="javascript:void(0);" onclick="getSelectValue(1,2)" >click me</a>
<select id="selector1" class="selector"></select>
<select id="selector2" class="selector"></select>
<script>
function getSelectValue(a,b){
// 先清空
$("#selector"+a).empty();
$("#selector"+b).empty();
var arr = [1,2,3];
// 再循环赋值
for(var i=0;i<arr.length;i++){     
$("#selector"+a).append("<option value='"+i+"'>"+arr[i]+"</option>");
$("#selector"+b).append("<option value='"+i+"'>"+arr[i]+"</option>");
           }
}
</script>
 </body>
```
##option所有的选项都列出来
```
添加 multiple属性即可。
<select multiple class="form-control" id="giftGiveType">
  <option value='1'>全部发放</option>
  <option value='2'>任选N件</option>
</select>
```
##jQuery中获得选中select值
第一种方式
$('#testSelect option:selected').text();//选中的文本
$('#testSelect option:selected') .val();//选中的值
$("#testSelect ").get(0).selectedIndex;//索引

第二种方式
$("#tesetSelect").find("option:selected").text();//选中的文本
…….val();
…….get(0).selectedIndex;
