##下拉菜单的查询


views/ibd/ibd-appointment.html:
```
< div   class = "form-group" >
  < label   class = "control-label col-md-4" > 仓库： </ label >
  < div   class = "col-md-8" >
  < div   custom-select = "s.sysNo as s.whName for s in  whOptions  | filter: { whName: $searchTerm } track by s.sysNo"   ng-model = "whSysNo" ></ div >
  </ div > </ div >   

```


masterDataService  在contoller.js中第一行已定义
 

发现发出的请求为：






mybatis.mapper.MstWhMapper.selectMstWhDaoList
MstWhListReqDto reqDto[draw 0,rows 2147483647,start 0,yn 1]

##全选效果的实现
ibd-appointment.html：





checkAll()方法在 webapp/js/controllers/ibd/ibd-appointment-controller.js

##时间控件的实现


```

< input   type = "text"   class = "form-control"   maxlength = "25"   id = "scheduledDateStart"   name = "scheduledDateStart"   onClick = "WdatePicker({minDate:'%y-%M-{%d+1}'})"   >
< input   type = "text"   class = "form-control"   maxlength = "25"   id = "scheduledDateEnd"   name = "scheduledDateEnd"   onClick = "WdatePicker({minDate:'#F{$dp.$D(\'scheduledDateStart\')}'})" >
```



