##多价管理（sales.jsp） 、促销管理(advanced.jsp) 、单品赠品管理()页面
中的 【操作】之审核通过、审核拒绝 由之前的列表放到详情页面中。



```
function  SetPromotionStatus(priceid,status,ifpass,skuid) {     }
priceid =  23118 ,  status =  2 ,  ifpass =  1 ,  skuid =  9990094
 

```
```
<!-- modal footer -->
                        <div class="modal-footer" style="text-align:center;">
                            <input type="hidden" id="priceId" />
                            <input type="hidden" id="skuId" />
                            <button type='button' class='btn btn-default' data-dismiss='modal' id="verify_yes" style="display:none" onclick="SetPromotionStatus('priceId',2,1,'skuId')">审核通过</button>
                            <button type='button' class='btn btn-default' data-dismiss='modal' id="verify_no" style="display:none" onclick="SetPromotionStatus('priceId',2,2,'skuId')">审核拒绝</button>
                            <button type='button' class='btn btn-default' data-dismiss='modal'>Close</button>                         </div>   

```


查询列表方法 getPromotionList() 去掉之前的审核 通过与审核拒绝 。


促销管理 (advanced.jsp)：
showPromotion();


```
<div class="modal-footer" style="text-align:center;">
                < input type="hidden" id="ruleId" />
                < button type= 'button' class='btn btn-default' data-dismiss='modal' id="verify_yes" style="display:none" onclick= " SetPromotionStatus('ruleId',2,1)">审核通过</ button >
                <button type='button' class='btn btn-default' data-dismiss='modal' id= " verify_no"   style="display:none" onclick="SetPromotionStatus('ruleId',2,2)">审核拒绝</button>
                <button type='button'  class= ' btn btn-default '  data-dismiss= ' modal ' >Close</button>             </div>   

```


##批量审核权限开关
```
除了详情中的审核权限 ，之前的列表页面也要有。所以要设置一个开关。
value = 1; 列表页面有
value = 2; 详情页面有




```
###power.properties
```
im_sales_conf= #im_sales_conf   

```
##设置列表页面中的审核按钮
```
power.properties:
im_sales_conf_list= #im_sales_conf_list  
1.多价列表sales.jsp
<button type='button' class='btn default'  " + conf + " onclick='SetPromotionStatus("+prplist[i]["priceId"]+",2,1,"+prplist[i]["itemSkuId"]+")'>审核通过  </button> 

<button type='button' " + conf + " class='btn default' onclick='SetPromotionStatus("+prplist[i]["priceId"]+",2,2,"+prplist[i]["itemSkuId"]+")'>审核拒绝</button>
 

 2.促销管理 advanced.jsp

getPromotionList  : 

<button type='button' class='btn default' onclick='SetPromotionStatus("+prplist[i]["ruleId"]+", 2,1)'>审核通过</button><button type='button' class='btn default' onclick='SetPromotionStatus("+prplist[i]["ruleId"]+",2,2)'>审核拒绝</button>   



3.单品赠品管理 pmSingle.jsp
3-1之前的审核
var  auditHtml  = "<button  type='button' class =' btn default '  onclick=toAudit('" + prplist[i]["ruleId"] + "','" + prplist[i]["skuid"] + "')>审核</button>";  


function toAudit(ruleId,skuid) 
{
    $("#auditState").val(1);
    $("#auditRuleId").val(ruleId);
    $("#auditSkuid").val(skuid);
    $('#autocomplete').click();
}


<a href="#" id="autocomplete" data-target="#myModal_autocomplete" role="button" class="btn red" data-toggle="modal" style="display:none">审核</a>


<div id="myModal_autocomplete" class="modal fade" role="dialog" aria-hidden="true">
                    <div class="modal-dialog " style="width:360px">
                        <div class="modal-content">
                            <div class="modal-header">
                                <button type="button" class="close" data-dismiss="modal" aria-hidden="true"></button>
                                <h4 class="modal-title">审核</h4>
                            </div>
                            <div class="modal-body form">
                                    <div class="row">
                                        <div class="col-md-9">
                                            <div class="form-group">
                                                <label class="control-label col-md-4"><br/>结果：</label>
                                                <div class="col-md-7"><br/>
                                                    <select class="form-control" name="auditState" id="auditState">
                                                        <option value="1" selected="true">通过</option>
                                                        <option value="2">不通过</option>
                                                    </select><br/>
                                                    <input type="hidden" id="auditRuleId"/>
                                                    <input type="hidden" id="auditSkuid"/>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                            </div>
                            <div class="modal-footer">
                                <button type="button" class="btn btn-default" data-dismiss="modal" id="confirm">确认</button>
                            </div>
                        </div>
                    </div>
                </div>  



3-2：现在的审核
getPromotionList  :
//审核权限
                 var  check_btn =  "" ;
                 var  conf = $( "#im_sales_conf_list" ).val();


if (conf ==  "1" ){
                        check_btn =  "<button type='button' class='btn default' onclick='SetPromotionStatus(" +prplist[i][ "ruleId" ]+ ","  + prplist[i][ "skuid" ] +  ",1,1)'>审核通过</button><button type='button' class='btn default' onclick='SetPromotionStatus(" +prplist[i][ "ruleId" ]+ ","  + prplist[i][ "skuid" ] +  ",2,2)'>审核拒绝</button>" ;
                    }


function  SetPromotionStatus(ruleId,skuId,state,ifpass){
     var  txtflag =  "" ;
     if (ifpass ==  "1" ){
        txtflag =  "审核通过" ;
    } else {
        txtflag =  "拒绝" ;
    }    
    
     if (window.confirm( '你确定要' +txtflag+ '吗？' )){
         //var siteUserName = getCookie("siteUserName");
         //var userName = window.Base64.decode(siteUserName);
        
         var  userName =  "Frank" ;
        
        $.ajax({
            url: '${ctx}/cust/pmSingleApproval.do' ,
            data:{ "skuid" :skuId, "ruleId" :ruleId, "state" :state, "userName" :encodeURI(userName)},
            dataType: 'json' ,
            cache: false ,
            type: 'get' ,
            success: function (data){
                alert(data.msg);
                $( "#query" ).click();
            }
        });
    } else {
         return ;
    }
}
 

4.配置 im_sales_conf_list.
```



