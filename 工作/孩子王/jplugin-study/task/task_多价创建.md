##多价管理 
###线上与线下的checkbox 
0.当选择门店+线下，并且 若优惠选择折扣，则下方展示 “ 折扣 设置 ” ；
但传值的是同一个字段，就是定价金额字段。



当销售渠道 选择线下时，展示的列表为：
1.线上与线下不同

2.只有线下时才有，线上时默认可积分

3.线上与线下不同，线上时与之前的一样可以精确到时间；当线下时才有促销时段；

4.线上时默认选择定价 ，而折扣置灰。线下时，定价与折扣都可选


5.线下时才有批量数量与 是否整件价


##补偿方式的select【前提条件是选择的优惠类型】


1.当优惠类型为定价时，补偿方式分为不补偿 和 按销量补偿；
当选择【销量补偿】


```
注意：
1.参与 实体 为必须项，为【store_code】，当光标blur时，必须为上面门店已选择的store_code。
2.销售渠道 为 可选 【线上与线下】，默认【线下】
3.供应商 可能为 【门店查询时带出来的信息】
4.参考进价 【商品查询时 带出来的信息】
5.特供价 【验证两位小数】
6.添加【行数不能超过上面已选门店的数量 】；比如上面门店总共选择了20个，则行数最多为20行【包括默认】；如果添加了9行数据，则9行按添加的计算，其他 的11行数据 按 默认计算。




```


2.当优惠类型为折扣时，补偿方式分类不补偿 和 按折扣分摊。
当选择【折扣分摊】



```
注意：验证【 供应商承担比例 】 0<100的两位小数
```
##关于参与实体
```
如果参与实体(点击添加行数) = 已选门店，则默认的那一行不参与计算。
如果参与实体（点击添加行数）< 已选门店，则其他的均按默认处理。
```
##协议文件的变更
```
//多价创建
req . setPriceRuleSetParamIn   ==>  setPriceRuleCreateParamIn (PriceRuleCreatePramPo  value )
 
```
##多价列表、详情页面也要改
```
列表：去除分站，增加 销售渠道 、实体 查询选项和列
详情：首先进去显示 的补偿方式单个信息，后面放个按钮，点击后再点击 列表信息。


getSiteName   ==》 getSaleWayName
```


##多价创建 商品编码查询
```
< div   class = "col-md-2" >
                                         < a   href = "#"   <%--  data-target="#mySkuModal_autocomplete" --%>   role = "button"   class = "btn green"   id = "query"   data-toggle = "modal" > 查询 </ a >
                                         < a   href = "#"   <%-- data-target="#myModal_autocomplete" --%>   role = "button"   class = "btn red"   data-toggle = "modal" > 多价信息 </ a >                                      </ div >   

```
##多价创建的问题


多价查询列表时(sales.jsp)，参与实体有可能是必传，当选择线上商城时，entityCode = 8000; 否则 entityCode = 输入的门店的编码。
```
com.haiziwang.jplugin.study.dbo.PriceRuleCreatePramPo

/**
     * 价格优惠类型 1 - 定价 2 - 折扣
     *
     * 版本 >= 20161020
     */      private   long   priceMode ;  

/**
     * 获取是否整件价 0 - 否 1 - 是
     * 
     * 此字段的版本 >= 20161020
     * 
     *  @return  isWholePrice value 类型为:long
     * 
     */
     public   long  getIsWholePrice() {
         return   isWholePrice ;     }   



/**
     * 补偿方式 0 - 不补偿 1 - 按销量补偿 2 - 按折扣分摊
     *
     * 版本 >= 20161020
     */      private   long   compensateType ;   

```

红框非必填项。
但是如果填了 批量数量，则是否整件价为必填项。
##验证图标
```
$( "#coupon_resourceBatch_sure" ).css( "color" , "green" ); $( "#coupon_resourceBatch_sure" ).text( "✔" );  
$( "#coupon_resourceBatch_sure_" +temp).css( "color" , "red" ); $( "#coupon_resourceBatch_sure_" +temp).text( "✘" );   

 

```
##多价详情、列表、审核
##促销列表、详情
##多价联调接口



```
开发环境的读取， jplugin升级到1.5.1前可以在我们本地配置环境，升级后就不能在本地配置了，改由C++在配置中心配置。
中台将更新后的接口发布到测试环境后，我们再调用 ，可能会出现调用超时的情况：
除了测试机器重启外，还有可能是我们本地的po文件与中台的po文件不一致导致。
```
##关于补偿方式
```
 如果选  线上商城，那就只能选线上销售渠道，就不展示 补偿方式
如果选门店，那不管选线上还是线下，都展示补偿方式
中台-吴浩 2016/11/2 11:23:23

默认选择  不补偿





// 补偿方式列表（销售渠道：线下）
            String  entityList  = getParam( "entityList" );
             entityList  = java.net.URLDecoder. decode ( entityList ,  "UTF-8" );
            Vector<PriceCompensatePo>  compenVector  =  new  Vector<PriceCompensatePo>();
             // JSONArray data = JSONArray.parseArray(entityList);
             // if(null!=data){
             // for(Object obj:data){
             // PriceCompensatePo compen = new PriceCompensatePo();
             // compen.setEntityId();
             // }
             // }
            List<PriceCompensatePo>  priceCompensateList  =  new  ArrayList<PriceCompensatePo>();
             if  ( entityList  !=  null  &&  entityList  !=  "" ) {
                 priceCompensateList  = JSONArray. parseArray ( entityList , PriceCompensatePo. class );
            }
             if  ( null  !=  priceCompensateList  &&  priceCompensateList .size() > 0) {
                 // for(PriceCompensatePo compen :priceCompensateList2){
                 // compenVector.add(compen);
                 // }
                 compenVector .addAll( priceCompensateList );
            }
             priceRuleCreateParam .setCompensateType( compensateType ); // 补偿方式
             if  (!String. valueOf ( compensateType ).equals( "0" )) {
                 priceRuleCreateParam .setVecPriceCompensatePo( compenVector );             }   

```


##特供价
```
// 特供价
         /*
         * if (!NormalUtil.isNullOrEmpty(specCost)) { Long longSpecCost =
         * NormalUtil.GetLongByFloat(Double.parseDouble(specCost));
         * priceRuleCreateParam.setSpecCost(longSpecCost);// 元转化成分 }
         */
         /*
         * if(!NormalUtil.isNullOrEmpty(supplyPrice)){
         * priceRuleCreateParam.setPurchaseCost(NormalUtil.GetLongByFloat(
         * Double. parseDouble(supplyPrice))); }          */   

```
##多价列表，参与实体
```
默认展示一个，点击后弹出已选门店信息。
```
##查询商品信息，先去除起购量与购买倍数


##多价详情


##多价查询


```
http://localhost:8083/jplugin-study/ocanal/multiPriceQueryList.do?pageno=1&pagesize=10&skuId=&saleway=2&entity=8000&status=0
会查询出线下及全渠道的，因为这里是包含。

查询销售渠道为：全渠道，也会查询出线上、线下、全渠道。
查询销售渠道为：线上，会查询出线上，全渠道。
```
##多价 查看同时段其他价格
```
老版本是没有分页功能 的。
function  getRuleList(pageno,pagesize){
        $( '#sample_10 tbody' ).html( '' );
        $( "#skuname" ).html( '' ); 
        $( "#dialogskuname" ).html( '' );
        $( "#compensateType" ).prop( "disabled" ,  false );
        
        
         var  skuId=$( "input[name='skucode']" ).val();
         if (skuId== "" ){
            alert( "请输入商品ID" );
             return ;
        }
        
        $.ajax({
            url: '${ctx}/ocanal/multiPriceQueryList.do' ,
            data:{ "skuId" :skuId, "pageno" :pageno, "pagesize" :pagesize},
            dataType: 'json' ,
            cache: false ,
            type: 'get' ,
            success: function (data){
                 var  dlgHtmlBeg =  "<tr>" ;
                 var  dlgHtmlEnd =  "</tr>" ;
                 var  dialogHtml =  "" ;
                 if (data.resultCode ==  "1" ){
                     var  prplist = data.databody;
                    
                     if (prplist!=undefined){
                         for ( var  i=0;i<prplist.length;i++){
                            dialogHtml +=  "<tr><td>" +prplist[i][ "priceId" ]+ "</td>"
                                        + "<td>" +getMultiPriceType(prplist[i][ "priceSceneId" ])+ "</td>"
                                         /*+"<td>"+ prplist[i]["priceUserIdentityRule"] + "</td>"*/
                                        + "<td>" +getLocalTime(prplist[i][ "priceStartTime" ])+ " 至 " + getLocalTime(prplist[i][ "priceEndTime" ]) + "</td>"
                                         /*+"<td>"+ getSiteName(prplist[i]["siteId"])+"</td>"*/
                                        + "<td>" +prplist[i][ "priceName" ]+ "</td>"
                                        + "<td>" +getMultiPriceDetail(prplist[i][ "priceSceneId" ],prplist[i])+ "</td>"    //定价
                                        + "<td>" +prplist[i][ "priceBuyLimitRule" ]+ "</td>"
                                        + "<td>" +prplist[i][ "priceCreaterId" ]+ "</td>"
                                        + "<td>" +prplist[i][ "priceLastModifiyer" ]+ "</td>"
                                        + "<td>" +getMultiPriceStatus(prplist[i][ "priceState" ])+ "</td></tr>" ;
                        }
                    }
                    
                     var  skuname=data.msg;
                    
                    $( '#sample_10 tbody' ).html(dialogHtml);
                    
                    $( "#skuname" ).html(skuname); 
                    $( "#dialogskuname" ).html(skuId+ "---" +skuname);
                     if (data.totalRecord%pagesize==0){
                        setPage(data.totalRecord/pagesize,data.totalRecord,pageno);
                    }
                     else {
                         //总页码，总条数，当前页码
                        setPage(parseInt(data.totalRecord/pagesize)+1,data.totalRecord,pageno);
                    }
                } else {
                    alert(data.msg);
                    setPage(0,0,1);
                }
            }
        });
    }
     function  setPage(totalPage,totalRecords,pageNo){    
         if (!pageNo){
            pageNo = 1;
        }
        kkpager.generPageHtml({
            pno : pageNo,
             //总页码
            total : totalPage,
             //总数据条数
            totalRecords : totalRecords,
            mode :  'click' ,  //设置为click模式
             //链接前部
            hrefFormer :  'pager_test' ,
             //链接尾部
            hrefLatter :  '.html' ,
            click :  function (n){
                 //处理完后可以手动条用selectPage进行页码选中切换
                 this .selectPage(n);
                
                getPromotionList(n,10);
                 //alert(n);
            }
        }, true );     }  
但是 有的字段中台接口中是没有的

```



