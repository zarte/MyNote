##判断是否选了Radio
```
方法： typeof (isWholePrice)=="undefined" 或 if(!isWholePrice)
实例：
var isWholePrice = $( "input[name='isWholePrice']:checked" ).val();   


               if ( typeof (isWholePrice)=="undefined"){
                    alert( "请选择是否整件价" );
                     return ;                 }   

```
##radio置灰 与 可用
```
置灰：$( "input[name='isWholePrice']" ).attr( "disabled" , "disabled" );  
可用： $( "input[name='isWholePrice']" ).removeAttr( "disabled" );
去除选中： $( "input[name='isWholePrice']" ).removeAttr( "checked" );
 

```
## JQuery控制radio选中和不选中方法总结
```
根据值设置radio选中、使用prop方法操作


一、设置选中方法
 $("input[name='名字']").get(0).checked=true; 
 $("input[name='名字']").attr('checked','true');
 $("input[name='名字']:eq(0)").attr("checked",'checked'); 
 $("input[name='radio_name'][checked]").val();  //获取被选中Radio的Value值




二、设置选中和不选中示例
<input type="radio" value="0" name="jizai" id="0"/>否
<input type="radio" value="1" name="jizai"  id="1"/>是
#jquery中，radio的选中与否是这么设置的。
$("#rdo1").attr("checked","checked");
 $("#rdo1").removeAttr("checked");


通过name
 $("input:radio[name="analyfsftype"]").eq(0).attr("checked",'checked');
 $("input:radio[name="analyshowtype"]").attr("checked",false);


通过id
 $("#tradeType0").attr("checked","checked");
 $("#tradeType1").attr("checked",false);




三、另一种设置选中方法
 <script type="text/javascript">  
 $(document).ready(function(){  
         $("input[@type=radio][name=sex][@value=1]").attr("checked",true);  
 });  
 </script>  


您的性别：  


 <input type="radio" name="sex" value="1" <s:if test="user.sex==1">checked</s:if>/>男  
 <input type="radio" name="sex" value="0" <s:if test="user.sex==0">checked</s:if>/>女




四、根据值设置radio选中
 $("input[name='radio_name'][value='要选中Radio的Value值']").attr("checked",true);  //根据Value值设置Radio为选中状态




五、使用prop方法操作示例
 $('input').removeAttr('checked'); 
 $($('#'+id+'input').eq(0)).prop('checked',false);
 $($('#'+id+' input').eq(0)).prop('checked',true);
```
##jquery获取第一个元素并选中
```
$( "input[name='priceMode']:eq(0)" ).attr( "checked" , "checked" );   

```
##jquery获取选中单选按钮radio的值
```
1.页面：

<div class="ma_l10 discount_radio_block">
                                                    <label>
                                                        <#if activity.discountType == 2>
                                                        <input type="radio" name="optionsRadios" id="" value="2" checked>
                                                        <#else> 
                                                        <input type="radio" name="optionsRadios" id="" value="2">
                                                        </#if>
                                                        <span class="">打折</span>
                                                    </label>
                                                </div>
                                                <div class="ma_l10 discount_radio_block">
                                                    <label>
                                                        <#if activity.discountType == 1>
                                                        <input type="radio" name="optionsRadios" id="" value="1" checked>
                                                        <#else>
                                                        <input type="radio" name="optionsRadios" id="" value="1">
                                                         </#if> 
                                                        <span class="">一口价</span>
                                                    </label>
                                                </div>  
2.语法：
var discountType = $('input[name="optionsRadios"]:checked').val();  

```