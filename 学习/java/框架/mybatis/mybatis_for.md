##mybatis插入对象 对应字段为null
```
因为传入的参数的字段为null，对象无法获取对应的jdbcType类型，而报的错误。 
只要在insert语句中insert的对象加上jdbcType就可以了，修改如下： 
#{menuTitle,jdbcType=VARCHAR} 
这样就可以解决错误了。 
```

##Mybatis foreach 批量操作
```
foreach属性
属性 描述
item 循环体中的具体对象。支持属性的点路径访问，如item.age,item.info.details。
具体说明：在list和数组中是其中的对象，在map中是value。
该参数为必选。
collection 要做foreach的对象，作为入参时，List<?>对象默认用list代替作为键，数组对象有array代替作为键，Map对象用map代替作为键。
当然在作为入参时可以使用@Param("keyName")来设置键，设置keyName后，list,array,map将会失效。 除了入参这种情况外，还有一种作为参数对象的某个字段的时候。举个例子：
如果User有属性List ids。入参是User对象，那么这个collection = "ids"
如果User有属性Ids ids;其中Ids是个对象，Ids有个属性List id;入参是User对象，那么collection = "ids.id"
上面只是举例，具体collection等于什么，就看你想对那个元素做循环。
该参数为必选。
separator 元素之间的分隔符，例如在in()的时候，separator=","会自动在元素中间用“,“隔开，避免手动输入逗号导致sql错误，如in(1,2,)这样。该参数可选。
open foreach代码的开始符号，一般是(和close=")"合用。常用在in(),values()时。该参数可选。
close foreach代码的关闭符号，一般是)和open="("合用。常用在in(),values()时。该参数可选。
index 在list和数组中,index是元素的序号，在map中，index是元素的key，该参数可选。


<select id="countByUserList" resultType="_int" parameterType="list">    
select count(*) from users    
  <where>    
    id in    
    <foreach item="item" collection="list" separator="," open="(" close=")" index="">    
      #{item.id, jdbcType=NUMERIC}    
    </foreach>    
  </where>    
</select>   


：：select count(*) from users WHERE id in ( ? , ? ) 
<insert id="addList">  
          
        INSERT INTO DELIVER  
            (  
                <include refid="selectAllColumnsSql"/>  
             )  
           
          <foreach collection="deliverList" item="item" separator="UNION ALL">  
                SELECT   
                     #{item.id, jdbcType=NUMERIC},  
                     #{item.name, jdbcType=VARCHAR}  
                FROM DUAL  
          </foreach>  
    </insert>  


：：insert into deliver select ?,? from dual union all select ?,? from dual

<insert id="ins_string_string">    
        insert into string_string (key, value) values    
        <foreach item="item" index="key" collection="map"    
            open="" separator="," close="">(#{key}, #{item})</foreach>    
    </insert>   


：：insert into string_string (key, value) values (?, ?) , (?, ?)  -- mysql

<select id="sel_key_cols" resultType="int">    
        select count(*) from key_cols where    
        <foreach item="item" index="key" collection="map"    
            open="" separator="AND" close="">${key} = #{item}</foreach>    
    </select>   


：：select count(*) from key_cols where col_a = ? AND col_b = ?  (一定要注意到$和#的区别，$的参数直接输出，#的参数会被替换为?，然后传入参数值执行。)
```

##oracle+mybatis 使用动态Sql当插入字段不确定的情况下实现批量insert
```
最近做项目遇到一个挺纠结的问题，由于业务的关系，DB的数据表无法确定，在使用过程中字段可能会增加，这样在insert时给我造成了很大的困扰。


先来看一下最终我是怎么实现的：
 
<insert id="batchInsertLine" parameterType="HashMap"> 
   <![CDATA[ 
   INSERT INTO tg_fcst_lines(${lineColumn}) 
   select result.*,sq_fcst_lines.nextval from( 
   ]]> 
   <foreach collection="lineList" item="item" index="index" separator="union all" > 
    (select  
    <foreach collection="item" index="key" item="_value" separator=","> 
      #{_value} 
    </foreach>  
    from dual) 
   </foreach> 
   <![CDATA[) result]]>   
 </insert>
 


由于数据表不确定，所以我无法确定我要insert的字段，由于是批量insert，确定value值也挺费劲。
 我传给mybatis的参数是一个map:
 
Map insertMap = new HashMap(); 
insertMap.put("lineColumn",lineColumn);    
insertMap.put("lineList", lineList);
lineColumn是一个字符串，lineList是一个list:  List<Map> lineList = new ArrayList();
 
lineList里存放的是map，map的键对应数据表的字段，值是你要insert的值，这样就可以通过foreach取出list的值作为insert语句的value，但由于map是无序的，存放的顺序和 遍历时取值的顺序不一定一致，所以为了确保insert字段和值可以一一对应，可以通过遍历一次map来取出key拼接一个字符串作为insert的字段


String lineColumn = "";  //拼接的SQL,作为insert语句的一部分 
[java] view plaincopy
Map<String,String> lineMap = lineList.get(0); 
for (String key : lineMap.keySet()) { 
  lineColumn +=key+","; 
} 
lineColumn +="LINE_ID";
 
这里的line)id是一个自增的字段，在语句中直接写序列会报错，所以先遍历list将取出的值作为result,在取出result的所有值，连同序列一起作为insert的值。
在取值的时候使用两个foreace嵌套来实现，外层的foreach遍历list,里层的foreach遍历map。

```
##mybatis 批量插入
<img src="images/img4.png" height="229" width="415">
这种形式可以的，如果数量非常大就分批。

