##满返券设置

```
输入优惠券批次号（ 18440、 18429 ），点击添加查询出来此券的相关信息，可以修改返券数量。


```
##带框的品类与商品
```
在< div   class = "row"   >前面加上：
<!-- 
                    <div class="row">
                        <label class="control-label col-md-1"> </label>
                        <div class="col-md-10">
                            <div class="panel panel-default">
                                      <div class="panel-body"> -->   

在后面加上：
<!-- 
                                    </div>
                                </div>
                            </div>
                            <label class="control-label col-md-1"> </label>
                        </div>  -->   

 
```
##验证商品，要确定商家是如何定义的




```
原来传入的参数： http://pro.haiziwang.com/jplugin-study/cust/checkSkus.do
skuids:1234
sellerids:
ruleType:4


提示商品id=1234的商品不存在，现在的参数：


skuids:
1234

sellerids:
1

ruleType:
1
```
##线下参与品类的实现


##各种div的展示与隐藏
线上商城时，







