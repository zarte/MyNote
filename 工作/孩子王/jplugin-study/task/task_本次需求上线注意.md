##com.haiziwang.jplugin.study.ctrl.CustomerController.importSales(HttpServletRequest, Map<String, Object>)
```
此方法已无用， // req.setPriceRuleSetParamIn(priceRuleSetParamIn);
SetMultiPriceRuleReq   此dto已变为全渠道的dto。
 

```
##配置列表、详情页面 审核按钮
## 对于品类，需要增加字段
```
查询线上、线下的品类 是同样的接口，调徐琦的。
创建规则时，我们本地会落地时（方便查询促销规则详情的时候展示 ），所以需要增加一个字段来区分 线上还是线下。
【脚本 要保留好，上线前让DBA先去执行】
ALTER TABLE `t_core_salecentercategory` ADD COLUMN channelType INT(11) COMMENT'渠道类型:0全渠道,1线上,2线下' AFTER categoryType;
```
##多价导入脚本
##新的菜单配置
```
http://test.sso.haiziwang.com/sso-web/common/main.jsp


```


##全渠道促销菜单配置



