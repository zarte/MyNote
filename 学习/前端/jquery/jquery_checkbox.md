[TOC]
##全选与反选
```
function  SelectAllType(){
             var  checkboxs = $( 'input[name="promobj"]' );
             for ( var  i=0; i<checkboxs.length;i++){
                 var  e = checkboxs[i];
                e.checked =  true ;
            }         }  
function  SelectAllType(){
             var  checkboxs = $( 'input[name="promobj"]' );
             for ( var  i=0; i<checkboxs.length;i++){
                 var  e = checkboxs[i];
                e.checked =  !e.checked    ;
            }
        }  

```

##checkbox循环选中
```
方式一：
var  sellModel = [];
        $( "input[name='sellModel']:checked" ).each( function (){
            sellModel.push($( this ).val());         });  
if(sellModel.length == 0){
    alert("请选择商家");

    return;

}


方式二：
var  obj = $( "input[name='sellModel']" );
         var  sellModel = []; //参与商家
         for (k  in  obj){
             if (obj[k].checked)
                sellModel.push(obj[k].value);
        }
        sellModel = JSON.stringify(sellModel);
         if (sellModel ==  "[]"  || sellModel.length == 0){
            alert( "请选择参与商家" );
             return  ;
        }  

```

##checkbox选中
```
$( "#joinBuySeller" ).attr( "checked" , true );   

jquery判断checked的三种方法:
.attr('checked):   //看版本1.6+返回:”checked”或”undefined” ;1.5-返回:true或false
.prop('checked'): //16+:true/false
.is(':checked'):    //所有版本:true/false//别忘记冒号哦
jquery赋值checked的几种写法:
所有的jquery版本都可以这样赋值:
// $("#cb1").attr("checked","checked");
// $("#cb1").attr("checked",true);
jquery1.6+:prop的4种赋值:
// $("#cb1″).prop("checked",true);//很简单就不说了哦
// $("#cb1″).prop({checked:true}); //map键值对
// $("#cb1″).prop("checked",function(){
return true;//函数返回true或false
});
//记得还有这种哦:$("#cb1″).prop("checked","checked");
```

##checkbox 置灰 可用
```
< input   type = "checkbox"   id = "mode_self"   name = "sellModel"   value = "0"   checked = "true" > < label   for = "mode_self"   style =" padding-left :  15px ;" > 自营 </ label > <%--自营商家id=3 --%>   

< input   type = "checkbox"   id = "mode_join"   name = "sellModel"   value = "1" > < label   for = "mode_join"   style =" padding-left :  15px ;" > 限商家 </ label >   

//置灰

$( "#mode_self" ).attr( "disabled" , "disabled" );  
$( "#mode_self" ).attr( "disabled" , true );

//可用  

$( "#mode_self" ).removeAttr( "disabled" );

$( "#mode_self" ). attr ( "disabled",false );
```

##checkbox 及时 刷新
```
1.jquery 方式
一般登录的时候 都有个记住用户名 记住密码 的两个checkbox 多选框  
用jquerymobile 做页面 ，当勾选checkbox 时总是不能获取它正确的值。  

解决办法：    
[code]
$('input[type="checkbox"]').bind('click',function() {  
       $(this).prop('checked').checkboxradio("refresh");   // 绑定事件及时更新checkbox的checked值  
  });

2.js方式 
如果要用js去改变checkbox的值时也要及时刷新。   

$('input [type="checkbox"]').attr('checked',false).checkboxradio("refresh");  
$('input [type="checkbox"]').attr('checked',false).checkboxradio("refresh");   

原因：因为手动改变它的值后，jquerymobile不能重新渲染。 这样页面显示的值和实际值就不一样了。
```

##其他 控件 刷新
```
复选按钮  
$("input[type='checkbox']").attr("checked",true).checkboxradio("refresh");  

单选按钮组:  
$("input[type='radio']").attr("checked",true).checkboxradio("refresh");  

选择列表::  
var myselect = $("select#foo");  
myselect[0].selectedIndex = 3;  
myselect.selectmenu("refresh");   

滑动条  
$("input[type=range]").val(60).slider("refresh");  

开关 (they use slider):  
var myswitch = $("select#bar");  
myswitch[0].selectedIndex = 1;  
myswitch .slider("refresh");
```

##checkbox去除选中
```
$("#chk input:checkbox").removeAttr("checked");
```

##jquery获取checkbox选中的多个值
```
var userInclude = [];
$('input[name="promobj"]:checked').each(function(){
userInclude.push($(this).val());
});
console.log('userInclude2:'+userInclude);
```

##jquery checkbox 是否选中
```
var flag1 = $('#checkAll').prop('checked');
var hasChk = $('#checkAll').is(':checked');
```

## 获得checkbox里选中的多个值
```
思路：利用name属性值获取checkbox对象，然后循环判断checked属性（true表示被选中，false表示未选中）
1、HTML结构
<input type="checkbox" name="test" value="1"/><span>1</span>
<input type="checkbox" name="test" value="2"/><span>2</span>
<input type="checkbox" name="test" value="3"/><span>3</span>
<input type="checkbox" name="test" value="4"/><span>4</span>
<input type="checkbox" name="test" value="5"/><span>5</span>
<input type='button' value='提交' onclick="fun()"/>
2、javascript代码
function fun(){
    obj = document.getElementsByName("test");
    //obj = $("input[name='test']");

    check_val = [];
    for(k in obj){
        if(obj[k].checked)
            check_val.push(obj[k].value);
    }
    alert(check_val);
}
```
##checkbox 置灰
//两种方法设置disabled属性
$('#areaSelect').attr("disabled",true);
$('#areaSelect').attr("disabled","disabled");
//三种方法移除disabled属性
$('#areaSelect').attr("disabled",false);
$('#areaSelect').removeAttr("disabled");
$('#areaSelect').attr("disabled","");