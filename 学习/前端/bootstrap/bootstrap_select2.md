##select2赋值
```
$("#id").select2().select2("val", 'oneofthevaluehere');
$('#id').select2().select2('val', $('.select2 option:eq(1)').val());


```
##select2应用
```
1、多选效果
 select2的多选很简单，设置一个属性就好了。


<script src="~/Scripts/jquery-1.10.2.js"></script>  <script src="~/Content/bootstrap/js/bootstrap.js"></script> <link href="~/Content/bootstrap/css/bootstrap.css" rel="stylesheet" />  <script src="~/Content/select2-master/dist/js/select2.js"></script> <link href="~/Content/select2-master/dist/css/select2.css" rel="stylesheet" />  　　<select id="sel_menu2" multiple="multiple" class="form-control">   <optgroup label="系统设置">    <option value="1">用户管理</option>    <option value="2">角色管理</option>    <option value="3">部门管理</option>    <option value="4">菜单管理</option>   </optgroup>   <optgroup label="订单管理">    <option value="5">订单查询</option>    <option value="6">订单导入</option>    <option value="7">订单删除</option>    <option value="8">订单撤销</option>   </optgroup>   <optgroup label="基础数据">    <option value="9">基础数据维护</option>   </optgroup>  </select>  //多选 $("#sel_menu2").select2({  tags: true,  maximumSelectionLength: 3 //最多能够选择的个数 }); 


2、图文结合的效果


<select id="sel_menu" class="js-example-templating js-states form-control">    <optgroup label="系统设置">     <option value="1">用户管理</option>     <option value="2">角色管理</option>     <option value="3">部门管理</option>     <option value="4">菜单管理</option>    </optgroup>    <optgroup label="订单管理">     <option value="5">订单查询</option>     <option value="6">订单导入</option>     <option value="7">订单删除</option>     <option value="8">订单撤销</option>    </optgroup>    <optgroup label="基础数据">     <option value="9">基础数据维护</option>    </optgroup>   </select>   $(function () { //带图片 $("#sel_menu").select2({  templateResult: formatState,  templateSelection: formatState });}); function formatState(state) { if (!state.id) { return state.text; } var $state = $(  '<span><img src="/content/images/' + state.element.value.toLowerCase() + '.ico" class="img-flag" /> ' + state.text + '</span>' ); return $state;}; 


3、远程搜索功能（即在用户输入搜索内容时动态去后台取数据）


<select id="sel_menu3" class="js-data-example-ajax form-control">  <option value="3620194" selected="selected">请选择</option> </select>   $(function () { //远程筛选 $("#sel_menu3").select2({  ajax: {   url: "/Home/GetProvinces",   dataType: 'json',   delay: 250,   data: function (params) {    return {     q: params.term, // search term     page: params.page    };   },   processResults: function (data, params) {    params.page = params.page || 1;     return {     results: data.items,     pagination: {      more: (params.page * 10) < data.total_count     }    };   },   cache: true  },  escapeMarkup: function (markup) { return markup; }, // let our custom formatter work  minimumInputLength: 1,  templateResult: formatRepoProvince, // omitted for brevity, see the source of this page  templateSelection: formatRepoProvince // omitted for brevity, see the source of this page });});   function formatRepoProvince(repo) { if (repo.loading) return repo.text; var markup = "<div>"+repo.name+"</div>"; return markup;} 


这里有要注意的一个地方就是processResults属性对应的方法有一个more属性用于是否分页显示的，这里的值要和你需要一次显示的值的条数匹配。
 
后台对应的方法如下：


public List<string> lstProvince = new List<string>() {"北京市","天津市","重庆市","上海市","河北省","山西省","辽宁省","吉林省","黑龙江省","江苏省","浙江省","安徽省","福建省","江西省","山东省","河南省","湖北省","湖南省","广东省","海南省","四川省","贵州省","云南省","陕西省","甘肃省","青海省","台湾省","内蒙古自治区","广西壮族自治区","西藏自治区","宁夏回族自治区","新疆维吾尔自治区","香港特别行政区","澳门特别行政区" };  public JsonResult GetProvinces(string q, string page)   {   var lstRes = new List<Province>();   for (var i = 0; i < 30; i++)   {    var oProvince = new Province();    oProvince.id = i;    oProvince.name = lstProvince[i];    lstRes.Add(oProvince);   }   if (!string.IsNullOrEmpty(q.Trim()))   {    lstRes = lstRes.Where(x => x.name.Contains(q)).ToList();   }   var lstCurPageRes = string.IsNullOrEmpty(page) ? lstRes.Take(10) : lstRes.Skip(Convert.ToInt32(page) * 10 - 10).Take(10);   return Json(new { items = lstCurPageRes, total_count = lstRes.Count }, JsonRequestBehavior.AllowGet);  } 


上面说了这么多，那么我们在选中select2的选项之后如何取值和赋值呢？
 
1、获取选中的值


var oMenuIcon = $("#txt_menuicon_web").select2({   placeholder: "请选择菜单图标",   templateResult: oInit.formatState,   templateSelection: oInit.formatState  });oMenuIcon.val(); 


2、设置select2的选中值
 


var oMenuIcon = $("#txt_menuicon_web").select2({  placeholder: "请选择菜单图标",  templateResult: oInit.formatState,  templateSelection: oInit.formatState });oMenuIcon.val("CA").trigger("change"); 


```


##通过 placeHold 来显示 select2的编辑带过来的值
```
  // 初始化下拉框并选中对应的关键字
                 var  msgTemplateId = $( '#msgId' ).val();
                 var  oMenu = $( '#kid' ).select2({
                    placeholder : msgTemplateId,
                     // minimumInputLength: 1,
                    ajax : {  
                        url : basePath +  "/message/getKeyWord.do" ,
                        dataType :  'json' ,
                        quietMillis : 250,
                        data :  function (term, pageNumber) {
                             return  {
                                name : term,  // search term
                                pageSize:10,
                                pageNumber:pageNumber
                            };
                        },
                        results :  function (data, pageNumber) { 
                             if (data&&data.rows&&data.rows.length){     
                                 var  more = (pageNumber*10)<data.total;     //用来判断是否还有更多数据可以加载
                                 return  {
                                    results:data.rows,more:more    
                                };
                            } else {
                                 return  {
                                    results : data.rows
                                };
                            }                
                        },
                        cache :  true
                    },
                    initSelection :  function (element, callback) {
                    },
                    id :  function (priv) {
                         return  priv.msgTemplateId;
                    },
                    formatResult :  function (word) { 
                         if  (word.msgTemplateId){
                             var  markup =  "<div id='"
                                    + word.msgTemplateId
                                    +  "'>"
                                    + word.name +  "</div>" ;
                             return  markup;
                        }
                    },  // omitted for brevity, see the source of this page  
                    formatSelection :  function (repo) {                           return  repo.name;   

```