##bootstrap-table方法之：showColumn/hideCoulumn显示或隐藏指定数据列
```
官方演示地址：http://issues.wenzhixin.net.cn/bootstrap-table/methods/showColumn-hideCoulumn.html


Show/Hide the specified column.
$table.bootstrapTable('showColumn', 'name');
$table.bootstrapTable('hideColumn', 'name');
```
##完善类似第二页查不到第一页数据的情况
```
简略版： //完善类似第二页查不到第一页数据的情况     $( '#tableMetaclassId' ).bootstrapTable( 'refresh' ,{url:basePath+ '/metaClassController/getMetaclassList.do' });
详细版：

<div class="form-group" style="text-align:right;float:right;">
                                <button class="btn btn-primary wn120 f_l btn-search" > <i class="fa fa-search"></i> 查询
                                </button>
                                <button class="btn btn-warning wn120 f_l btn-reset"> <i class="fa fa-undo"></i> 清除条件
                                </button>                             </div>  


<script type="text/javascript">
    jQuery(document).ready(function() {
        queryInternetPaymentList();
        $(".btn-reset").on("click",function(){
            $(".check-text").val(null)
        });
        $('*[maxlength]').maxlength({
            alwaysShow: true,
            limitReachedClass: "label label-danger",
        });
        //完善类似第二页查不到第一页数据的情况
        $(".btn-search").click(function(){
            var queryParams = {url: "${base}"+"/interPayment/findInterPages.do"};
            $('#internetPayment_list').bootstrapTable('refresh',queryParams);
        });
    });
    
     var queryParams = function (params){
        var temp = {
            limit: params.limit,  //页面大小
            offset: params.offset, //页码
            merCode: $("#merCode").val(),
            serviceCode: $("#serviceCode").val(),
            deptCode: $("#deptCode").val(),
            deptName: $("#deptName").val(),
            occDate: $("#occDate").val(),
            upTime: $("#upTime").val()
        };
        return temp;
    };
    function queryInternetPaymentList() {
        $('#internetPayment_list').bootstrapTable({
            method: 'get',
            url: "${base}"+"/interPayment/findInterPages.do",
            queryParams: queryParams,//传递参数
            striped: true,      //是否显示行间隔色
            cache: false,      //是否使用缓存，默认为true
            pagination: true,     //是否显示分页
            sortable: false, //是否排序
            sidePagination: "server",//服务端分页
            pageNumber:1,      //初始化加载第一页，默认第一页
            pageSize: 5,      //每页的记录行数（*）
            pageList: [5, 10, 20],  //可供选择的每页的行数（*）
            columns: [{
                field: 'Number',
                title: '序号',
                align: 'center',
                valign: 'middle',
                formatter: function (value, row, index) {
                    var num = index+1;
                    if(num<10){
                        return "0"+num;
                    }
                    return num;
                }
            }, {
                field: 'merCode',
                title: '商户编码',
                align: 'center',
                valign: 'middle'
            }, {
                field: 'serviceCode',
                title: '服务编码',
                align: 'center',
                valign: 'middle'
            }, {
                field: 'serviceName',
                title: '服务名称',
                align: 'center',
                valign: 'middle'
            }, {
                field: 'payBusNo',
                title: '支付商户号',
                align: 'center',
                valign: 'middle'
            }, {
                field: 'deptCode',
                title: '部门编码',
                align: 'center',
                valign: 'middle'
            }, {
                field: 'deptName',
                title: '部门名称',
                align: 'center',
                valign: 'middle'
            },{
                field: 'occDate',
                title: '发生日期',
                align: 'center',
                valign: 'middle'
            },{
                field: 'payNumer',
                title: '支付笔数',
                align: 'center',
                valign: 'middle'
            },{
                field: 'payAmount',
                title: '支付金额',
                align: 'center',
                valign: 'middle',
                formatter: function (value, row, index) {
                    var result = value.toFixed(2);
                    return result;
                }
            },{
                field: 'preferAmount',
                title: '优惠金额',
                align: 'center',
                valign: 'middle',
                formatter: function (value, row, index) {
                    var result = value.toFixed(2);
                    return result;
                }
            },{
                field: 'upTime',
                title: '上传时间',
                align: 'center',
                valign: 'middle'
            }]                
           });  
    }
    </script>  

```