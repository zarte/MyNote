##百度地图api
```
1.页面：
< div   class = "form-group" >
                                                 < label   class = "control-label col-md-3" > 坐标(经度) </ label >
                                                 < div   class = "col-md-4" >
                                                     < div   class = "input-group" >
                                                         < input   type = "text"   id   =   "lon"    name = "lon"   class = "form-control"   disabled />
                                                         < span   class = "input-group-btn" >
                                                         < button   class = "btn default"   type = "button"   onclick = "toMap();" >< i   class = "fa fa-map-marker" ></ i ></ button >
                                                     </ span >
                                                     </ div >
                                                     < #include "pages/common/map.ftl" />
                                                     < span   class = "help-block" ></ span >
                                                 </ div >
                                             </ div >
                                             < div   class = "form-group" >
                                                 < label   class = "control-label col-md-3" > 坐标(纬度) </ label >
                                                 < div   class = "col-md-4" >
                                                     < div   class = "input-group" >
                                                         < input   type = "text"   id   =   "lat"    name = "lat"   class = "form-control"   disabled />
                                                         < span   class = "input-group-btn" >
                                                         < button   class = "btn default"   onclick = "toMap();"   type = "button" >< i   class = "fa fa-map-marker" ></ i ></ button >
                                                     </ span >
                                                     </ div >
                                                     < span   class = "help-block" ></ span >
                                                 </ div >                                              </ div >   

< #include "pages/common/map.ftl" />   



<!-- 地图-->
     < script   type = "text/javascript"   src = "http://api.map.baidu.com/api?v=2.0&ak=14UYddc7fV4mhcIOVO5eYrfV"   ></ script >      < script   type = "text/javascript"   src = " ${base} /RES/pages/common/map.js" ></ script >  
 

map.ftl:
< div   id = "mapMT"   class = "modal fade"   keyboard = "false"   data-keyboard = "false"   data-backdrop = "static"   tabindex = "-1"   data-width = "300" >
         < div   class = "modal-dialog" >
             < div   class = "modal-content" >
                 < div   class = "modal-header" >
                     < button   type = "button"   class = "close"   id = "clear"   onclick = "closed();"   data-dismiss = "modal"   aria-hidden = "true" ></ button >
                     < h4   class = "modal-title" > 浏览器定位 </ h4 >
                 </ div >
                 < div   class = "modal-body" >
                     <!-- <title>浏览器定位</title> -->
                     < div   id = "allmap"   style =" width :  400px ;  height :  300px " ></ div >
                     < div   id = "r-result"   style =" padding-bottom :  10px ; width :  250px "  >
                        请输入关键字: < input   type = "text"   id = "suggestId"   size = "20"   value = ""    style =" width :  200px ;"  />
                     </ div >
                     < div   id = "searchResultPanel" style =" border :  1px solid #C0C0C0 ;  width :  150px ;  height :  auto ;  display :  none ;" ></ div >
                     < div >
                         < input   type = "hidden"   id = "loc" >
                     </ div >
                 </ div >
             </ div >
         </ div >      </ div >  


map.js:
var  basePath =  '${base}' ;
function  toMap(){
    $( '#mapMT' ).modal( 'show' );
}
function  closed(){
    $( '#suggestId' ).val( "" );
    $( '#mapMT' ).modal( 'hide' )
}
//百度地图API功能
var  map =  new  BMap.Map( "allmap" );
var  point =  new  BMap.Point(116.331398, 39.897445);
map.addControl( new  BMap.NavigationControl());
map.addControl( new  BMap.ScaleControl());
map.addControl( new  BMap.OverviewMapControl());
map.addControl( new  BMap.MapTypeControl());
map.centerAndZoom(point, 12);
map.enableScrollWheelZoom( true );
map.addEventListener( "click" ,
         function (e) {
             //alert(e.point.lng + ", " + e.point.lat);  
            document.getElementById( "loc" ).value = e.point.lng +  ", " + e.point.lat;
            $( "#lon" ).val(e.point.lng);
            $( "#lat" ).val( e.point.lat);
        });
function  myFun(result) {
     var  cityName = result.name;
    map.setCenter(cityName);
     //alert("当前定位城市:"+cityName);
}
var  myCity =  new  BMap.LocalCity();
myCity.get(myFun);
// 百度地图API功能
function  G(id) {
     return  document.getElementById(id);
}
var  ac =  new  BMap.Autocomplete(  //建立一个自动完成的对象
{
     "input"  :  "suggestId" ,
     "location"  : map
});
ac.addEventListener( "onhighlight" ,  function (e) {  //鼠标放在下拉列表上的事件
     var  str =  "" ;
     var  _value = e.fromitem.value;
     var  value =  "" ;
     if  (e.fromitem.index > -1) {
        value = _value.province + _value.city + _value.district
                + _value.street + _value.business;
    }
    str =  "FromItem<br />index = "  + e.fromitem.index +  "<br />value = "
            + value;
    value =  "" ;
     if  (e.toitem.index > -1) {
        _value = e.toitem.value;
        value = _value.province + _value.city + _value.district
                + _value.street + _value.business;
    }
    str +=  "<br />ToItem<br />index = "  + e.toitem.index +  "<br />value = "
            + value;
    G( "searchResultPanel" ).innerHTML = str;
});
var  myValue;
ac.addEventListener( "onconfirm" ,  function (e) {  //鼠标点击下拉列表后的事件
     var  _value = e.item.value;
    myValue = _value.province + _value.city + _value.district
            + _value.street + _value.business;
    G( "searchResultPanel" ).innerHTML =  "onconfirm<br />index = "
            + e.item.index +  "<br />myValue = "  + myValue;
    setPlace();
});
function  setPlace() {
    map.clearOverlays();  //清除地图上所有覆盖物
     function  myFun() {
         var  pp = local.getResults().getPoi(0).point;  //获取第一个智能搜索的结果
        map.centerAndZoom(pp, 18);
        map.addOverlay( new  BMap.Marker(pp));  //添加标注
    }
     var  local =  new  BMap.LocalSearch(map, {  //智能搜索
        onSearchComplete : myFun
    });
    local.search(myValue);
}
//用经纬度设置地图中心点
function  theLocation(){
     var  lon = $( "#lon" ).val();
     var  lat = $( "#lat" ).val();
     if (lon !=  ""  && lat !=  "" ){
        map.clearOverlays(); 
         var  new_point =  new  BMap.Point(lon,lat);
         var  marker =  new  BMap.Marker(new_point);   // 创建标注
        map.addOverlay(marker);               // 将标注添加到地图中
        map.panTo(new_point);      
    }
}
//延迟2秒钟后执行-只执行1次
var  t1 = window.setTimeout(theLocation,2000); 
//重复执行
//var t2 = window.setInterval(hello,1000);  
 
```
##导入数据
```
1. < a   href = "javascript:toupload(1,'/userController/userListPage.do');"   id = "up"   class = "btn btn-default btn-sm" >< i   class = "fa fa-pencil" ></ i > 导入员工 </ a >
< script   src = " ${resRoot} /pages/common/upload.js"   type = "text/javascript" ></ script > < script   src = " ${resRoot} /pages/common/jquery-form.js"   type = "text/javascript" ></ script >   

< #include "pages/common/upload.ftl" />  


页面upload.ftl:
< div   id = "stack1"   class = "modal fade"   tabindex = "-1"   data-width = "400"    data-backdrop = "static" >
         < div   class = "modal-dialog" >
             < div   class = "modal-content" >
                 < div   class = "modal-header" >
                     < button   type = "button"   class = "close"   data-dismiss = "modal"   aria-hidden = "true" ></ button >
                     < h4   class = "modal-title" > 上传文件 </ h4 >
                 </ div >
                 < div   class = "modal-body" >
                     < div   class = "row" >
                         < div   class = "col-md-12" >
                             < form   id = "fileupload"   name = "fileuploadForm"    action = ""    method = "post"   enctype = "multipart/form-data" >
                                 < h4 > 请选择要上传的文件 </ h4 >
                                     < div   class = "form-group" >
                                             < input   type = "file"     name = "file"   id = "file"   >
                                             < input   type = "hidden"    name = "param"   id = "param"   value = "1" >
                                             < input   type = "hidden"    name = "url"   id = "url"   value = "/deptController/deptListPage.do" >
                                     </ div >
                             </ form >
                         < a   class = "btn green"   data-toggle = "modal"    href = "#stack2"    > 错误提示 </ a >
                         </ div >
                     </ div >
                 </ div >
                 < div   class = "modal-footer" >
                
                     < button   type = "button"    data-dismiss = "modal"   class = "btn"   id = "close" > 关闭 </ button >
                     < button   type = "button"   class = "btn blue"   onclick = "upload()"   id = "upd" > 确认导入 </ button >
                 </ div >
             </ div >
         </ div >
     </ div >
     < div   id = "stack2"   class = "modal fade"   tabindex = "-1" >
         < div   class = "modal-dialog" >
             < div   class = "modal-content" >
                 < div   class = "modal-header" >
                     < button   type = "button"   class = "close"   data-dismiss = "modal"   aria-hidden = "true" ></ button >
                     < h4   class = "modal-title" > 错误提示 </ h4 >
                 </ div >
                 < div   class = "modal-body" >
                     < div   class = "row" >
                         < div   class = "col-md-12"   id = "err" >
                             < p > 无错误信息  </ p >
                         </ div >
                     </ div >
                 </ div >
                 < div   class = "modal-footer" >
                     < button   type = "button"   data-dismiss = "modal"   class = "btn" > 关闭 </ button >
                     < button   type = "button"   class = "btn yellow"   data-dismiss = "modal" > 确认 </ button >
                 </ div >
             </ div >
         </ div >
     </ div >


upload.js:
var  basePath =  '${base}' ;
function  todownload(param)
{
     var  fileName;
     if (param== '1' )
    {
        fileName= "员工导入模板.xls" ;
    }
     else   if (param== '2' )
    {
        fileName= "部门导入模板.xls" ;
    }
     var  getTimestamp =  new  Date().getTime();
    window.location = basePath+ '/excel/download.do?fileName=' +encodeURI(encodeURI(fileName))+ "×tamp=" +getTimestamp;
}
/**
  *   弹出导入窗口
  *   param   1 - 员工导入
                2 -   部门导入
  */  
function  toupload(param,url)
{
     //需要重置form表单
    document.getElementById( "fileupload" ).reset();
    $( "#param" ).val(param);
    $( "#url" ).val(url);
     var  file = $( "#file" )
    file.after(file.clone().val( "" ));
    file.remove();
    
    $( "#err" ).html( "<div class='form-group'><p>无错误信息 </p></div>" );
    $( '#stack1' ).modal( 'show' );
}
//导入模板
function  upload(){
     var  file = $( "#file" ).val();
     if (file== null ||file== '' )
    {
        alert( "请先选择要上传的文件" );
         return ;
    }
     //按钮不可用
     //$("#upd").attr('disabled',true);//设置disabled属性为true，按钮不可用 
     //$("#upd").html("正在导入，请稍等...");
         var  getTimestamp =  new  Date().getTime();
        $( "#fileupload" ).ajaxSubmit({
            type: 'post' ,
            dataType: "json" ,
            url:basePath +  '/excel/upload.do?timestamp=' +getTimestamp,
            success: function (result){
                 if (result.status ==  "1" ){
                    alert( "导入成功！" );
                    $( "#upd" ).attr( 'disabled' , false ); //设置disabled属性为false，按钮可用
                     //关闭窗口
                    $( "#close" ).click();
                    
                     //需要刷新列表页面
                    location.href = basePath + $( "#url" ).val();
                } else   if (result.status ==  "0" ){
                    alert( "导入失败" );
                    $( "#upd" ).attr( 'disabled' , false ); //设置disabled属性为false，按钮可用
                     var  list = result.content;
                     var  trs =  "" ;
                     for ( var  i=0;i <list.length; i++){
                           trs +=  "<div class='form-group'><p>" +list[i]+ "</p></div>" ;
                    }
                    $( "#err" ).html(trs);
                    $( '#stack2' ).modal( 'show' );
                } else { //既有正确又有错误的
                    alert( "部分数据导入成功" );
                    $( "#upd" ).attr( 'disabled' , false ); //设置disabled属性为false，按钮可用
                     var  list = result.content;
                     var  trs =  "" ;
                     for ( var  i=0;i <list.length; i++){
                           trs +=  "<div class='form-group'><p>" +list[i]+ "</p></div>" ;
                    }
                    $( "#err" ).html(trs);
                    $( '#stack2' ).modal( 'show' );
                    
                }
            },
            error: function (XmlHttpRequest,textStatus,errorThrown){
                alert( "导入出错！" );
                $( "#upd" ).attr( 'disabled' , false ); //设置disabled属性为false，按钮可用
            }
        }); }  


2.java代码：
@Controller
@RequestMapping (value= "excel" ,produces= "text/html;charset=UTF-8" ) public   class  ExcelController  extends  BaseController {  
@Autowired
     private  ExcelService  excelService ;
    
     /**
     * 导入数据
     *  @param  file
     *  @param  request
     *  @param  response
     */
     @RequestMapping ( "upload" )
     public  String upload( @RequestParam (value =  "file" , required =  false )  MultipartFile  file , HttpServletRequest  request , HttpServletResponse  response ){
        String  param  = request .getParameter( "param" );
         log .info( "upload param=" + param + ",file=" + file .getOriginalFilename());
        BaseResult  result  =  new  BaseResult();
        String  filePath  =saveFile( file ,  request );
         if (StringUtils. isEmpty ( filePath ))  //保存文件失败
        {
             result .setStatus(BaseResult. ERROR );
             return  ajaxJson( response , JSONObject. fromObject ( result ).toString());
            
        }
        UserInfoDto  user  = getUserInfo( request );
         result  =  excelService .readXls( filePath ,  param ,  user );
         return  ajaxJson( response , JSONObject. fromObject ( result ).toString());
    }
    
    
     private  String saveFile(MultipartFile  file , HttpServletRequest  request )
    {
         // excel数据写入到数据库
        String  filePath  =  "" ;
         try  {
             request .setCharacterEncoding( "utf-8" );
             // 上传的服务器路径
            String  basePath  =  request .getSession().getServletContext().getRealPath( "/" ) +  "uploadFile"  + File. separator ;
            File  f  =  new  File( basePath );
             if (! f .exists())
            {
                 f .mkdir();
            }
             // 上传的excel文件名称【uuid保证上传的文件是唯一的】
            String  fileName  = UUID. randomUUID ().toString() +  "_"  +  file .getOriginalFilename();
             filePath  =  basePath  +  fileName ;
             // 检查服务器的文件上传路径是否存在，不存在就创建
            File  targetFile  =  new  File( filePath );
             // excel文件上传到应用服务器
             file .transferTo( targetFile );
        }  catch  (Exception  e ) {
             log .error( "upload file fail" , e );
             filePath  = null ;
        }
         return   filePath ;
    }
    
    
    
     /**
     * 下载模板
     *  @param  request
     *  @param  response
     */
     @RequestMapping ( "download" )
     public   void  download(HttpServletRequest  request , HttpServletResponse  response ){
        String  fileName  =  request .getParameter( "fileName" );
         try  {
             fileName  = URLDecoder. decode ( fileName , "UTF-8" );
        }  catch  (UnsupportedEncodingException  e ) {
        }
        String  basePath  =  request .getSession().getServletContext().getRealPath( "/" );   //项目物理路径
        String  tempPath  =  basePath  + "template" +File. separator + fileName  ;   //模板物理路径
         log .info( "download fileName = " + tempPath );
         //判断该模板是否存在，如果不存在则创建
        File  file  =  new  File( basePath  + "template" );
         file .mkdirs();
        downloadFile( response ,  tempPath );
    }
    
     /**
     * 下载文件
     *  @param  response
     *  @param  filepath
     */
     private   void  downloadFile(HttpServletResponse  response , String  filepath ) {
         try  {
             // path是指欲下载的文件的路径。
            File  file  =  new  File( filepath );
             // 取得文件名。
            String  filename  =  file .getName();
             // 取得文件的后缀名。
             //String ext = filename.substring(filename.lastIndexOf(".") + 1).toUpperCase();
             // 以流的形式下载文件。
            InputStream  fis  =  new  BufferedInputStream( new  FileInputStream( filepath ));
             byte []  buffer  =  new   byte [ fis .available()];
             fis .read( buffer );
             fis .close();
             // 清空response
             response .reset();
             // 设置response的Header
             response .setContentType( "application/vnd.ms-excel;charset=UTF-8" );
            String  downLoadName  =  new  String( filename .getBytes( "utf-8" ),  "iso8859-1" );
             response .addHeader( "Content-Disposition" ,  "attachment;filename="  +  downLoadName );
             response .addHeader( "Content-Length" ,  ""  +  file .length());
            OutputStream  toClient  =  new  BufferedOutputStream( response .getOutputStream());
             toClient .write( buffer );
             toClient .flush();
             toClient .close();
        }  catch  (IOException  e ) {
             log .error( "下载文件失败" , e );
        }
    }
}


@Service (value= "excelService" )
public   class  ExcelServiceImpl   implements  ExcelService{
     private   static   final  MyLogger  log  = MyLoggerFactory. getLogger (ExcelUtil. class );
    
     @Autowired
     private  UserIntfService  userIntfService ;
    
     @Autowired
     private  DeptInfService  deptInfService ;
    
     @Override
     public  BaseResult readXls(String  filePath , String  param , UserInfoDto  user ) {
        
        BaseResult  result  =  new  BaseResult();
         //从excel中读取类型
         try
        {
             int   cellNumber =0;   //根据类型设置模板列数
             if (MmcConstants. UPLOAD_TYPE_USER .equals( param ))
            {
                 cellNumber  = 7;
            }
             else   if (MmcConstants. UPLOAD_TYPE_STORE .equals( param ))
            {
                 cellNumber =9;
            }
            
            List<Map<Integer, Object>>  list  =  new  ExcelUtil().readObjFromXls( filePath , cellNumber );
             //根据param 来区分具体的业务保存方法
             if (MmcConstants. UPLOAD_TYPE_USER .equals( param ))
            {
                 result  = userIntfService .saveUserExcel( list , user );
            }
             else   if (MmcConstants. UPLOAD_TYPE_STORE .equals( param ))
            {
                 result  = deptInfService .saveDeptExcel( list , user );
            }
        }
         catch (BizzException  be )
        {
             log .error( "上传文件失败，param=" + param , be );
            List<String>  list  =  new  ArrayList<String>();
             list .add( be .getMessage());
             result .setContent( list );
             result .setStatus(BaseResult. ERROR );
        }
         catch  (Exception  e ) {
             log .error( "上传文件失败，param=" + param , e );
            List<String>  list  =  new  ArrayList<String>();
             list .add( e .getMessage());
             result .setContent( list );
             result .setStatus(BaseResult. ERROR );
        }
         return   result ;
    }
    
    
}  

```



##根据不同状态查询


点击不同的选项，进行改变状态的值
1.页面添加hidden隐藏域

```
< form   id = "changeTab_form" >
                        < input   id = "status"   type = "hidden"   class = "form-control"   name = "status"   size = "16"   value = "1" >                     </ form >   
```
2.Controller:


3.实体类


4.页面js



##将form表单封装到实体对象中
```
1.form表单
< form   id = "money_form"   class = "sou-pad2 sou-bor-1 sou-wid100 sou-hei129"   id = "form-1" >
                                         < p > 打款单号：  < span   class = "sou-bold" >
                                             < input   id = "payCode"   type = "text"   disabled = true   value = " ${soDetailOut.afterSaleOrderPay.payCode } " ></ span ></ p >
                                         < p   class = "" >< span   color:red ; > * </ span > 打款流水： < span   class = "sou-bold" >
                                         < #if soDetailOut.afterSaleOrderStatus == 1>
                                            ${soDetailOut.afterSaleOrderPay.payWaterno }
                                         </ #if>
                                          < #if soDetailOut.afterSaleOrderStatus == 0>
                                             < input   id = "payWaterno"   type = "text"   class = "sou-text"   value = " ${soDetailOut.afterSaleOrderPay.payWaterno } " >
                                         </ #if>
                                         </ span ></ p >
                                         < p > 打款日期： < span   class = "sou-bold" >
                                         < #if soDetailOut.afterSaleOrderStatus == 1>
                                             < #if soDetailOut.afterSaleOrderPay.payTime !="">
                                            ${soDetailOut.afterSaleOrderPay.payTime?datetime}
                                             </ #if>
                                         </ #if>
                                          < #if soDetailOut.afterSaleOrderStatus == 0>
                                              < #if soDetailOut.afterSaleOrderPay.payTime !="">
                                                  < input   id = "payTime"   type = "text"   class = "sou-text"   id = "sou-date"   value = " ${soDetailOut.afterSaleOrderPay.payTime} " >
                                              < #else>
                                                  < input   id = "payTime"   type = "text"   class = "sou-text"   id = "sou-date"   value = "" >
                                              </ #if>
                                         </ #if>
                                         </ span ></ p >
                                         < p > 打款类型： < span   class = "sou-bold" >
                                          < #if soDetailOut.afterSaleOrderStatus == 1>
                                                 < #if soDetailOut.afterSaleOrderPay.payBank == 0>支付宝 </ #if>
                                                 < #if soDetailOut.afterSaleOrderPay.payBank == 1>银联 </ #if>
                                                 < #if soDetailOut.afterSaleOrderPay.payBank == 2>微信 </ #if>
                                         </ #if>
                                          < #if soDetailOut.afterSaleOrderStatus == 0>
                                              < select   id = "payBank"   name = "payBank"   class = "select2me"   id = "dk-btn"   style =" width :  270px ; outline : none ;     padding :  6px 12px ; border : 1px solid #ddd ;"   data-placeholder = " " >
                                                     < option   value = "0" > 支付宝 </ option >
                                                     < option   value = "1" > 银联 </ option >
                                                     < option   value = "2" > 微信 </ option >
                                              </ select >
                                         </ #if>
                                             </ span >
                                         </ p >
                                         < p   class = "" >< span   color:red ; > * </ span > 打款机构： < span   class = "sou-bold" >
                                          < #if soDetailOut.afterSaleOrderStatus == 1>
                                            ${soDetailOut.afterSaleOrderPay.outAccount }
                                         </ #if>
                                          < #if soDetailOut.afterSaleOrderStatus == 0>
                                              < input   id = "payAccount"   type = "text"   class = "sou-text"   id = "dk-text"   value = " ${soDetailOut.afterSaleOrderPay.outAccount } " >
                                         </ #if>
                                         </ span ></ p >
                                         < p > 打款金额： < span   class = "sou-bold" >
                                          < #if soDetailOut.afterSaleOrderStatus == 1>
                                            ${soDetailOut.afterSaleOrderPay.payAmount }
                                         </ #if>
                                          < #if soDetailOut.afterSaleOrderStatus == 0>
                                              < input   id = "payAmount"   type = "text"   class = "sou-text"   value = " ${soDetailOut.afterSaleOrderPay.payAmount } " >
                                         </ #if>
                                         </ span ></ p >
                                         < p > 申请时间： < span   class = "sou-bold" >
                                          < #if soDetailOut.afterSaleOrderStatus == 1>
                                              < #if soDetailOut.afterSaleOrderPay.createTime!="">
                                            ${soDetailOut.afterSaleOrderPay.createTime?datetime}
                                             </ #if>
                                         </ #if>
                                          < #if soDetailOut.afterSaleOrderStatus == 0>
                                              < #if soDetailOut.afterSaleOrderPay.createTime!="">
                                                  < input   id = "approveCreateTime"   type = "text"   disabled = "true"   value = " ${soDetailOut.afterSaleOrderPay.createTime?datetime} " >
                                              < #else>
                                                  < input   id = "approveCreateTime"   type = "text"   disabled = "true"   value = "" >
                                              </ #if>
                                         </ #if>
                                         </ span ></ p >
                                         < p >< span   class = "sou-dakuan" > 打款备注： </ span >
                                         < span   class = "sou-bold"   style =" display :  table-cell ;" >
                                          < #if soDetailOut.afterSaleOrderStatus == 1>
                                             ${soDetailOut.afterSaleOrderPay.payRemark }
                                         </ #if>
                                          < #if soDetailOut.afterSaleOrderStatus == 0>
                                             < textarea   id = payRemark   name = "payRemark"   cols = "30"   class = "sou-text"   rows = "5" >
                                             </ textarea >
                                         </ #if>
                                         </ span ></ p >
                                         < input   type = "hidden"   id = "orderId"   name = "orderId"   value = " ${soDetailOut.afterSaleOrderId } "   />
                                         < input   type = "hidden"   id = "orderType"   name = "orderType"   value = " ${soDetailOut.afterSaleOrderPay.orderType } "   />
                                         < div   class = "clear" ></ div >
                                         < div   class = "sou-fabiao-ok"   style =" margin : 15px 60px ;" >
                                             < div   class = "sou-fabiao-sub2 sou-fabiao-sub-ok"   style =" border-right :  none ;" >
                                                 < a   id = "confirm_btn"    onclick = "javascript:confirmMoney(' ${soDetailOut.saleOrderOut.userId } ',' ${soDetailOut.afterSaleOrderId } ',' ${soDetailOut.afterSaleOrderCode } ');" > 确认 </ a >
                                             </ div >
                                             < div   class = "sou-fabiao-sub2 sou-fabiao-sub-bh" >
                                                 < a   href = " ${base} /payAfterSaleOrder/toList.do"   style =" color :  #fff ;" > 取消 </ a >
                                             </ div >
                                         </ div >                                      </ form >  
2.实体类
public   class  ComfirmOrderOut  implements  Serializable {
     /**
     * 
     */
     private   static   final   long   serialVersionUID  = 6746941920063925421L;
     /**
     * 订单ID
     */
     private  String  orderId ;
     /**
     * 订单编码
     */
     private  String  orderCode ;
     /**
     * 订单类型(1销售订单,2售后订单)
     */
     private  Integer  orderType ;
     /**
     * 物理主键
     */
     private  Long  payerId ;
     /**
     * 支付方类型,宝宝店,供应商,平台
     */
     private  Integer  payerType ;
     /**
     * 支付编码
     */
     private  String  payCode ;
     /**
     * 支付流水
     */
     private  String  payWaterno ;
     /**
     * 支付时间
     */
     private  String  payTime ;
     /**
     * 支付方式,线上支付,线下支付
     */
     private  Integer  payWay ;
     /**
     * 支付平台(支付宝,微信支付,银联支付)
     */
     private  Integer  payBank ;
     /**
     * 支付金额
     */
     private  BigDecimal  payAmount ;
     /**
     * 交易ID
     */
     private  String  tradeId ;
     /**
     * 支付备注
     */
     private  String  payRemark ;
     /**
     * 支出账号
     */
     private  String  outAccount ;
     /**
     * 进账账号
     */
     private  String  inAccount ;
     /**
     * 最后更新人
     */
     private  Long  lastOperator ;
         ...getter/setter...
3.Controller:
@RequestMapping (value =  "/confirmMoney" )
     public  String confirmMoney(HttpServletRequest  request , HttpServletResponse  response , ComfirmOrderOut  comfirmOrder ) {
         log .info( "确认打款web入参："  + JSON. toJSONString ( comfirmOrder ));
        PayAfterSaleOrderIn  orderPay  =  new  PayAfterSaleOrderIn();
         orderPay .setPayCode( comfirmOrder .getPayCode()); // 支付编码
         orderPay .setPayBank( comfirmOrder .getPayBank());  
```
##将对象转为字符串，测试参数时很方便
```
ComfirmOrderOut  comfirmOrder;  

import  com . alibaba . fastjson .JSON;
log .info( "确认打款web入参："  +  JSON. toJSONString ( comfirmOrder ));   

```
##根据时间字段查询
1.页面


2.Controller：通过类将日期参数带过来




3.对应的sql



##分页的实现
1.页面



当传递每页显示的记录数不同时，重新发出请求到服务器：
引入datagrid.js
```
< script   src = " ${resRoot} /common/jquery.datagrid-master/jquery.datagrid.js" ></ script > < script   src = " ${resRoot} /common/jquery.datagrid-master/plugins/jquery.datagrid.bootstrap3.js" ></ script >  
```
设置页面datagrid:
```
< div   id = "page-Tab"   class = ""   style =" width :  100% ;" >
                         < div    id = "dataGrid"   style =" overflow : auto " ></ div >
                         < div   class = "control-btn"   style =" width :  200px ; float : left ; position : relative ; top : -5px ;" >
                              < select   id = "selectpage"   class = "selectpage form-control select2me"   style =" width : 70px ; display :  inline-block ;"  required   data-placeholder = " " >
                                 < option   value = "5" > 5 </ option >
                                 < option   value = "10"   selected = "selected" > 10 </ option >
                                 < option   value = "15" > 15 </ option >
                             </ select >
                           < span   id = "totalnum" ></ span >
                          </ div >
                           < form   id = "pageSize_form" >
                              < input   id = "changePage"   name   = "paging"   type = "text" />
                          </ form >
                     </ div >  
```
引入js:
```
$( '#selectpage' ).on( 'change' , function (){
     var  changePage = $( "#selectpage" ).val();
    $( "#changePage" ).val(changePage);
    searchByPageNum();
});
function  searchByPageNum(){
     var  pageSize_form = $( '#pageSize_form' );
     var  parm = $.serializeObject(pageSize_form);
    
    $( "#dataGrid" ).datagrid(  "fetch" ,parm); }  
```
 


2.java代码










## sku 和 spu
sku :最小可销售单元
spu :商品
搜索：一般走sku，

内部查询：sku抛出来;
吐给第三方推广走spu.

一个spu对应多个sku，比如
spu                         sku
大衣                         红色大衣
大衣                         黄色大衣
大衣                         绿色大衣                                  


## 将页面原型页面发布到服务器上：


##关于redis与solor
商品的查询，帖子，标签，走搜索solor；
用户，分类，放在redis；
##branch与trunk
trunk做完一个版本后，拉到branch中，将branch中原来的老版本干掉
trunk相当于一个留存，线上的bug直接在trunk下改，改完后，更新到branch中。
##新增与修改合并为一个方法

```
public  Boolean saveSuppImg(SuppImgDto  suppImg ) {
         /**
         * 先查询判断是新增还是修改
         * 插入数据库
         */
         log .info( "SuppImgServiceImpl-->saveSuppImg-->SuppImgDto:" + suppImg .toString());
        SuppImgDto  tempDto  =  this .getSuppImg( suppImg .getSuppId(),  suppImg .getActivityType()+ "" );
         if ( tempDto == null ){
            SuppImgBo  imgIn  =  new  SuppImgBo( suppImg );
             try {
                 return   suppImgBaseService .addSuppImg( imgIn );
            } catch (Exception  e ){
                 log .error( "新增供应商图片失败" , e );
                 throw   new  SysException( "新增供应商图片失败" );
            }
        } else {
            SuppImgBo  imgIn  =  new  SuppImgBo( suppImg );
             try {
                 return   suppImgBaseService .modSuppImg( imgIn );
            } catch (Exception  e ){
                 log .error( "修改供应商图片失败" , e );
                 throw   new  SysException( "修改供应商图片失败" );
            }
        }     }  
``` 
