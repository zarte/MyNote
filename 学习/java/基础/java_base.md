[TOC]
##解决前台传到后台日志乱码问题
```
EncodingFilter.java

package  com.haiziwang.jplugin.study.Util;
import  java.io.IOException;
import  javax.servlet.Filter;
import  javax.servlet.FilterChain;
import  javax.servlet.FilterConfig;
import  javax.servlet.ServletException;
import  javax.servlet.ServletRequest;
import  javax.servlet.ServletResponse;
/**
 * 编码过滤器
 *  @ClassName : EncodingFilter 
 */
public   class  EncodingFilter  implements  Filter {
    
     private  String  encoding= "utf-8" ;
     public   void  destroy() {
        
    }
     public   void  doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
             throws  IOException, ServletException {
           //设置request字符编码
           request.setCharacterEncoding(encoding);
            //设置respongse字符编码
           response.setContentType( "text/html;charset=" +encoding);
           chain.doFilter(request, response);
    }
     public   void  init(FilterConfig arg0)  throws  ServletException {
        
        
    }
}


web.xml

  <filter>
      <filter-name>encodingFilter</filter-name>
        <filter-class>com.haiziwang.jplugin.study.Util.EncodingFilter</filter-class>
  </filter>
  <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>   </filter-mapping>   

```
## spring mvc 下载文件 IE浏览器文件名是乱码
```
使用poi,做传统的excel导出，然后想在浏览器中，让用户选择另存为，保存用户下载的xls文件，这个时候，可能的是在ie下出现乱码（ie,9,10,11),但在firefox,chrome下没乱码， 

页面下载文件时，内容都是中文，只有文件名是乱码，在谷歌等浏览器下是可以的。

     /**
     * 
     *  @Title : processFileName
     * 
     *  @Description : ie,chrom,firfox下处理文件名显示乱码
     */
     public   static  String processFileName(HttpServletRequest  request , String  fileNames ) {
        String  codedfilename  =  null ;
         try  {
            String  agent  =  request .getHeader( "USER-AGENT" );
             if  ( null  !=  agent  && -1 !=  agent .indexOf( "MSIE" ) ||  null  !=  agent  && -1 !=  agent .indexOf( "Trident" )) { // ie
                String  name  = java.net.URLEncoder. encode ( fileNames ,  "UTF8" );
                 codedfilename  =  name ;
            }  else   if  ( null  !=  agent  && -1 !=  agent .indexOf( "Mozilla" )) { // 火狐,chrome等
                 codedfilename  =  new  String( fileNames .getBytes( "UTF-8" ),  "iso-8859-1" );
            }
        }  catch  (Exception  e ) {
             e .printStackTrace();
        }
         return   codedfilename ;     }   


然后java代码

response.setHeader("Cache-Control", "private");  

response.setHeader("Pragma", "private");  

response.setContentType("application/vnd.ms-excel;charset=utf-8");  

response.setHeader("Content-Type", "application/force-download");  

title = FileUtil.processFileName(request, title);  

response.setHeader("Content-disposition", "attachment;filename=" + title + ".xls");
```
##解决浏览器兼容问题，文件下载不同浏览器乱码
```
// 设置response的Header
             if  ( request .getHeader( "User-Agent" ).toUpperCase().indexOf( "MSIE" ) > 0) {
                 filename  = URLEncoder. encode ( filename ,  "UTF-8" );
            }  else  {
                 filename  =  new  String( filename .getBytes( "UTF-8" ),  "ISO8859-1" );
            }
             response .setContentType( "application/vnd.ms-excel;charset=UTF-8" );
             response .addHeader( "Content-Disposition" ,  "attachment;filename="  +  filename );              response .addHeader( "Content-Length" ,  ""  +  file .length());   

```
##java web项目中使用多个数据源
```
是可以使用多个数据源的，如果整合了mybatis.在写sql时，加上对应数据库的schema就可以了。

举例：
1.service中的配置文件：/rageon-service/src/main/resources/conf/spring/spring-res.xml：
<!-- JNDI数据源 -->
    <jee:jndi-lookup id="dataSource" jndi-name="jdbc/demo"
        proxy-interface="javax.sql.DataSource" lookup-on-startup="false" />
    <jee:jndi-lookup id="dataSource" jndi-name="jdbc/rageon"         proxy-interface="javax.sql.DataSource" lookup-on-startup="false" /> 

2.tomcat服务器的context.xml：
  <Resource auth="Container" driverClassName="com.mysql.jdbc.Driver"

        maxActive="100" maxIdle="30" maxWait="10000" name="jdbc/demo"
        password="root" type="javax.sql.DataSource"
        url="jdbc:mysql://192.168.1.173/demo?useUnicode=true&characterEncoding=utf-8"
    username="root" testWhileIdle="true" timeBetweenEvictionRunsMillis="10000" validationQuery="select 1" />
    <Resource auth="Container" driverClassName="com.mysql.jdbc.Driver"
        maxActive="100" maxIdle="30" maxWait="10000" name="jdbc/rageon"
        password="root" type="javax.sql.DataSource"
        url="jdbc:mysql://192.168.1.173/rageon?useUnicode=true&characterEncoding=utf-8"
    username="root" testWhileIdle="true" timeBetweenEvictionRunsMillis="10000" validationQuery="select 1" /> 3.service中的sqlMap文件：sqlMap_tabCommon.xml    
<sql id="delete">
        <![CDATA[
            delete from demo.tab_common where ID = :id
        ]]>     </sql>  

```
##获取java类的当前路径 

```
this.getClass().getResource("/").getPath()//获取类的当前目录

```

##获取项目根目录

```
1.普通java工程中获取

方式一：
String path = System.getProperty("user.dir");

方式二：
File directory = new File("");// 参数为空 String courseFile = directory.getCanonicalPath();  

2.web工程中获取
String path = this.getServletContext().getRealPath("/");【从servlet出发】
String path=request.getSession().getServletContext().getRealPath("/");【 从httpServletRequest出发 】

3. 在jsp文件或Servlet中，
可以通过getServletContext().getRealPath("/")来获取项目根目录的绝对路径。
<% 
String realPath=request.getServletContext().getRealPath("/");//项目绝对路径
%>
<%=realPath%>
获取到(E:\adanac_demo\zTree\zTree-web\target\zTree-web\)

```

##过多if-else分支的优化
```
int code;
if("Name".equals(str))
    code = 0;
else if("Age".equals(str))
    code = 1;
else if("Address".equals(str))
    code = 2;

1. 用一个Map可以做到，if-else的变化点使用Map的get方法来代替：
Map typeCodeMap = new HashMap();
typeCodeMap.put("Name", 0);
typeCodeMap.put("Age", 1);
typeCodeMap.put("Address", 2);
...
int code = typeCode.get(type);

2. 枚举：
public enum Codes {
    Name(0), Age(1), Address(2);
    public int code;
    Codes(int code){
        this.code = code;
    }
}
 

//使用：
int code = Codes.valueOf(str).code;

3. 多态：

ICode iCode = (ICode)Class.forName("com.xxx." + str).newInstance();
int code = iCode.getCode();
```

##java long类型转为string并且保留两位小数
```
DecimalFormat  df  =  new  DecimalFormat( "0.00" );
String  num  =  df .format( po .getMutiPrice() / 100.0); po .setPrice( num );  
```
##java的四舍五入
```
11.556 = 11.56          ——六入
11.554 = 11.55          —–四舍
11.5551 = 11.56         —–五后有数进位
11.545 = 11.54          —–五后无数，若前位为偶数应舍去
11.555 = 11.56          —–五后无数，若前位为奇数应进位

下面实例是使用银行家舍入法：

public static void main(String[] args) {

        BigDecimal d = new BigDecimal(100000);      //存款

        BigDecimal r = new BigDecimal(0.001875*3);   //利息

        BigDecimal i = d.multiply(r).setScale(2,RoundingMode.HALF_EVEN);     //使用银行家算法 

 

        System.out.println("季利息是："+i);

        }

Output:

季利息是：562.50

在上面简单地介绍了银行家舍入法，目前java支持7中舍入法：

1、 ROUND_UP：远离零方向舍入。向绝对值最大的方向舍入，只要舍弃位非0即进位。

2、 ROUND_DOWN：趋向零方向舍入。向绝对值最小的方向输入，所有的位都要舍弃，不存在进位情况。

3、 ROUND_CEILING：向正无穷方向舍入。向正最大方向靠拢。若是正数，舍入行为类似于ROUND_UP，若为负数，舍入行为类似于ROUND_DOWN。Math.round()方法就是使用的此模式。

4、 ROUND_FLOOR：向负无穷方向舍入。向负无穷方向靠拢。若是正数，舍入行为类似于ROUND_DOWN；若为负数，舍入行为类似于ROUND_UP。

5、 HALF_UP：最近数字舍入(5进)。这是我们最经典的四舍五入。

6、 HALF_DOWN：最近数字舍入(5舍)。在这里5是要舍弃的。
7、 HAIL_EVEN：银行家舍入法。

提到四舍五入那么保留位就必不可少了，在java运算中我们可以使用多种方式来实现保留位。

保留位
方法一：四舍五入
double   f   =   111231.5585;
BigDecimal   b   =   new   BigDecimal(f);
double   f1   =   b.setScale(2,   RoundingMode.HALF_UP).doubleValue();
在这里使用BigDecimal ，并且采用setScale方法来设置精确度，同时使用RoundingMode.HALF_UP表示使用最近数字舍入法则来近似计算。在这里我们可以看出BigDecimal和四舍五入是绝妙的搭配。

方式二：
java.text.DecimalFormat   df   =new   java.text.DecimalFormat(”#.00″);
df.format(你要格式化的数字);
例：new java.text.DecimalFormat(”#.00″).format(3.1415926)

#.00 表示两位小数 #.0000四位小数 以此类推…

方式三：
double d = 3.1415926;
String result = String .format(”%.2f”);
%.2f %. 表示 小数点前任意位数   2 表示两位小数 格式后的结果为f 表示浮点型。

方式四：
此外如果使用struts标签做输出的话，有个format属性,设置为format=”0.00″就是保留两位小数

例如：
<bean:write name="entity" property="dkhAFSumPl"  format="0.00" />
或者
<fmt:formatNumber type="number" value="${10000.22/100}" maxFractionDigits="0"/>
maxFractionDigits表示保留的位数
```
##java bigDecimal四舍五入保留两位小数
```
bigDecimal =  bigDecimal.setScale(2, BigDecimal.ROUND_HALF_UP);
```
##java中double保留两位小数
```
方式一：

DecimalFormat df = new DecimalFormat("######0.0");  

            System.out.println(df.format(f)); 

方式二：
System.out.println(String.format("%.2f", f)); 

方式三：
BigDecimal b = bg.multiply(bg2);
double disPrice = b.setScale(2, BigDecimal.ROUND_HALF_UP).doubleValue();  
```
##java 只改变对象的一个值
```
拷贝对象：将dto的值拷贝到user
org . apache . commons . beanutils .BeanUtils.copyProperties(user, dto);
拷贝完成后再修改某个值即可：user.setName("allen");
```
##对比当前时间判断是否过期
```
//需要比对失效时间，失效的话则返回空
        Date endDate = DateUtils.parse(dto.getEndTime(), Constants.PATTREN_2);
        Date now = new Date();
        //该券已过期了
        if(endDate.before(now))
        {
            log.info("getCouponRule is expire {}",id);
            return null;
        }         return dto;  

```

##两个相同属性的对象拷贝
```
class LoginUser extends UserInfoDto 
public LoginUser refreshUser(LoginUser user){
        if(null != user && null != user.getMobile()){
            UserInfoDto dto = userIntfService.getUserInfo(user.getMobile());
            if(null != dto && null != dto.getUserId()){
                try {
                     org . apache . commons . beanutils .BeanUtils.copyProperties(user, dto);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
        return user;

    } 

```
## 产生一个Java的内存泄露
```
那么怎么才能产生一个内存泄露呢？

解决方案

在纯Java中，有一个很好的方式可以产生真正的内存泄露（通过执行代码使对象不可访问但仍存在于内存中）：
应用产生一个长时间运行的线程（或者使用一个线程池加速泄露）。
线程通过一个（可选的自定义）类加载器加载一个类。
该类分配大内存（例如，new byte[1000000]），赋值给一个强引用存储在静态字段中，再将它自身的引用存储到 ThreadLocal 中。分配额外的内存是可选的（泄露类实例就够了），但是这样将加速泄露工作。
线程清除所有自定义类的或者类加载器载入的引用。
重复上面步骤。

这样是有效的，因为ThreadLocal持有对象的引用，对象持有类的引用，接着类持有类加载器的引用。 反过来，类加载器持有所有已加载类的引用。这会使泄露变得更加严重，因为很多JVM实现的类和类加载都直接从持久带（permgen）分配内存，因而不会被GC回收。

``` 
## 使用Java在一个区间内产生随机整数数
```
我试着使用Java生成一个随机整数，但是随机被指定在一个范围里。例如，整数范围是5~10，就是说5是最小的随机值，10是最大的。5到10之间的书也可以是生成的随机数。

解决方案

标准的解决方式（Java1.7 之前）如下：
import java.util.Random;

public static int randInt(int min, int max) {

    Random rand;
    int randomNum = rand.nextInt((max - min) + 1) + min;

    return randomNum;
}

请查看 相关的JavaDoc 。在实践中， java.util.Random  类总是优于  java.lang.Math.random() 。

特别是当标准库里有一个直接的API来完成这个工作，就没有必要重复制造轮子了。

```

##（如何） 从数组创建ArrayList
```
我有一个数组，初始化如下：
Element[] array = {new Element(1), new Element(2), new Element(3)};
我希望将这个数组转化成一个ArrayList类的对象。

解决方案
new ArrayList<Element>(Arrays.asList(array))
```
##将两个list合并到一个map中
```
public   class  ListTest {
     public   static   void  main(String[]  args ) {
        Map<Integer, String>  map  =  new  HashMap<Integer, String>();
        List<String>  list1  =  initList1 ();
        List<Integer>  list2  =  initList2 ();
         for  ( int   i  = 0;  i  <  list1 .size();  i ++) {
             map .put( i ,  list1 .get( i ) +  ","  +  list2 .get( i ));
        }
        System. out .println( map .toString());
    }
     private   static  List<Integer> initList2() {
        List<Integer>  list  =  new  ArrayList<Integer>();
         list .add(1);
         list .add(2);
         list .add(3);
         return   list ;
    }
     private   static  List<String> initList1() {
        List<String>  list  =  new  ArrayList<String>();
         list .add( "a" );
         list .add( "b" );
         list .add( "c" );
         return   list ;
    }
}   

```
##遍历map的四种方式
```
Map<String, String> map = new HashMap<String, String>();
  map.put("1", "value1");
  map.put("2", "value2");
  map.put("3", "value3");
  
  //第一种：普遍使用，二次取值
  System.out.println("通过Map.keySet遍历key和value：");
  for (String key : map.keySet()) {
   System.out.println("key= "+ key + " and value= " + map.get(key));
  }
  
  //第二种
  System.out.println("通过Map.entrySet使用iterator遍历key和value：");
  Iterator<Map.Entry<String, String>> it = map.entrySet().iterator();
  while (it.hasNext()) {
   Map.Entry<String, String> entry = it.next();
   System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
  }
  
  //第三种：推荐，尤其是容量大时
  System.out.println("通过Map.entrySet遍历key和value");
  for (Map.Entry<String, String> entry : map.entrySet()) {
   System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
  }

  //第四种
  System.out.println("通过Map.values()遍历所有的value，但不能遍历key");
  for (String v : map.values()) {
   System.out.println("value= " + v);
  }
 }

```
## 遍历HashMap 的最佳方法
```
这样遍历 entrySet ：
public static void printMap(Map mp) {
    Iterator it = mp.entrySet().iterator();
    while (it.hasNext()) {
        Map.Entry pair = (Map.Entry)it.next();
        System.out.println(pair.getKey() + " = " + pair.getValue());
        it.remove(); // avoids a ConcurrentModificationException
    }
}

```

##InputStream转String
```
如果你有一个 java.io.InputStream 对象，如处理这个对象并生成一个字符串？
假定我有一个  InputStream  对象，它包含文本数据，我希望将它转化成一个字符串（例如，这样我可以将流的内容写到一个log文件中）。
InputStream  转化成 String 最简单方法是什么？

使用  Apache commons   IOUtils 库来拷贝InputStream到StringWriter是一种 不错的方式，类似这样：
StringWriter writer = new StringWriter();
IOUtils.copy(inputStream, writer, encoding);
String theString = writer.toString();

甚至
// NB: does not close inputStream, you can use IOUtils.closeQuietly for that
// 注意：不关闭inputStream，你可以使用 IOUtils.closeQuietly
String theString = IOUtils.toString(inputStream, encoding);

或者，如果不想混合Stream和Writer，可以使用  ByteArrayOutputStream。

来源：  http://www.codeceo.com/article/stackoverflow-10-java-problem.html
```
##java中的Number类
Java语言为每一个内置数据类型(byte、int、long、double)提供了对应的包装类。
所有的包装类（Integer、Long、Byte、Double、Float、Short）都是抽象类Number的子类。
```
##枚举的使用
```
实例一：

1.枚举的定义
public enum CouponRuleStatusEnum {
    //1:已保存 2:已发布 3:已终止
    /**
     * 1:已保存
     */
    SAVED(1),
    /**
     * 2:已发布
     */
    PUBLISH(2),
    /**
     *  3:已终止
     */
    STOPED(3);
        
    
    private int value;
    
    private CouponRuleStatusEnum(int value)
    {
        this.value=value;
    }
    public int getValue() {
        return value;
    }
    public void setValue(int value) {
        this.value = value;
    }
} 

2.枚举的使用
dto.setStatus(CouponRuleStatusEnum.SAVED.getValue());  
实例二：
public enum AfterSaleApplyStatus{
        UNCHECK(0, "待审核"),
        CHECKED(1, "通过审核"),
        REJECTED(2, "驳回");
        private final int value;
        private final String name;
        
        private AfterSaleApplyStatus(int value, String name) {
            this.value = value;
            this.name = name;
        }
        
        public int getValue() {
            return value;
        }
        public String getName() {
            return name;
        }
             }  

private Integer status;  

afterSaleApply.setStatus(DataDictionary.AfterSaleApplyStatus.UNCHECK.getValue());//待审批  

```
##javac 编译
###javac 编译：程序包org.junit不存在
```
javac OperateTest.java时，提示程序包不存在。
-classpath 类路径
设置用户类路径，它将覆盖 CLASSPATH 环境变量中的用户类路径。

javac -classpath E:\.m2\Repository\junit\junit\4.10\junit-4.10.jar OperateTest.java
即可。


package com.allen.common.test;
import org.junit.Test;
public class OperateTest {
    @Test
    public void testPlus() {
        int i = 0;
        i = i++;
        System.out.println(i);// 0
    }

    @Test
    public void testPlus2() {
        int i = 0;
        i++;
        System.out.println(i);// 1
    }

}
```
###使用javap反编译Java字节码文件
```
1、使用不带任何选项参数的命令：javap Person
javap Person和javap -package Person的显示结果一样，因为-package选项参数是默认的，用于显示package(不带任何访问修饰符，即我们常说的friendly)、protected、public修饰的类或成员。

备注：在dos下进入工作目录D:\java，然后使用命令javap test.Person也可以实现上述操作。下同。
2、使用命令：javap -public Person显示public修饰的类或成员。

与此类似，选项参数-protected用于显示protected以上访问级别(protected、public)的类或成员；选项参数-private用于显示private以上访问级别，也就是所有的类或成员。

3、使用命令：javap -public -l Person显示public修饰的类或成员，并显示行号表格和本地变量表格。

4、使用命令：javap -c Person显示Person.class反汇编出的字节码命令。

由于选项参数之间组合较多，因此其他选项参数不再一一截图赘述，仅在下面使用文字进行说明：

-classpath <pathlist>
手动指定用户class字节码文件的存放目录，javap程序将在此目录下查找class文件，多个路径以英文分号分隔。例如：javap -classpath D:\java\test Person(即使DOS窗口的当前工作目录为其他任意路径，该命令均可正确执行)。
-s
打印变量的内部类型签名，例如：javap -classpath D:\java\test -s Person。
-extdirs <dirs>
指定javap搜索已安装的java扩展的位置，默认的java扩展的位置为jre\lib\ext。例如：javap -classpath D:\java\test -extdirs D:\java\myext Person
-bootclasspath <pathlist>
指定使用Java底层类加载器(bootstrap class loader)加载的字节码文件的位置。例如：javap -classpath D:\java\test -bootclasspath D:\java\core Person
-verbose
打印方法参数和本地变量的数量以及栈区大小。
-J<flag>
使用javap.exe来执行java.exe虚拟机的相关命令，例如javap -J-version相当于java -version，可以有多个命令，中间以空格隔开。
```