## 创建、详情页面增加特供价（非必选、只校验格式就好）。


在团购单价下面添加特供价。
groupbuy_createrule.jsp、groupbuy_info.jsp（详情页面）
GroupBuyController.java（Controller）


```
initGroupRuleSku(ruleData);

//填充拼团规则商品信息
     function  initGroupRuleSku(ruleData){
        $( "#sample_7 tbody" ).html( "" );
         var  dlgHtml =  "<tr><td>" +ruleData.itemSkuId+ "</td>"
                    + "<td>" +ruleData.itemTitle+ "</td>"
                    + "<td>￥" +getYuanByFen(ruleData.basePrice)+ "</td>"
                    + "<td>￥" +getYuanByFen(ruleData.sellPrice)+ "</td>"
                    + "<td>￥" +getYuanByFen(ruleData.price)+ "</td>"
                    + "<td>￥" +getYuanByFen(ruleData.specCost)+ "</td>"
                    + "<td>" +ruleData.priceRuleId+ "</td>"
                    + "<td>" +ruleData.buyNum+ "</td></tr>" ;
        $( "#sample_7 tbody" ).html(dlgHtml);     }   

```

