[TOC]
##List接口、Set接口和Map接口
```
1、List和Set接口自Collection接口，而Map不是继承的Collection接口
  Collection表示一组对象,这些对象也称为collection的元素;一些 collection允许有重复的元素,而另一些则不允许;一些collection是有序的,而另一些则是无序的;JDK中不提供此接口的任何直接实 现,它提供更具体的子接口(如 Set 和 List)实现;Map没有继承Collection接口,Map提供key到value的映射;一个Map中不能包含相同key,每个key只能映射一个value;Map接口提供3种集合的视图,Map的内容可以被当做一组key集合,一组value集合,或者一组key-value映射;
2.、List接口 
  `元素有放入顺序，元素可重复`
  List接口有三个实现类：LinkedList，ArrayList，Vector 
    LinkedList：底层基于链表实现，链表内存是散乱的，每一个元素存储本身内存地址的同时还存储下一个元素的地址。链表增删快，查找慢 
    ArrayList和Vector的区别：ArrayList是非线程安全的，效率高；Vector是基于线程安全的，效率低 
  元素有放入顺序，元素可重复 
  List是一种有序的Collection，可以通过索引访问集合中的数据,List比Collection多了10个方法，主要是有关索引的方法。
    1).所有的索引返回的方法都有可能抛出一个IndexOutOfBoundsException异常
    2).subList(int fromIndex, int toIndex)返回的是包括fromIndex，不包括toIndex的视图，该列表的size()=toIndex-fromIndex。
所有的List中只能容纳单个不同类型的对象组成的表，而不是Key－Value键值对。例如：[ tom,1,c ];
所有的List中可以有相同的元素，例如Vector中可以有 [ tom,koo,too,koo ];
所有的List中可以有null元素，例如[ tom,null,1 ];
基于Array的List（Vector，ArrayList）适合查询，而LinkedList（链表）适合添加，删除操作;
3、Set接口
  `元素无放入顺序，元素不可重复（注意：元素虽然无放入顺序，但是元素在set中的位置是有该元素的HashCode决定的，其位置其实是固定的）`
  Set接口有两个实现类：HashSet(底层由HashMap实现)，LinkedHashSet 
  SortedSet接口有一个实现类：TreeSet（底层由平衡二叉树实现）
  Query接口有一个实现类：LinkList 
  Set具有与Collection完全一样的接口，因此没有任何额外的功能，不像前面有两个不同的List。实际上Set就是Collection,只是行为不同。(这是继承与多态思想的典型应用：表现不同的行为。)Set不保存重复的元素(至于如何判断元素相同则较为负责)
  Set : 存入Set的每个元素都必须是唯一的，因为Set不保存重复元素。加入Set的元素必须定义equals()方法以确保对象的唯一性。Set与Collection有完全一样的接口。Set接口不保证维护元素的次序。
  HashSet : 为快速查找设计的Set。存入HashSet的对象必须定义hashCode()。
  TreeSet : 保存次序的Set, 底层为树结构。使用它可以从Set中提取有序的序列。
  LinkedHashSet : 具有HashSet的查询速度，且内部使用链表维护元素的顺序(插入的次序)。于是在使用迭代器遍历Set时，结果会按元素插入的次序显示。
4、Map接口
  `以键值对的方式出现的`
  Map接口有三个实现类：HashMap，HashTable，LinkeHashMap 
    HashMap非线程安全，高效，支持null；
    HashTable线程安全，低效，不支持null 
    SortedMap有一个实现类：TreeMap 
```
##HashMap  与 LinkedHashMap
```
不同点：
1.HashMap put 进去数据不是有序的，而LinkedHashMap是有序的。

相同点：
二者 通过put方法，如果key相同，数据会覆盖。

@Test
public  void  test1() throws  Exception {
  Map<String, String>  map  =  new  LinkedHashMap<String, String>();
   for ( int i = 0;  i< 10; i ++) {
      map .put("" + i + 1,  "Name"  +  i );
  }
  map .put( "01" ,  "F_" );
  map .put( "11" ,  "Name1" );
  map .put( "21" ,  "F_Nme" );     
}   

``` 
##break 与 continue
```
1.break语句 （强行结束循环）
  break语句作用：1、可以用来从循环体内跳出循环体，即提前结束循环，接着执行循环下面的语句。2、使流程跳出switch结构
注意:break语句不能用于循环语句和switch语句之外的任何其他语句中 

2.continue语句作用：结束本次循环，即忽略循环体中continue语句下面尚未执行的语句，接着进行下一次是否执行循环的判定。
注意:continue语句不能用于循环语句之外的任何其他语句中 


3.continue语句和break语句的区别：
   continue语句只结束本次循环，而不是终止整个循环的执行。
   break语句则是结束整个循环过程，不再判断执行循环的条件是否成立。break语句可以用在循环语句和switch语句中。在循环语句中用来结束内部循环；在switch语句中用来跳出switch语句。 
注意:循环嵌套时，break和continue只影响包含它们的最内层循环，与外层循环无关。
```
##HashMap Hashtable区别
```
 我们先看2个类的定义
public class Hashtable
    extends Dictionary
    implements Map, Cloneable, java.io.Serializable
public class HashMap
    extends AbstractMap
    implements Map, Cloneable, Serializable
可见Hashtable 继承自 Dictiionary 而 HashMap继承自AbstractMap


Hashtable的put方法如下
public synchronized V put(K key, V value) {  //###### 注意这里1
      // Make sure the value is not null
      if (value == null) { //###### 注意这里 2
        throw new NullPointerException();
      }
      // Makes sure the key is not already in the hashtable.
      Entry tab[] = table;
      int hash = key.hashCode(); //###### 注意这里 3
      int index = (hash & 0x7FFFFFFF) % tab.length;
      for (Entry e = tab[index]; e != null; e = e.next) {
        if ((e.hash == hash) && e.key.equals(key)) {
          V old = e.value;
          e.value = value;
          return old;
        }
      }
      modCount++;
      if (count >= threshold) {
        // Rehash the table if the threshold is exceeded
        rehash();
        tab = table;
        index = (hash & 0x7FFFFFFF) % tab.length;
      }
      // Creates the new entry.
      Entry e = tab[index];
      tab[index] = new Entry(hash, key, value, e);
      count++;
      return null;
    }
注意1 方法是同步的
注意2 方法不允许value==null
注意3 方法调用了key的hashCode方法，如果key==null,会抛出空指针异常


HashMap的put方法如下
public V put(K key, V value) { //###### 注意这里 1
    if (key == null)  //###### 注意这里 2
      return putForNullKey(value);
    int hash = hash(key.hashCode());
    int i = indexFor(hash, table.length);
    for (Entry e = table[i]; e != null; e = e.next) {
      Object k;
      if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
        V oldValue = e.value;
        e.value = value;
        e.recordAccess(this);
        return oldValue;
      }
    }
    modCount++;
    addEntry(hash, key, value, i);  //###### 注意这里
    return null;
  }
注意2 方法允许key==null
注意3 方法并没有对value进行任何调用，所以允许为null
HashMap
允许将null作为一个entry的key或者value
非线程安全，效率略高，同步(Collections.synchronizedMap) 
-----------------------------------------------------------------------------------------
HashTable
不允许将null作为一个entry的key或者value
方法是Synchronize的，线程安全，效率低些，多个线程访问Hashtable时，不需要自己为它的方法实现同步
```
##静态代码块、构造代码块、构造函数的区别
```
静态代码块只执行一次。
构造代码块在每一次构造对象的开始执行，每构造一次都会执行一次，无论针对所有的对象初始化都会执行。
构造函数只会和相匹配的函数一致时才会执行。
静态代码块：
class StaticCode {
static {
System.out.println("I'm staticcode");
}
void show() {
System.out.println("show run");
}
}
public class StaticCode2 {
public static void main(String[] args) {
new StaticCode().show();
new StaticCode().show();
}
}
输出结果：
I'm staticcode
show run
show run
构造代码块和构造函数：
public class StaticConstructor {
public static void main(String[] args) {
Person p1 = new Person();
Person p2 = new Person("旺财");
}
}
class Person {
String name;
// 构造函数的代码块
{
System.out.println("哇啊");
}
Person() {
name = "baby";
show();
}
Person(String name) {
this.name = name;
show();
}
void show() {
System.out.println("name" + name);
}
}
输出结果：
哇啊
namebaby
哇啊
name旺财
```

##jdbc的executeUpdate()与executeUpdate(String sql)方法的区别
```
executeUpdate() 这是 PreparedStatement 接口自身的方法。
executeUpdate(String sql) 这是 PreparedStatement 从父接口 Statement 中继承过来的方法。
executeUpdate() 中执行 SQL 语句需要在创建 PerparedStatement 时通过 Connection 的 prepareStatement(String sql) 方法中写出，因为 PerparedStatement 中的 SQL 语句数据库需要进行预编译和缓存，因此要在创建 PerparedStatement 对象时给出 SQL 语句。
String sql = "delete from dept where deptno = ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, id);
int i = pstmt.executeUpdate();
而 executeUpdate(String sql) 是 Statement 中的方法，参数中的 SQL 语句只是提交给数据库去执行，并不需要预编译。
Statement st = conn.createStatement();
int i = st.executeUpdate(sql);
如果 SQL 语句中有 ? 占位符，那么在设置好占位符中的值后，必须使用 executeUpdate() 执行。
而 executeUpdate(String sql) 只是提交一个 SQL 语句，且这个语句中不能带有 ? 占位符。
在当前id下存在有多条记录，而只想根据时间更新最新的一条记录的情况下，使用executeUpdate()，执行的SQL正确的情况下，不会提交到数据库。而executeUpdate(sql)却可以自动提交到数据库。
public static Connection getConnectionForJDBC()
{
PropertiesUtil util = PropertiesUtil.getInstance();
try
{
if (conn == null)
{
// 加载驱动包
Class.forName(util.getValue("jdbc.className"));
conn =
DriverManager.getConnection(util.getValue("jdbc.url"), util.getValue("jdbc.user"),
util.getValue("jdbc.password"));
}
// 初始化数据库连接
}
catch (ClassNotFoundException e)
{
fileUtil.writeLog("驱动包加载失败，错误信息如下：" + e.getMessage());
}
catch (SQLException e)
{
fileUtil.writeLog("初始化数据库连接失败，错误信息如下：" + e.getMessage());
}
return conn;
}
/////////////////// executeUpate(sql)可以自动提交到数据库////////////////////////////
private static int changeSynflag(String id) {
String sql = "update user_common t set t.synflag = 0 where t.id'"
+ id
+ "' and t.updatetime = (select max(t2.updatetime) from user_common t2 where t2.id='"
+ id + "')";
Connection conn = OpraterOracle.getConnectionForJDBC();
PreparedStatement pstm;
int i = 0;
try {
pstm = conn.prepareStatement(sql);
i = pstm.executeUpdate(sql);
pstm.close();
conn.close();
} catch (SQLException e) {
e.printStackTrace();
}
return i;
}
/////////////////////如果换成executeUpdate()却不可以提交到数据库中/////////////////////
pstm = conn.prepareStatement(sql);
pstm.setString(1,id);
pstm.setString(2,id);
i = pstm.executeUpdate();
/////////////////////////////////////////////////////////////////////////////////
```
##Comparable和Comparator
JDK的大量的类包括常见的 String、Byte、Char、Date等都实现了Comparable接口，

Comparable
Comparable可以认为是一个内比较器，实现了Comparable接口的类有一个特点，就是这些类是可以和自己比较的，至于具体和另一个实现了Comparable接口的类如何比较，则依赖compareTo方法的实现.compareTo方法的返回值是int，有三种情况：

1、比较者大于被比较者（也就是compareTo方法里面的对象），那么返回正整数

2、比较者等于被比较者，那么返回0

3、比较者小于被比较者，那么返回负整数

写个很简单的例子：

public class Domain implements Comparable<Domain>
{
    private String str;

    public Domain(String str)
    {
        this.str = str;
    }

    public int compareTo(Domain domain)
    {
        if (this.str.compareTo(domain.str) > 0)
            return 1;
        else if (this.str.compareTo(domain.str) == 0)
            return 0;
        else 
            return -1;
    }
    
    public String getStr()
    {
        return str;
    }
}
public static void main(String[] args)
    {
        Domain d1 = new Domain("c");
        Domain d2 = new Domain("c");
        Domain d3 = new Domain("b");
        Domain d4 = new Domain("d");
        System.out.println(d1.compareTo(d2));
        System.out.println(d1.compareTo(d3));
        System.out.println(d1.compareTo(d4));
    }
运行结果为：

0
1
-1
注意一下，前面说实现Comparable接口的类是可以支持和自己比较的，但是其实代码里面Comparable的泛型未必就一定要是Domain，将泛型指定为String或者指定为其他任何任何类型都可以----只要开发者指定了具体的比较算法就行。

 

Comparator
Comparator可以认为是是一个外比较器，个人认为有两种情况可以使用实现Comparator接口的方式：

1、一个对象不支持自己和自己比较（没有实现Comparable接口），但是又想对两个对象进行比较

2、一个对象实现了Comparable接口，但是开发者认为compareTo方法中的比较方式并不是自己想要的那种比较方式

Comparator接口里面有一个compare方法，方法有两个参数T o1和T o2，是泛型的表示方式，分别表示待比较的两个对象，方法返回值和Comparable接口一样是int，有三种情况：

1、o1大于o2，返回正整数

2、o1等于o2，返回0

3、o1小于o3，返回负整数

写个很简单的例子，上面代码的Domain不变（假设这就是第2种场景，我对这个compareTo算法实现不满意，要自己写实现）：

public class DomainComparator implements Comparator<Domain>
{
    public int compare(Domain domain1, Domain domain2)
    {
        if (domain1.getStr().compareTo(domain2.getStr()) > 0)
            return 1;
        else if (domain1.getStr().compareTo(domain2.getStr()) == 0)
            return 0;
        else 
            return -1;
    }
}
public static void main(String[] args)
{
    Domain d1 = new Domain("c");
    Domain d2 = new Domain("c");
    Domain d3 = new Domain("b");
    Domain d4 = new Domain("d");
    DomainComparator dc = new DomainComparator();
    System.out.println(dc.compare(d1, d2));
    System.out.println(dc.compare(d1, d3));
    System.out.println(dc.compare(d1, d4));
}
看一下运行结果：

0
1
-1
当然因为泛型指定死了，所以实现Comparator接口的实现类只能是两个相同的对象（不能一个Domain、一个String）进行比较了，因此实现Comparator接口的实现类一般都会以"待比较的实体类+Comparator"来命名

总结
总结一下，两种比较器Comparable和Comparator，后者相比前者有如下优点：

1、如果实现类没有实现Comparable接口，又想对两个类进行比较（或者实现类实现了Comparable接口，但是对compareTo方法内的比较算法不满意），那么可以实现Comparator接口，自定义一个比较器，写比较算法

2、实现Comparable接口的方式比实现Comparator接口的耦合性 要强一些，如果要修改比较算法，要修改Comparable接口的实现类，而实现Comparator的类是在外部进行比较的，不需要对实现类有任何修 改。从这个角度说，其实有些不太好，尤其在我们将实现类的.class文件打成一个.jar文件提供给开发者使用的时候。实际上实现Comparator 接口的方式后面会写到就是一种典型的策略模式。
##快速理解Java中的五种单例模式

解法一：只适合单线程环境（不好）

package test;
/**
 * @author xiaoping
 *
 */
public class Singleton {
    private static Singleton instance=null;
    private Singleton(){
        
    }
    public static Singleton getInstance(){
        if(instance==null){
            instance=new Singleton();
        }
        return instance;
    }
}
注解:Singleton的静态属性instance中，只有instance为null的时候才创建一个实例，构造函数私有，确保每次都只创建一个，避免重复创建。
缺点：只在单线程的情况下正常运行，在多线程的情况下，就会出问题。例如：当两个线程同时运行到判断instance是否为空的if语句，并且instance确实没有创建好时，那么两个线程都会创建一个实例。

解法二：多线程的情况可以用。（懒汉式，不好）

public class Singleton {
    private static Singleton instance=null;
    private Singleton(){
        
    }
    public static synchronized Singleton getInstance(){
        if(instance==null){
            instance=new Singleton();
        }
        return instance;
    }
}
注解：在解法一的基础上加上了同步锁，使得在多线程的情况下可以用。例如：当两个线程同时想创建实例，由于在一个时刻只有一个线程能得到同步锁，当第一个线程加上锁以后，第二个线程只能等待。第一个线程发现实例没有创建，创建之。第一个线程释放同步锁，第二个线程才可以加上同步锁，执行下面的代码。由于第一个线程已经创建了实例，所以第二个线程不需要创建实例。保证在多线程的环境下也只有一个实例。
缺点：每次通过getInstance方法得到singleton实例的时候都有一个试图去获取同步锁的过程。而众所周知，加锁是很耗时的。能避免则避免。

解法三：加同步锁时，前后两次判断实例是否存在（可行）

public class Singleton {
    private static Singleton instance=null;
    private Singleton(){
        
    }
    public static Singleton getInstance(){
        if(instance==null){
            synchronized(Singleton.class){
                if(instance==null){
                    instance=new Singleton();
                }
            }
        }
        return instance;
    }
}
注解：只有当instance为null时，需要获取同步锁，创建一次实例。当实例被创建，则无需试图加锁。
缺点：用双重if判断，复杂，容易出错。

解法四：饿汉式（建议使用）

public class Singleton {
    private static Singleton instance=new Singleton();
    private Singleton(){
        
    }
    public static Singleton getInstance(){
        return instance;
    }
}
注解：初试化静态的instance创建一次。如果我们在Singleton类里面写一个静态的方法不需要创建实例，它仍然会早早的创建一次实例。而降低内存的使用率。

缺点：没有lazy loading的效果，从而降低内存的使用率。

解法五：静态内部内。（建议使用）

public class Singleton {
    private Singleton(){
        
    }
    private static class SingletonHolder{
        private final static Singleton instance=new Singleton();
    }
    public static Singleton getInstance(){
        return SingletonHolder.instance;
    }
}
注解：定义一个私有的内部类，在第一次用这个嵌套类时，会创建一个实例。而类型为SingletonHolder的类，只有在Singleton.getInstance()中调用，由于私有的属性，他人无法使用SingleHolder，不调用Singleton.getInstance()就不会创建实例。
优点：达到了lazy loading的效果，即按需创建实例。