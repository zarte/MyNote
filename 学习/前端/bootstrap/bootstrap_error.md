##Uncaught TypeError: $(...).bootstrapTable is not a function
```
<!-- 表格插件 -->
     < link   rel = "stylesheet"   href = " ${base} /RES/assets/plugins/bootstrap-table/css/bootstrap-table.min.css" >      < script   src = " ${base} /RES/assets/plugins/bootstrap-table/js/bootstrap-table.js" ></ script >   

```
##bootstrap 表格出来了，但分页插件没有
```
有可能是数据传入参数有问题：
Pager<SkuDto>  datas  =  skuQueryService .querySkus( sku ,  param );  
List<GoodDto>  list  =  new  ArrayList<GoodDto>();
         for  (SkuDto  s  :  skuList ) {
            GoodDto  dto  =  new  GoodDto();
             // dto.setId(id);
             dto .setFskuId( s .getFskuId());
                 list .add( dto );   
                      } 
Pager<GoodDto>  res  =  new  Pager<GoodDto>( datas .getTotalDataCount(),  pageParam .getPageSize(),
                 pageParam .getPageNumber());          res .setDatas( list );  
return res;


@ResponseBody
     @RequestMapping (value =  "listGoods" )      public  String listGoods(HttpServletRequest  request , BootstrapPage  page , GoodQueryDto  param ) {  
               Map<String, Object>  map  =  new  HashMap<String, Object>();
             map .put( "pageNumber" ,  page .getPageNumber()); // 页码
             map .put( "pageSize" ,  page .getPageSize()); // 每页条数  



Pager<GoodDto>  pager  =  activityGoodService .listGoods( map );
            List<GoodDto>  infoList  =  pager .getDatas();
            BootstrapTable<GoodDto>  data  =  new  BootstrapTable<GoodDto>( infoList ,
                     pager .getTotalDataCount() ==  null  ? 0 :  pager .getTotalDataCount(),  pager .getPageNumber());
             return  JSONObject. fromObject ( data ).toString();   

 

```