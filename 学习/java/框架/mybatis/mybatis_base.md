##mybatis批量更新
##Mybatis_插入数据后返回主键ID_返回数据字段与类中字段相对应
```
正确Mapper.xml中的写法
<insert id="insertUser" parameterType="cn.itcast.mybatis.po.User">  
    <!--   
    将插入数据的主键返回，返回到user对象中  
      
    =================SELECT LAST_INSERT_ID()：得到刚insert进去记录的主键值，只适用与自增主键  
      
    keyProperty：将查询到主键值设置到parameterType指定的对象的哪个属性  
    order：SELECT LAST_INSERT_ID()执行顺序，相对于insert语句来说它的执行顺序  
    resultType：指定SELECT LAST_INSERT_ID()的结果类型  
     -->  
    <selectKey keyProperty="id" order="AFTER" resultType="java.lang.Integer">  
        SELECT LAST_INSERT_ID()  
    </selectKey>  
    INSERT INTO user(username,birthday,sex,address) VALUES(#{username},#{birthday},#{sex},#{address})   
    <!--   
    使用mysql的uuid（）生成主键  
    执行过程：  
    首先通过uuid()得到主键，将主键设置到user对象的id属性中  
    其次在insert执行时，从user对象中取出id属性值  
     -->  
    <!--  
<selectKey keyProperty="id" order="BEFORE" resultType="java.lang.String">  
        SELECT uuid()  
    </selectKey>  
    insert into user(id,username,birthday,sex,address) value(#{id},#{username},#{birthday},#{sex},#{address})
 -->  
 </insert>  
```
##mybatis插入记录返回主键的两种方式
http://blog.csdn.net/prevention/article/details/32825081
1、XyzMapper.xml

<insertid=“doSomething"parameterType="map"useGeneratedKeys="true"keyProperty=“yourId">
...
</insert>

或

<insert id=“doSomething" parameterType=“com.xx.yy.zz.YourClass" useGeneratedKeys="true" keyProperty=“yourId">
...
</insert>


2、XyzMapper.java

public int doSomething(Map<String, Object> parameters);

or

public int 
doSomething(YourClass c);

3、要在map或c中有一个字段名为yourId，Mybatis会自动把主键值赋给这个字段。

Map<String, Object> parameters = new HashMap<String, Object>();
parameters.put(“yourId”, 1234);
...
mapper.doSomething(parameters);
System.out.println(“id of the field that is primary key” + parameters.get(“yourId"));

或

YourClass c = new YourClass();
...
mapper.doSomething(c);
System.out.println(“id of the field that is primary key” + c.yourId);


