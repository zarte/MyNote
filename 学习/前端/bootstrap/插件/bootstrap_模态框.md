[TOC]
##根据条件来判断是否展示modal
var errHtml = "<a href='#' onclick=showCountno('"+prplist[i]["id"]+"','"+prplist[i]["tempType"]+"','"+prplist[i]["countNo"]+"'); data-target='#myModal_count_no' class='btn' data-toggle='modal'>"+prplist[i]["countNo"]+"</a>";

<div id="myModal_count_no" class="modal fade" role="dialog" aria-hidden="true"></div>
1.a标签中 要去除 data-target='#myModal_count_no'。
function showCountno(id,tempType,no){
    if(no > 0){
        $("#myModal_count_no").modal('show');
    }else{
        $("#myModal_count_no").modal('hide');
    }
}

##modal框 与  点击 事件
```
bootstap modal框 点击 既可以显示modal框，又可以触发事件：
var import_btn = "<a href='#' onclick='javascript:toReImport(" + prplist[i]["id"] + ")' id='autocomplete' data-target='#myModal_autocomplete' role='button' class='btn purple' data-toggle='modal' >重新导入</a>";
function toReImport(id){
$("#fileIdHid").val(id);
console.log('fileId:'+id);
}
``` ##模态框（modal data-toggle data-target）
```
实例一(弹出框)
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<title>Bootstrap 实例 - 模态框（Modal）插件事件</title>
<link rel="stylesheet" href="http://cdn.static.runoob.com/libs/bootstrap/3.3.7/css/bootstrap.min.css">
<script src="http://cdn.static.runoob.com/libs/jquery/2.1.1/jquery.min.js"></script>
<script src="http://cdn.static.runoob.com/libs/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</head>
<body>


<h2>模态框（Modal）插件事件</h2>
<!-- 按钮触发模态框 -->
<button class="btn btn-primary btn-lg" data-toggle="modal" data-target="#myModal">
开始演示模态框
</button>
<!-- 模态框（Modal） -->
<div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
<div class="modal-dialog">
<div class="modal-content">
<div class="modal-header">
<button type="button" class="close" data-dismiss="modal" aria-hidden="true">×
</button>
<h4 class="modal-title" id="myModalLabel">
模态框（Modal）标题
</h4>
</div>
<div class="modal-body">
点击关闭按钮检查事件功能。
</div>
<div class="modal-footer">
<button type="button" class="btn btn-default" data-dismiss="modal">
关闭
</button>
<button type="button" class="btn btn-primary">
提交更改
</button>
</div>
</div><!-- /.modal-content -->
</div><!-- /.modal-dialog -->
</div><!-- /.modal -->
<script>
   $(function () { $('#myModal').modal('hide')})});
</script>
<script>
   $(function () { $('#myModal').on('hide.bs.modal', function () {
      alert('嘿，我听说您喜欢模态框...');})
   });
</script>


</body>
</html>


实例二（列表）
< div   class = "col-md-2" >
                                         < button   type = "button"   class = "btn green"   id = "query" > 查询 </ button >
                                         < a   href = "#"   data-target = "#myModal_autocomplete"   role = "button"   class = "btn red"   data-toggle = "modal" > 多价信息 </ a >                                      </ div >  
< div   id = "myModal_autocomplete"   class = "modal fade"   role = "dialog"   aria-hidden = "true" >
                             < div   class = "modal-dialog"   style =" width : 1200px " >
                                 < div   class = "modal-content" >
                                 < div   class = "modal-header" >
                                     < button   type = "button"   class = "close"   data-dismiss = "modal"   aria-hidden = "true" ></ button >
                                     < h4   class = "modal-title" > 已存在多价规则 </ h4 >
                                 </ div >
                                 < div   class = "modal-body form" >
                                     < div   class = "form-group" >
                                         < div   class = "portlet box red-intense" >
                                             < div   class = "portlet-title" >
                                             < div   class = "caption" >
                                                 < i   class = "fa fa-globe" ></ i > 商品信息： < label   class = "control-label"   id = "dialogskuname"   ></ label >
                                             </ div >
                                             < div   class = "tools" >
                                                 < a   href = "javascript:;"   class = "collapse" >
                                                 </ a >
                                             </ div >
                                         </ div >
                                         < div   class = "portlet-body" >
                                             < table   class = "table table-striped table-bordered table-hover"   id = "sample_10" >
                                                 < thead >
                                                     < tr >
                                                         < th >
                                                            价格类型
                                                         </ th >
                                                         < th >
                                                            身份类型
                                                         </ th >
                                                         < th >
                                                            时间段
                                                         </ th >
                                                         < th   class = "hidden-xs" >
                                                            站点信息
                                                         </ th >
                                                         < th   class = "hidden-xs" >
                                                            规则ID
                                                         </ th >
                                                         < th   class = "hidden-xs" >
                                                            规则名称
                                                         </ th >
                                                         < th   class = "hidden-xs" >
                                                            定价
                                                         </ th >
                                                         < th   class = "hidden-xs" >
                                                            限购描述
                                                         </ th >
                                                         < th   class = "hidden-xs" >
                                                            创建人
                                                         </ th >
                                                         < th   class = "hidden-xs" >
                                                            审核人
                                                         </ th >
                                                         < th   class = "hidden-xs" >
                                                            状态
                                                         </ th >
                                                        
                                                     </ tr >
                                                 </ thead >
                                                 < tbody >
                                                    
                                                 </ tbody >
                                             </ table >
                                         </ div >
                                     </ div >
                                 </ div >
                             </ div >
                             < div   class = "modal-footer"   style =" text-align : center ;" >
                                 < button   type = "button"   class = "btn btn-default"   data-dismiss = "modal" > Close </ button >
                             </ div >
                         </ div >
                     </ div >
                 </ div >  



```