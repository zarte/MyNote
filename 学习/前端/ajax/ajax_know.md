[TOC]
##ajax批量删除
```
js页面jquery代码： 
$(document).ready(function() {
    // 全选 
    $("#allChk").click(function() {
        $("input[name='subChk']").prop("checked", this.checked);
    });
    // 单选 
    var subChk = $("input[name='subChk']") subChk.click(function() {
        $("#allChk").prop("checked", subChk.length == subChk.filter(":checked").length ? true: false);
    });
    /* 批量删除 */
    $("#del_model").click(function() {
        // 判断是否至少选择一项 
        var checkedNum = $("input[name='subChk']:checked").length;
        if (checkedNum == 0) {
            alert("请选择至少一项！");
            return;
        }
        // 批量选择 
        if (confirm("确定要删除所选项目？")) {
            var checkedList = new Array();
            $("input[name='subChk']:checked").each(function() {
                checkedList.push($(this).val());
            });
            $.ajax({
                type: "POST",
                url: "deletemore",
                data: {
                    'delitems': checkedList.toString()
                },
                success: function(result) {
                    $("[name ='subChk']:checkbox").attr("checked", false);
                    window.location.reload();
                }
            });
        }
    });
});

页面元素： < a href = "#" id="del_model" > <span > 删除用户 < /span> 
<th class="tal"><input type="checkbox" id="allChk"/ > 全选 < /th> 
<td><input type="checkbox" name="subChk" value="${user.id}"/ > </td>

回调函数，在请求完成后需要进行的操作：此处是把选中的checkbox去掉（因为是用到了freemarker的list循环，去掉是数据后checkbox序号变化，还有有相应未知的checkbox被选中，需要去掉）
success: function(result) {
    $("[name = 'items']:checkbox").attr("checked", false);
    window.location.reload();
}

java后台代码：
@RequestMapping(value = "/deletemore", method = RequestMethod.POST) 
public String deleteMore(HttpServletRequest request, HttpServletResponse response) { 
    String items = request.getParameter("delitems"); 
    String[] item = items.split(","); 
    for (int i = 0; i < item.length; i++) { 
        userService.delete(Integer.parseInt(item[i])); 
    } 
    return "redirect:list"; 
} 
```

##ajax返回值供其他函数调用
```
function  getSpec(priceId,skuId){
     var  specCost =  "" ;
    $.ajax({
        url: '${ctx}/cust/multiPriceQueryDetail.do' ,
        data:{ "skuid" :skuId, "priceId" :priceId},
        dataType: 'json' ,
        cache: false ,
        type: 'post' ,
        async: false ,//先执行完此ajax函数再执行后面的语句
        success: function (data){
             var  res =  "" ;
             if (data.resultCode ==  "1" )
            {
                 var  prplist = data.databody;
                 if (prplist.length > 0)
                {
                    specCost=prplist[0].specCost==0? "--" :getYuanByFen(prplist[0].specCost);
                }
            }
        }
    });
     return  specCost; }   

```

##完整的ajax请求
```
项目中的一次经验，后台返回可以正常返回，data.resultCode=1,但前台就是查询不出数据。
原因是后台返回的json数据格式有问题。ajax请求处理失败，success执行不到，执行了error, 错误信息parseError.
http://www.bejson.com/  可在线检测json格式是否正确。
http://www.json.cn/  可将json压缩，将json转为xml.


$.ajax({
            url: '${ctx}/cust/multiPriceQuery.do' ,
            data:{ "skuid" :skuid, "siteId" :siteId},
            dataType: 'json' ,
            cache: false ,
            type: 'get' ,
            success: function (data){
                 if (data.resultCode ==  "1" ) {
                    alert("success");
                }
            },
            error:  function (XMLHttpRequest, textStatus, errorThrown) {
                alert(XMLHttpRequest.status);
                alert(XMLHttpRequest.readyState);
                alert(textStatus);
            },
            complete:  function (XMLHttpRequest, textStatus) {
                 this ;  // 调用本次AJAX请求时传递的options参数
            }         });   

```

##根据ajax的结果来决定执行哪个方法
```
$.ajax({
    cache: false,
    async: false, // 太关键了同步和异步的参数
    dataType: 'json',
    type: 'post',
    url: "../handle/Ladder_Fee_Code.ashx?ajaxaction=Select_FangAn",
    success: function(data) {
        alert("1");
    }
});
alert("2");
备注：ajax请求，async参数默认为true,即ajax请求默认是异步的；当为false时，表示请求是同步。
```

## Ajax本地跨域问题 
```
Cross origin requests are only supported for HTTP
分析：浏览器为了安全性考虑，默认对跨域访问禁止。


解决：给浏览器传入启动参数（allow-file-access-from-files），允许跨域访问。Windows下，运行（CMD+R）或建立快捷方式：
"C:/Program Files (x86)/Google/Chrome/Application/chrome.exe" --allow-file-access-from-files
```

##关于JS的Ajax方法导致跨域问题的解决办法
```
摘要：跨域问题是web前端开发者经常碰到的

前面发布了一篇关于利用load()方法拼接页面的技术文章，有网友运行后发现在谷歌浏览器里无法正常显示，console.log()控制台报错：
 
 XMLHttpRequest cannot load file:///E:2014/DEMO/html_ajax/header.html. Cross origin requests are only supported for protocol schemes: http, data, chrome-extension, https, chrome-extension-resource. 
 
这是由于涉及到跨域问题！
由于该网友没有在服务器环境里运行含有ajax方法的页面，而是直接通过浏览器打开（类似file:///的访问形式，即file协议）
本地页面ajax()请求本地页面，须通过服务器环境运行，类似这样：
http://127.0.0.1:8888/2014/DEMO/html_ajax/index.html
 
注意：如果是在远程服务器里ajax()请求外域服务器里的页面，即使通过服务器环境运行也会报跨域的错误，此时需要通过JSONP的形式！


什么是JSONP？
JSONP(JSON with Padding)是JSON的一种“使用模式”，可用于解决主流浏览器的跨域数据访问的问题。
JSONP举例：
$.ajax({ 
    type : "get", 
    async:false, 
    url : "http://app.example.com/base/json.do?sid=1494&busiId=101", 
    dataType : "jsonp",//jsonp数据类型 
    jsonp: "jsonpCallback",//服务端用于接收callback调用的function名的参数 
    success : function(data){ 
        $("#myID").text("Result:"+data.result) 
    }, 
    error:function(){ 
        alert('fail'); 
    } 
}); 
```

##AJAX()方法中的async:false的含义
```
http://www.exp99.com/jswz/f2e/1415610158_116.html
ajax()方法中的async:false与async:true的用途


$.ajax({  
    type : "get",  
    async:false,  
    url : "请求的地址",  
    dataType : "jsonp",//jsonp数据类型  
    jsonp: "jsonpCallback",//服务端用于接收callback调用的function名的参数  
    success : function(data){  
        $("#myID").text("Result:"+data.result)  
    },  
    error:function(){  
        alert('数据请求失败！');  
    }  
}); 
async属性的用途：
如果async设置为：true，即async:true则不会等待ajax请求返回的结果，会直接执行ajax后面的语句。
如果async设置为：false，即async:false则必须等待ajax请求返回结果后，再执行ajax后面的语句。
经测试，火狐浏览器滚动条下拉到底部触发ajax方法时会出现闪屏问题，只需将async设置为true！
```
##ajax的beforeSend 提高用户体验
```
$.ajax请求中有一个beforeSend方法，用于在向服务器发送请求前执行一些动作。
实例一：防止重复数据
// 提交表单数据到后台处理
$.ajax({
    type: "post",
    data: studentInfo,
    contentType: "application/json",
    url: "/Home/Submit",
    beforeSend: function () {
        // 禁用按钮防止重复提交
        $("#submit").attr({ disabled: "disabled" });
    },
    success: function (data) {
        if (data == "Success") {
            //清空输入框
            clearBox();
        }
    },
    complete: function () {
        $("#submit").removeAttr("disabled");
    },
    error: function (data) {
        console.info("error: " + data.responseText);
    }
});
```