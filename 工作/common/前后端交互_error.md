##net::ERR_INCOMPLETE_CHUNKED_ENCODING
```
跳转到修改页面的时候报错了，原因有可能是程序代码 中报错了，比如：
List<TemplateResourceDto>  tempResList  =  msgTemplateDto .getTempResList();
             if  ( tempResList  !=  null  &&  tempResList .size() > 0) {
                 modelMap .put( "tempResList" ,  tempResList );             }  
页面中判断list是否为有误，导致报错的。不应该写<#if tempResList != null> ,因为freemaker中 只支持基本类型。
正确的写法：

<#if (tempResList?size>
    0) >
    <#list tempResList as res>
        <tr class='list'>
            <td>
                <div class="sou-pic">
                    <img src="${res.spic}" alt="">
                    <input type='hidden' value="${res.rid}" />
                    <input type='hidden' value='${res.remark}' />
                </div>
            </td>
            <td>
                ${res.title}
            </td>
            <td>
                ${res.sort}
            </td>
            <td>
                <a id='b${res.sort}' href='javascript:$.keyword.edittuwen("b${res.sort}")'
                class='btn btn-xs blue' title='编辑'>
                    <i class='fa fa-pencil'>
                    </i>
                </a>
                <a id='a${res.sort}' href='javascript:$.keyword.delImg("a${res.sort}")'
                class='btn btn-xs red btn-delete' title='删除'>
                    <i class='fa fa-times'>
                    </i>
                </a>
            </td>
        </tr>
    </#list>
</#if>
 

```
##页面406，POST http://192.168.1.173:8080/mmc-web/message/addMsg.do?paramJson=%257B%2522tit…2:%257B%2522contentType%2522:1,%2522msgContent%2522:%2522bb%2522%257D%257D 406 (Not Acceptable)
```
后台Action可以正常返回数据，但页面406
@Controller
@RequestMapping (value =  "message" , produces =  "text/html;charset=UTF-8" ) public   class  MessageAction  extends  MmcAction {   

@ResponseBody
     @RequestMapping (value =  "addMsg" , method = RequestMethod. POST )
     public  BaseResult addMsg(HttpServletRequest  request ,  @RequestParam  String  paramJson ) {
         log .info( "====addMsg====" );
        CompanyInfoDto  companyInfoDto  =  this .getMerchantInfo( request );
        String  suppId  =  companyInfoDto .getCompanyId().toString();
        BaseResult  br ;
         try  {
             paramJson  = java.net.URLDecoder. decode ( paramJson ,  "utf-8" );
            PushTaskDto  task  = JSON. parse ( paramJson , PushTaskDto. class );
             br  =  msgService .addMsg( task );
        }  catch  (Exception  e ) {
             log .error( "addMsg error:"  +  e .getMessage());
             br  =  new  BaseResult(BaseResult. ERROR , MmcErrorCode. ERROR_MSG_UNKNOW );
        }
         return   br ;     }   

}


原因：处理方法注解没有加全：
返回的是 json 但是现在制定是 text / plain ; charset = UTF - 8 ，文本
@RequestMapping ( value = "acct/save" , method = RequestMethod . POST , produces = "application/json" )
```
##页面404，后台action报错， org.springframework.beans.InvalidPropertyException : Invalid property 'msgTemplateDto[contentType]' of bean class [com.bn.b2b.wxChat.pushtask.dto.PushTaskDto]: Property referenced in indexed property path 'msgTemplateDto[contentType]' is neither an array nor a List nor a Map; returned value was [1]
```


1.js:
function  saveMsg(){
     var  msg_form = $( '#addMsgForm' );
     var  parm = $.serializeObject(msg_form);
     var  suppId = $( "#suppId" ).val();
    parm.suppId = suppId;
     if (parm.pushmode ==  "1" ){
        parm.pushtime =  "" ;
    }
     if (parm.msgType==1){
         var  msgContent = $( "#msgContent" ).val();
         var  msgTem =  new  Object();
        msgTem.contentType = 1;
        msgTem.msgContent = msgContent;
    }
    parm.msgTemplateDto = msgTem;
    
     //var paramJson = encodeURI(encodeURI(JSON.stringify(param)));
    
     var  url = basePath +  '/message/addMsg.do' ;
    $.ajax({
        type :  "post" ,
        url : url,
        data : parm,
        dataType :  "json" ,
        success :  function (data) {
             if  (data.status ==  "1" ) {
                alert(data.message);
            } else   if  (data.status ==  "0" ) {
                alert(data.message);
            }
        }
    }); }  
 

2-1.action中的dto:
public   class  PushTaskDto  implements  Serializable{  
private  String  title ;
private  MsgTemplateDto  msgTemplateDto ;
}
public   class  MsgTemplateDto  implements  Serializable{
//消息类型 1: 关注回复 2:自动回复 3:被动消息 4:推送消息   

private  Integer  msgType ;
/ /消息内容类型    1:文本 2:资源 
private  Integer  contentType ;

/ /文本消息内容
  private  String  msgContent ;

//  资源集合 当contentType为2时才有数据 private  List<TemplateResourceDto>  tempResList ;   

}
2-2:action:
@Controller
@RequestMapping (value =  "message" , produces =  "text/html;charset=UTF-8" ) public   class  MessageAction  extends  MmcAction {  
@ResponseBody
     @RequestMapping (value =  "addMsg" , method = RequestMethod. POST )
     public  BaseResult addMsg(HttpServletRequest  request , PushTaskDto  msg ) {
         log .info( "====addMsg====msg:"  +  msg .toString());
        CompanyInfoDto  companyInfoDto  =  this .getMerchantInfo( request );
        String  suppId  =  companyInfoDto .getCompanyId().toString();
        BaseResult  br ;
         try  {
             br  =  msgService .addMsg( msg );
        }  catch  (Exception  e ) {
             log .error( "addMsg error:"  +  e .getMessage());
             br  =  new  BaseResult(BaseResult. ERROR , MmcErrorCode. ERROR_MSG_UNKNOW );
        }
         return   br ;
    }
} 

```
##页面上的值，action接收不到
```
原因：有可能是Action中少了注解：
@RequestMapping (value =  "message" , produces =  "text/html;charset=UTF-8" )  


正确方式：
function insert (){
var name = $ ( "#activityname" ). val (). trim ();
var img = $ ( "#activityPhoto" ). val ();
 
var parm = new Object ();
parm . name = name ;
parm . img = img ;
parm . discountType = selectvalue ;
 
$ . ajax ({
url : '${base}/xs/add.do' , // 跳转到 action
data : parm ,
type : 'post' ,
cache : false ,
dataType : 'json' ,
success : function ( data ) {
if ( data . status == "1" ){
layer . msg ( '保存成功！请继续维护活动商品...' , { time : 1500 });
} else {
layer . msg ( '新建失败，请重试' , { time : 1500 });
}
},
error : function () {
alert ( "未知异常！" );
}
});
}



@Controller
@RequestMapping (value =  "xs" , produces =  "text/html;charset=UTF-8" ) public   class  DiscountAction  extends  MmcAction {   



@RequestMapping(value =  "add" , method = RequestMethod.POST)
public  String add(HttpServletRequest request, HttpServletResponse response, XSActivityDto activity) {
    CompanyInfoDto companyInfoDto =  this .getMerchantInfo(request);
    String suppId = companyInfoDto.getCompanyId().toString();
    activity.setSuppId(suppId);
    BaseResult br = disCountService.addActivity2(activity);
     return  ajaxJson(response, JSONObject.fromObject(br).toString());
}


}
 

```