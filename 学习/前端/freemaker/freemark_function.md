## freemarker 解析后台传来的json字符串
```
1.java后台代码传json字符串
// 查询已添加商品列表   

ModelAndView  mav  =  new  ModelAndView( "pages/sale/coupon/BC_couponMod.ftl" );   

Pager<CouponGoodsDto>  pager  =  disRuleService .queryAddList( id ,  pageParam );
List<CouponGoodsDto>  dataList  =  pager .getDatas();
mav .addObject( "goodsList" ,  dataList );
JSONArray  oldGoodsList  = JSONArray. fromObject ( dataList ); mav .addObject( "oldGoodsList" ,  oldGoodsList .toString());   



2.页面接收数据
<input type="text" id="oldGoodsList" value='eval("("+${oldGoodsList}+")")' />
得到的数据格式为：
eval("("+[{"barCode":"110","name":"chenjianihao","ruleId":"4504f5b9e4c24b358458752427102b04","skuId":"201604171758069628","spuId":"f5ce4cf302fb405384e6127d7ce60712"},{"barCode":"11","name":"小白好美的","ruleId":"4504f5b9e4c24b358458752427102b04","skuId":"7632f2ed211b42bc9ea9d963c19e98df","spuId":"2c6d16e703ee462b98099c6296009e10"},{"barCode":"123","name":"小白你好","ruleId":"4504f5b9e4c24b358458752427102b04","skuId":"baae69f97c8c4ab9ba6860fe8b16bf7d","spuId":"513dbcec5182422790b85e33aed510dd"}]+")")  


3.由于数据格式不对，所以要通过js处理：
//存放条码编码对应商品信息的集合
var  goodsList =  new  Array();//定义一个数组
var  temp = $( "#oldGoodsList" ).val();
var  strlen0 = temp.length;//字符串的长度
var  strlen1 = eval(strlen0+ "-" +5);//字符串长度减少5
var  temp0 = temp.substring(9,strlen1);//从9个开始截取，保留中间的字符串，去掉最后5个字符串
console.log(temp0)
var  ods = eval(temp0);//将string转为json数组对象
for ( var  i=0;i<ods.length;i++){
    goodsList.push(ods[i]);//将转换后的对象放入到数组中 }   



4.再将新的数据传到后台
//维护保存
$( "#couponGoodsM" ).on( "click" , function () {
     var  ruleId = $( "#ruleId" ).val();
    console.log(barCodeList)
    console.log(goodsList)
     var  oldnum = $( "#oldnum" ).val();
     var  couponGoodsForm = $( '#couponGoodsForm' );
     var  param = $.serializeObject(couponGoodsForm);
     var  paramJson = encodeURI(JSON.stringify(goodsList));   //前端编码
    $.ajax({  
        url: '${base}/cr/addCouponGoods.do?ruleId=' +ruleId+ '&oldnum=' +oldnum, // 跳转到 action
        data:{ "paramJson" :paramJson},
        type: 'post' ,  
        dataType: 'json' ,
        success:  function (data) {
             if (data.status ==  "1"  ){
                console.log( "修改优惠券商品成功！" );
                layer.msg( '修改成功！' , {time: 1500});
                goBack();
            } else {
                console.log( "修改优惠券商品失败！" );
                layer.msg( '修改失败！' , {time: 1500});
            }  
         },  
         error :  function () {
             console.log( "修改优惠券商品异常！" );
             layer.msg( '修改异常！' , {time: 1500});
         }  
    });
});


5.后台接收数据：
     /**
     * 批量添加条码券商品
     * 
     *  @param  request
     *  @param  response
     *  @return
     */
     @SuppressWarnings ({  "deprecation" ,  "unchecked"  })
     @RequestMapping (value =  "addCouponGoods" , produces =  "text/html;charset=UTF-8" )
     public   void  addCouponGoods(HttpServletRequest  request , HttpServletResponse  response ,
             @RequestParam  String  paramJson ) {
         // 获取参数
        Map<String, Object>  result  =  new  HashMap<String, Object>();
         try  {
            String  ruleId  =  request .getParameter( "ruleId" );
            String  oldnum  =  request .getParameter( "oldnum" );
             paramJson  = java.net.URLDecoder. decode ( paramJson ,  "utf-8" );
             log .info( "DisRuleAction====>addCouponGoods====>paramJson:"  +  paramJson .toString());
            CompanyInfoDto  companyInfoDto  =  this .getMerchantInfo( request );
            String  suppId  =  companyInfoDto .getCompanyId().toString();
             // JSONObject jsonObject = JSONObject.fromObject(paramJson);//将json数据转为json对象
            JSONArray  data  = JSONArray. fromObject ( paramJson );//将json数据转为json数组
            BaseResult  br  =  disRuleService .addCouponGoods( data ,  ruleId ,  oldnum );
             if  ( br .getStatus().equals(MmcConstants. REG_SUCC )) {
                 result .put( STATUS ,  SUCCESS );
                 result .put( MESSAGE ,  "操作成功" );
            }  else  {
                 result .put( STATUS ,  ERROR );
                 result .put( MESSAGE ,  "操作失败" );
            }
        }  catch  (Exception  e ) {
             log .error( "DisRuleAction====>addCouponGoods====>error：" ,  e .toString());
             result .put( STATUS ,  ERROR );
             result .put( MESSAGE ,  "操作异常" );
        }
        ajaxJson( response , JSONObject. fromObject ( result ).toString());     }   

```