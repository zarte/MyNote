##bindEvent()
```
<script src="${resRoot}/advertisement/adResource/adResource.js"></script>
<script type="text/javascript">
$(function(){
    var typeId = $('#typeId').val();
    if(typeId ==0)
    {
        typeId = "";
    }
    //查询分页
    initDataGrid(typeId);
    bindEvent();
    //跳转到添加广告资源的页面
    $("#add_btn").click(function(){
        window.location.href="${base}/adResource/toAddAdResource.do";        
    });
    
}); </script>  
```

 ## initDataGrid(typeId)

```
function  initDataGrid(typeId){
        console.info(typeId);
         var  urls =  "" ;
            if (typeId == "" )
        {
               urls =  "${base}/adResource/findAdResource.do" ;
               
        }
            else
           {
               urls =  "${base}/adResource/findAdResource.do?typeId=" +typeId;
           }
        var  datagrid = $( "#dataGrid" ).datagrid({
               url:urls,
            noData:  '<table class="table table-bordered table-hover table-striped"><thead><tr><th>广告类型</th><th>广告资源名称</th><th>图片</th><th>跳转类型</th><th>创建人</th><th>创建日期</th><th>开始时间</th><th>结束时间</th><th>投放状态</th><th>操作</th></tr></thead></table>' ,
            col: [
              {
                      field:  "name" ,
                      title:  "广告类型"
                },{
                      field:  "adName" ,
                      title:  "广告资源名称"
                },{
                    field:  "orderNumber" ,
                    title:  "显示顺序"
                },{
                field:  "adImageUrl" ,
                title:  "图片" ,
                render:  function ( data ) {
                     return   "<img style='width:100px;height:100px;' src='"  + data.value +  "'</img>" ;
                }
            },{
                field:  "adType" ,
                title:  "跳转类型" ,
                render:  function (data){
                     if (0 == data.value){
                         return   "图片无跳转" ;
                    } else   if (1 == data.value){
                         return   "图片链接跳转" ;
                    } else   if (2 == data.value){
                         return   "功能模块" ;
                    } else   if (3 == data.value){
                         return   "文本展示" ;
                    } else {
                         return   "未知" ;
                    }
                }
            },{
                field:  "createUserId" ,
                title:  "创建人"
               
            },{
                field:  "createTime" ,
                title:  "创建时间"
            },{
                field:  "startTime" ,
                title:  "开始时间"
            },{
                field:  "endTime" ,
                title:  "结束时间"
            },{
                field:  "fstatus" ,
                title:  "投放状态" ,
                render:  function (data){
                     if (0 == data.value){
                         return   "未开始" ;
                    } else   if (1 == data.value){
                         return   "投放中" ;
                    } else   if (2 == data.value){
                         return   "已结束" ;
                    } else {
                         return   "未知" ;
                    }
                }
            },{
                field:  "id" ,
                title:  "操作" ,
                render:  function ( data ) {
                     return   "<a href='${base}/adResource/toModifyAdResourceById.do?id=" +data.row.id+ "'>编辑</a>|" +
                     "<a href='javascript:void(0);' onclick='delete_btn(" +data.row.id+ ")'>删除</a>|" +
                     "<a href='javascript:void(0);' onclick='find_btn(" +data.row.id+ ")'>查看</a>" ;
                }
            }],
            attr: {  "class" :  "table table-bordered table-hover table-striped"  },
            sorter:  "bootstrap" ,
            pager:  "bootstrap" ,
            paramsDefault: {paging:10},
            onBefore:  function () {
                
            },
            onData:  function ( data ) {
                
            },
            onRowData:  function ( data, num, $tr ) {
                
            },
            onComplete:  function () {
                
            }
        }) }  
```
 
