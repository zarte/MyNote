##注册新增加的接口
```
Plugin.java
// 多价导入记录Service
        ExtensionCtxHelper. addRuleExtension ( this , ISalesService. class .getName(), ISalesService. class ,                 SalesServiceImpl. class );   



否则，controller调用时空指针异常：svc为null
// 插入记录表
                ISalesService  svc  = RuleServiceFactory. getRuleService (ISalesService. class );
                 param .put( "fileName" ,  retLs .get(0).split( "," )[1]);
                 param .put( "status" ,  "1" );
                 param .put( "filePath" , PropertiesUtil. get ( "ftp.path" ) +  "/"  +  fileName );
                 // param.put("check_id", check_id);                  svc .insertSales( param );   



同样的,新增的mapper文件也要注册
// 多价导入记录mapper         ExtensionMybatisDasHelper. addMappingExtension ( this , DbConstant. DB_NAME , ISalesMapper. class );  
 

```
##增加临时表
```
CREATE TABLE `t_sales_importcheck` (
  `id` INT(11) NOT NULL AUTO_INCREMENT COMMENT '自增ID',
  `createTime` DATETIME DEFAULT NULL COMMENT '创建时间',
  `createPin` VARCHAR(50) DEFAULT NULL COMMENT '创建人',
  `updateTime` DATETIME DEFAULT NULL COMMENT '更新时间',
  `updatePin` VARCHAR(50) DEFAULT NULL COMMENT '更新人',
  `status` INT(11) DEFAULT NULL COMMENT '校验状态,0:未校验,1:校验通过,2:校验未通过',
  `fileName` VARCHAR(512) DEFAULT NULL COMMENT '文件名称',
  `filePath` VARCHAR(256) DEFAULT NULL COMMENT '文件路径',
  PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8 COMMENT='多价导入校验表'
`yn` bit(1) DEFAULT b'1' COMMENT '该条记录是否有效',

ALTER TABLE `t_sales_importcheck` ADD COLUMN yn BIT(1) NOT NULL AFTER updatePin;//该条记录是否有效

ALTER TABLE `t_sales_importcheck` ALTER yn SET DEFAULT b'1';//修改yn字段的默认值

ALTER TABLE `t_sales_importcheck` ADD COLUMN ts TIMESTAMP NOT NULL AFTER id;//当前服务器系统时间

ALTER TABLE `t_sales_importcheck` MODIFY id INT(11) UNSIGNED AUTO_INCREMENT; //为id属性增加unsigned属性（编号上限可以扩大一倍）



```
##mapper插入记录后返回主键


```
1.mapper insert 需要增加 useGeneratedKeys = "true"   keyProperty = "id"   keyColumn = "id"
2.接口文件参数需要为实体：public   int  insertSalesCheck(SaleCheckEntity  saleCheckEntity );   

< insert   id = "insertSalesCheck"   parameterType = "com.haiziwang.jplugin.study.dbo.model.SaleCheckEntity"   useGeneratedKeys = "true"   keyProperty = "id"   keyColumn = "id" >
        insert into t_sales_importcheck
        (status,fileName,filePath,createTime,createPin) 
        values
        (${status},#{fileName},#{filePath},now(),#{createPin})      </ insert >  


3.调用时，参数实体既是入参，也是出参
SaleCheckEntity  checkEntity  =  new  SaleCheckEntity();
                 checkEntity .setCreatePin( param .get( "createPin" ).toString()); // 创建人
                 checkEntity .setStatus( checked  ==  true  ?  new  Integer(1) :  new  Integer(2)); // 验证是否通过
                 checkEntity .setFileName( retLs .get(0).split( "," )[1]);
                 checkEntity .setFilePath(PropertiesUtil. get ( "ftp.path" ) +  "/"  +  fileName );
                 int   check_id  =  checkService .insertSalesCheck( checkEntity );
                System. err .println( "是否成功:"  +  check_id  +  ",check_id:"  +  checkEntity .getId());  //这里取出插入后的主键



```
##导入规则信息表与补偿信息表的校验规则与数据组装逻辑


```
1.规则与补偿的格式校验：目前前端只校验数字格式，其他不管。
2.补偿信息与规则的校验：
3.规则表中的 折扣设置 与 定价金额 不能同时设置。


```
##修改多价导入
```
< update   id = "updateStatus"   parameterType = "map" >
        update t_core_betray set status = ${status},updateTime = #{updateTime},updatePin=#{updatePin},yn=1
         < where >
            id = ${id}
         </ where >      </ update >   



< update   id = "update" >
        update DBCustomer set custName=#{custName},address=#{address}
        where custId=#{custId}      </ update >  
 

```
##导入中台校验结果
```
1.<多价开始时间和结束时间无效>
2.<多价价格 MutiPrice 没有设置>

long   multiPrice  = NormalUtil. getFormatPrice (String. valueOf ( line .get(8)));                      priceRule .setMutiPrice( multiPrice ); // 定价金额   

3.<补偿方式为按折扣分摊，却没设置供应商承担比例>
compensateType=2;
Vector<PriceCompensatePo>  vecPriceCompensatePo  =  new  Vector<PriceCompensatePo>();  
所以组织规则的补偿信息时，要 store_code+skuid + compensateType 去对应。
4.对于规则表中的参与实体 如果 在补偿表中没有匹配到的，就是默认的参与实体。
 



```


##审核多价导入，即批量添加多价规则 error
```
[2016/11/16 12:09:24][INFO][Logger4Log4j] 创建多价入参：com.haiziwang.jplugin.study.dbo.protocol.SetMultiPriceRuleReq@17e722c9
[WIN] missing ENV parameter CONFIG4CGI_API! please config right value! value is the configfile's path!
十一月 16, 2016 12:09:24 下午 org.apache.coyote.AbstractProtocol pause
信息: Pausing ProtocolHandler ["http-nio-8083"]
十一月 16, 2016 12:09:24 下午 org.apache.coyote.AbstractProtocol pause
信息: Pausing ProtocolHandler ["ajp-nio-8012"]
$$$ now to stop plugin envirment
close rpc service port...
close http service port... $$$ plugin envirment stopped   

```
##多价导入列表查询
```
==>  Preparing: select sl.id,sl.status,sc.status importStatus,sl.fileName,sl.filePath,DATE_FORMAT(sl.createTime, '%Y-%m-%d %H:%i:%S') createTime, DATE_FORMAT(sl.updateTime, '%Y-%m-%d %H:%i:%S') updateTime,sl.createPin from t_sales_importLog sl, t_sales_importcheck sc WHERE sl.check_id = sc.id and sl.createTime LIKE CONCAT('%',?,'%') order by id desc limit ?,? 
[2016/11/16 14:43:12][DEBUG][BaseJdbcLogger] ==> Parameters: 2016-11-16(String), 0(Integer), 10(Integer)
[2016/11/16 14:43:12][DEBUG][BaseJdbcLogger] <==      Total: 0
[2016/11/16 14:43:12][DEBUG][BaseJdbcLogger] ==>  Preparing: select count(1) from t_sales_importLog WHERE createTime LIKE CONCAT('%',?,'%') 
[2016/11/16 14:43:12][DEBUG][BaseJdbcLogger] ==> Parameters: 2016-11-16(String) [2016/11/16 14:43:12][DEBUG][BaseJdbcLogger] <==      Total: 1  
 

```
##导入ExcelUtil
```
private   static  XSSFWorkbook printExcel2007(List<List<List<Object>>>  sheetList ) {
         // 创建工作簿实例
        XSSFWorkbook  workbook  =  new  XSSFWorkbook();
         for  ( int   k  = 0;  k  <  sheetList .size();  k ++) {
            List<List<Object>>  osh  =  sheetList .get( k );
            XSSFSheet  sheet  =  null ;
             // int cellNum = osh.get(0).size();
             int   cellNum  = 0;
             // 创建工作表实例
             if  (0 ==  k ) {
                 cellNum  = Integer. parseInt (MultiPriceEnum. IMPORTRULECOUNT .getValue());
                 sheet  =  workbook .createSheet( "规则信息" );
            }  else   if  (1 ==  k ) {
                 cellNum  = Integer. parseInt (MultiPriceEnum. IMPORTCOMPCOUNT .getValue());
                 sheet  =  workbook .createSheet( "补偿信息" );
            }
             cellNum  += 1; // 结果列
             // 设置列宽
             setSheetColumnWidth ( cellNum ,  sheet );
             // 获取样式
            XSSFCellStyle  style  =  createTitleStyle ( workbook );
             if  ( osh  !=  null ) {
                 // 给excel填充数据
                 for  ( int   i  = 0;  i  <  osh .size();  i ++) {
                     // 将dataList里面的数据取出来
                    List<Object>  line  =  osh .get( i );
                     int   lineSize  =  line .size();
                    System. err .println( "line's size:"  +  lineSize );
                    XSSFRow  row1  =  sheet .createRow( i ); // 建立新行
                     if  ( i  == 0) {
                         for  ( int   j  = 0;  j  <  lineSize ;  j ++) {
                            System. out .println( k  +  "表头第【"  +  j  +  "】行:"  +  line .get( j ));
                             createCell ( row1 ,  j ,  style , XSSFCell. CELL_TYPE_STRING ,  line .get( j ));
                        }
                    }  else  {
                         if  ( k  == 0) {
                             for  ( int   j  = 0;  j  <  lineSize ;  j ++) {
                                System. out .println( k  +  "表第【"  +  j  +  "】行:"  +  line .get( j ));
                                 createCell ( row1 ,  j ,  style , XSSFCell. CELL_TYPE_STRING ,  line .get( j ));
                            }
                        }  else  {
                             for  ( int   j  = 0;  j  <  lineSize ;  j ++) {
                                System. out .println( k  +  "表第【"  +  j  +  "】行:"  +  line .get( j ));
                                 createCell ( row1 ,  j ,  style , XSSFCell. CELL_TYPE_STRING ,  line .get( j ));
                            }
                        }
                    }
                }
            }  else  {
                 createCell ( sheet .createRow(0), 0,  style , XSSFCell. CELL_TYPE_STRING ,  "查无资料" );
            }
        }
         return   workbook ;     }   







getLineRuleCompensate  


System. err .println(NormalUtil. isNullOrEmpty (String. valueOf ( compensate .getAgioRate())));//WRONG
                        System. err .println(String. valueOf ( compensate .getAgioRate()).equals(SaleCenterConstants. ZERO ));//WIGHT


 

```
##导入 去除 正在导入中的提示
```
//$.blockUI({ message: '<h1><img src="../images/loading.gif" />正在导入......</h1>' });
                    $("#cancel").click();
                    $. ajax/*FileUpload*/    ({ 
                        url:url,
                        // secureuri:false,
                        // fileElementId:'excel',
                        dataType:  'json' ,
                        success:  function  (data){
                             //if(data.readyState == "complete"){
                                 if (data.resultCode ==  "1" ){
                                     //$.growlUI('提示', '');
                                     //$.unblockUI();
                                    alert( "导入成功" );
                                    $( "#excel" ).val( "" );
                                    $( "#cancel" ).click();
                                    $( "#query" ).click();
                                }
                             //}
                             else {
                                $( "#autocomplete" ).click();
                                alert( "导入失败,"  + data.msg);
                            }
                        }                     });   

```
##批量添加规则接口
```
对于一条规则：[8000, 线上, 539866, 秒杀价, , , , , 17.7, , , 1, 不补偿, 2016-11-19 15:30:00, 2016-11-19 23:59:00, , 3, 2, 1, 0, , 80, 成功] result=10241
【result = 10241,参数错误 】
 <多价状态 PriceState 没有设置><创建者 PriceCreaterId 没有设置>

 这两个参数在校验的时候是不会去校验的

Integer  priceSceneId  = Integer. parseInt (getParam( "priceSceneId" ));   

priceRuleCreateParam .setPriceSceneId( priceSceneId ); // 价格类型   

priceRuleCreateParam .setPriceState(1); // 初始   

String  createPin  = NormalUtil. decodeToString (getParam( "createPin" ));  
  priceRuleCreateParam .setPriceCreaterId( userName );//添加者

```