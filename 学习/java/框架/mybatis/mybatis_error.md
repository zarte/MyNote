[TOC]
##org.apache.ibatis.exceptions.TooManyResultsException: Expected one result (or null) to be returned by selectOne(), but found: 4
```
查询参数没有传入ID.
```
##Could not resolve type alias 'Library'.  Cause:java.lang.ClassNotFoundException: Cannot find class: Library
原因:xml文件中使用了别名，但配置文件中没有配置别名。
解决一：不使用别名，改用全名。
E:\adanac_demo\zTree\zTree-intf\src\main\resources\mybatis.mapper\LibraryMapper.xml
<mapper namespace="mybatis.mapper.LibraryMapper">
    <resultMap id="BaseResultMap" type="Library">
</mapper>
改为
<mapper namespace="mybatis.mapper.LibraryMapper">
    <resultMap id="BaseResultMap" type="com.adanac.study.ztree.bean.Library">
</mapper>
解决二：在配置文件中配置别名
E:\adanac_demo\zTree\zTree-intf\src\main\resources\config\mybatis-config.xml
<configuration>
    <typeAliases>
        <typeAlias type="com.adanac.study.ztree.bean.Library" alias="Library"/>
    </typeAliases>
</configuration>
##org.mybatis.spring.transaction.SpringManagedTransaction.getTimeout() mybatis和spring-mybatis版本问题
出现这个错误的原因主要是spring-mybatis和mybatis版本不匹配，产生冲突的原因；我测试的时候mybatis和spring-mybatis的版本分别为：3.4.1和1.1.1会出现此错误，经过再三测试3.3.1和1.1.1；3.4.1和1.3.1没有错误。
解决：在Web项目的pom文件中配置
<!-- mybatis版本号 -->
<mybatis.version>3.4.0</mybatis.version>
<mybatis-spring.version>1.3.0</mybatis-spring.version>
<tomcat.jdbc.version>8.0.30</tomcat.jdbc.version>
由于在common项目的pom文件中配置的mybatis的版本为3.4.0，所以配置
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>${mybatis-spring.version}</version>
</dependency>
##Cause: java.lang.IllegalArgumentException: Mapped Statements collection does not contain value for mybatis.mapper.SalesMapper..insert
原因：实现类中NAMESPACE中多写个.
private static final String NAMESPACE = "mybatis.mapper.SalesMapper.";
##Parameters: null 
[zTree 2017-03-17 17:22:53,605](DEBUG) ==>  Preparing: select id, create_id, update_id, create_time, update_time, validity, code, function_name, parent_id, is_leaf_node, sub_system_id, is_hidden from t_ztree_privilege where id = ?  
[zTree 2017-03-17 17:22:53,634](DEBUG) - mybatis.mapper.PrivilegeMapper.selectByPrimaryKey - (BaseJdbcLogger.java:145) ==> Parameters: null 
[zTree 2017-03-17 17:22:53,947](DEBUG) - mybatis.mapper.PrivilegeMapper.selectByPrimaryKey - (BaseJdbcLogger.java:145) <==      Total: 0 

原因：mapper文件中参数与实体中的参数不一致
解决：create_id 修改为 createId

<select id="selectOneByCode" resultMap="BaseResultMap" parameterType="Privilege">
    select
    <include refid="Base_Column_List"/>
    from t_ztree_privilege
    <where>
      <if test="id != null and id != ''">
        and id = #{id}
      </if>
      <if test="create_id != null and create_id != ''">
        and create_id = #{createId}
      </if>
      <if test="updateId != null and updateId != ''">
        and updte_id #{updateId}
      </if>
    </where>
    limit 1
  </select>

 ##查询方法 parameter 为null
 /**
   * 根据id查询条数
   * @return
   */
  public int getLibraryByPid(Library library);
 <!--根据id查询条数-->
  <select id="getLibraryByPid" parameterType="Library" resultType="java.lang.Integer">
      select count(*) from t_library
      <where>
          <if test="Pid != null and Pid !=''">
              and pid = #{Pid}
          </if>
      </where>
  </select>

##Caused by: java.lang.IllegalArgumentException: Result Maps collection does not contain value for mybatis.mapper.CurdMapper.ZTree
原因：
1.没有ZTree类。
2.CurdMapper的实现类CurdMapperImpl中 注解没有写全。
@Repository 改为 @Repository("curdMapper")

主要是因为你的select标签内部的resultMap属性指向的不正确

在sql文件中只要有一个resultMap或resultType属性指向错误，则在这个文件中其余正确的语句也不能执行，

所以在出现上述错误时，可能不是当前正在执行的语句的错误，而是该文件中其它语句映射错了认真检查其它语句。

未找到真正原因。

3.真正原因：在测试list方法，报错，却不是list的问题，而是get的问题。
<select id="get" resultMap="com.adanac.study.ztree.bean.ZTree" parameterType="java.lang.Integer">
        select
        <include refid="Base_Column_List"/>
        from t_ztree
        where id=#{id,jdbcType=INTEGER}
    </select>
Result Maps collection does not contain value for com.adanac.study.ztree.bean.ZTree
  at org.apache.ibatis.session.Configuration$StrictMap.get(Configuration.java:853)
解决：resultMap 改为 resultType。因为返回值只有一个。

##Caused by: java.lang.ClassCastException: java.lang.Long cannot be cast to java.lang.Integer
原因：接口中的参数是Long,
ZTree get(Long id);
而mapper文件中的参数却为Integer。
##MyBatis不识别Integer值为0的数据
<if test="form.passLine != null and  form.passLine != '' ">  
    and is_live =  #{form.passLine,jdbcType=INTEGER}  
</if>
修改为：
<if test="form.passLine != null and  form.passLine != -1 ">  
    and is_live =  #{form.passLine,jdbcType=INTEGER}  
</if>
或
<if test="form.passLine != null">  
    and is_live =  #{form.passLine,jdbcType=INTEGER}  
</if>

##mybatis3中，数据库字段为空，结果集不返回字段名
```
查询一个列表，当某字段的值为null的时候，返回的结果集中会不显示该字段名称。
在Mybatis框架配置文件中加一句即可。

<configuration>     
    <settings>   
        <setting name="callSettersOnNulls" value="true"/> <!-- 返回空字段 -->  
    </settings>     
</configuration>  

还有一种方式是建立一个类，实现Mybatis的TypeHandler接口。
```