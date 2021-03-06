[TOC]
##分页offset过大，Sql优化经验
```
1.offset过大导致查询很慢，如下sql：

SELECT
*
FROM table
where condition1 = 0
and condition2 = 0
and condition3 = -1
and condition4 = -1
order by id asc
LIMIT 2000 OFFSET 50000
这样查询会很慢但如果改为


SELECT
*
FROM table
JOIN
(select id from table
where condition1 = 0
and condition2 = 0
and condition3 = -1
and condition4 = -1
order by id asc
LIMIT 2000 OFFSET 50000)
as tmp using(id)
这样就会快很多，数据量大的时候，至少可以提升5倍以上的效率
``` ##慢查询注意谁是驱动表
```
DBA告诉我们：
    MySQL 表关联的算法是 Nest Loop Join，是通过驱动表的结果集作为循环基础数据，然后一条一条地通过该结果集中的数据作为过滤条件到下一个表中查询数据，然后合并结果。
 
EXPLAIN 结果中，第一行出现的表就是驱动表（Important!）
 
以上两个查询语句，驱动表都是 city，如上面的执行计划所示！
 
对驱动表可以直接排序，对非驱动表（的字段排序）需要对循环查询的合并结果（临时表）进行排序（Important!）
因此，order by ads.id desc 时，就要先 using temporary 了！
 
驱动表的定义
wwh999 在 2006年总结说，当进行多表连接查询时， [驱动表] 的定义为：
1）指定了联接条件时，满足查询条件的记录行数少的表为[驱动表]；
2）未指定联接条件时，行数少的表为[驱动表]（Important!）。
 
忠告：如果你搞不清楚该让谁做驱动表、谁 join 谁，请让 MySQL 运行时自行判断
既然“未指定联接条件时，行数少的表为[驱动表]”了，
而且你也对自己写出的复杂的 Nested Loop Join 不太有把握（如下面的实例所示），
就别指定谁 left/right join 谁了，
请交给 MySQL优化器 运行时决定吧。
如果您对自己特别有信心，可以像火丁一样做优化。
 
小结果集驱动大结果集
de.cel 在2012年总结说，不管是你，还是 MySQL，
优化的目标是尽可能减少JOIN中Nested Loop的循环次数，
以此保证：
永远用小结果集驱动大结果集（Important!）！
——实例讲解——
 
Nested Loop Join慢查SQL语句
先了解一下 mb 表有 千万级记录，mbei 表要少得多。慢查实例如下：
explain
SELECT mb.id, ……
FROMmb LEFT JOIN mbei ON mb.id=mbei.mb_id INNER JOINu ON mb.uid=u.uid  
WHERE 1=1  
ORDER BY mbei.apply_time DESC
limit 0,10
够复杂吧。Nested Loop Join 就是这样，
以驱动表的结果集作为循环的基础数据，然后将结果集中的数据作为过滤条件一条条地到下一个表中查询数据，最后合并结果；此时还有第三个表，则将前两个表的 Join 结果集作为循环基础数据，再一次通过循环查询条件到第三个表中查询数据，如此反复。
这条语句的执行计划如下：
    id  select_type  table   type    possible_keys   key             key_len  ref                     rows  Extra                                       
------  -----------  ------  ------  --------------  --------------  -------  -------------------  -------  --------------------------------------------
     1  SIMPLE       mb      index   userid          userid          4        (NULL)               6060455  Using index; Using temporary; Using filesort
     1  SIMPLE       mbei    eq_ref  mb_id  mb_id  4        mb.id             1                                              
     1  SIMPLE       u       eq_ref  PRIMARY         PRIMARY         4        mb.uid        1  Using index                                
由于动用了“LEFT JOIN”，所以攻城狮已经指定了驱动表，虽然这张驱动表的结果集记录数达到百万级！
.
.
如何优化？
优化第一步：LEFT JOIN改为JOIN
干嘛要 left join 啊？直接 join！
explain
SELECT mb.id…… 
FROM mb JOIN mbei ON mb.id=mbei.mb_id INNER JOINu ON mb.uid=u.uid  
WHERE 1=1  
ORDER BY mbei.apply_time DESC
limit 0,10
立竿见影，驱动表立刻变为小表 mbei 了， Using temporary 消失了，影响行数少多了：
    id  select_type  table   type    possible_keys   key      key_len  ref                             rows  Extra         
------  -----------  ------  ------  --------------  -------  -------  ----------------------------  ------  --------------
     1  SIMPLE       mbei    ALL     mb_id  (NULL)   (NULL)   (NULL)                         13383  Using filesort
     1  SIMPLE       mb      eq_ref  PRIMARY,userid  PRIMARY  4        mbei.mb_id       1                
     1  SIMPLE       u       eq_ref  PRIMARY         PRIMARY  4        mb.uid                1  Using index  


优化第一步之分支1：根据驱动表的字段排序，好吗？
left join不变。干嘛要根据非驱动表的字段排序呢？我们前面说过“对驱动表可以直接排序，对非驱动表（的字段排序）需要对循环查询的合并结果（临时表）进行排序！”的。
explain
SELECT mb.id…… 
FROM mb LEFT JOIN mbei ON mb.id=mbei.mb_id INNER JOINu ON mb.uid=u.uid  
WHERE 1=1  
ORDER BY mb.id DESC
limit 0,10
也满足业务场景，做到了rows最小：
    id  select_type  table   type    possible_keys   key             key_len  ref                    rows  Extra      
------  -----------  ------  ------  --------------  --------------  -------  -------------------  ------  -----------
     1  SIMPLE       mb      index   userid          PRIMARY         4        (NULL)                   10             
     1  SIMPLE       mbei    eq_ref  mb_id  mb_id  4        mb.id            1  Using index
     1  SIMPLE       u       eq_ref  PRIMARY         PRIMARY         4        mb.uid       1  Using index
 


优化第二步：去除所有JOIN，让MySQL自行决定！
写这么多密密麻麻的 left join/inner join 很开心吗？
explain
SELECT mb.id…… 
FROM mb,mbei,u   
WHERE 
    mb.id=mbei.mb_id
    and mb.uid=u.user_id
order by mbei.apply_time desc
limit 0,10
立竿见影，驱动表一样是小表 mbei：
    id  select_type  table   type    possible_keys   key      key_len  ref                             rows  Extra         
------  -----------  ------  ------  --------------  -------  -------  ----------------------------  ------  --------------
     1  SIMPLE       mbei    ALL     mb_id  (NULL)   (NULL)   (NULL)                         13388  Using filesort
     1  SIMPLE       mb      eq_ref  PRIMARY,userid  PRIMARY  4        mbei.mb_id       1                
     1  SIMPLE       u       eq_ref  PRIMARY         PRIMARY  4        mb.uid                1  Using index  


最后的总结：
强调再强调：
不要过于相信你的运气！
不要相信你的开发环境里SQL的执行速度！
请拿起 explain 武器，
如果你看到以下现象，请优化：
出现了Using temporary；
rows过多，或者几乎是全表的记录数；
key 是 (NULL)；
possible_keys 出现过多（待选）索引。
```

##联表查询注意谁是驱动表
```
[慢查优化]联表查询注意谁是驱动表 & 你搞不清楚谁join谁更好时请放手让mysql自行判定

写在前面的话：

   不要求每个人一定理解 联表查询 (join/left join/inner join 等 ) 时的 mysql 运算过程；

    不要求每个人一定知道线上 （现在或未来） 哪张表数据量大，哪张表数据量小；

      但把 mysql 客户端（如 SQLyog ，如 HeidiSQL ）放在桌面上， 时不时拿出来 explain 一把，这是一种美德 ！ 在实例讲解之前，我们先回顾一下联表查询的基础知识。

——联表查询的基础知识——

引子：为什么第一个查询using temporary，第二个查询不用临时表呢？

下面两个查询，它们只差了一个order by，效果却迥然不同。

第一个查询：
EXPLAIN extended
SELECT ads.id
FROM ads , city 
WHERE
   city.city_id = 8005
   AND ads.status = 'online'
   AND city.ads_id=ads.id
ORDER BY   ads.id   desc
执行计划为：

    id  select_type  table   type    possible_keys   key      key_len  ref                     rows  filtered  Extra                          
------  -----------  ------  ------  --------------  -------  -------  --------------------  ------  --------  -------------------------------
     1  SIMPLE       city    ref     ads_id,city_id  city_id  4        const                   2838    100.00  Using temporary ; Using filesort
     1  SIMPLE       ads     eq_ref  PRIMARY         PRIMARY  4        city.ads_id       1    100.00  Using where                   


第二个查询：
EXPLAIN extended
SELECT ads.id
FROM ads, city 
WHERE
   city.city_id =8005
   AND ads.status = 'online'
   AND city.ads_id=ads.id
ORDER BY   city.ads_id   desc
执行计划里没有了using temporary：
    id  select_type  table   type    possible_keys   key      key_len  ref                     rows  filtered  Extra                      
------  -----------  ------  ------  --------------  -------  -------  --------------------  ------  --------  ---------------------------
     1  SIMPLE       city    ref     ads_id,city_id  city_id  4        const                   2838    100.00  Using where; Using filesort
     1  SIMPLE       ads    eq_ref  PRIMARY         PRIMARY  4        city.ads_id       1    100.00  Using where               
为什么？
 
DBA告诉我们：
    MySQL 表关联的算法是 Nest Loop Join，是通过驱动表的结果集作为循环基础数据，然后一条一条地通过该结果集中的数据作为过滤条件到下一个表中查询数据，然后合并结果。
 
EXPLAIN 结果中，第一行出现的表就是驱动表（Important!）
 
以上两个查询语句，驱动表都是 city，如上面的执行计划所示！
 
对驱动表可以直接排序 ， 对非驱动表（的字段排序）需要对循环查询的合并结果（临时表）进行排序 （Important!）
因此，order by ads.id desc 时，就要先 using temporary 了！
 
驱动表的定义
wwh999   在 2006年总结说，当进行多表连接查询时，   [驱动表]   的定义为：
1）指定了联接条件时， 满足查询条件的记录行数少 的表为[驱动表]；
2） 未指定联接条件时， 行数少 的表为[驱动表] （Important!）。
 
忠告：如果你搞不清楚该让谁做驱动表、谁 join 谁，请让 MySQL 运行时自行判断
既然“未指定联接条件时， 行数少 的表为[驱动表]”了，
而且你也对自己写出的复杂的 Nested Loop Join 不太有把握（如下面的实例所示），
就别指定谁 left/right join 谁了，
请交给 MySQL优化器 运行时决定吧。
如果您对自己特别有信心，可以 像火丁一样做优化 。
 
小结果集驱动大结果集
de.cel   在2012年总结说，不管是你，还是 MySQL，
优化的目标是尽可能减少JOIN中Nested Loop的循环次数，
以此保证：
永远用小结果集驱动大结果集 （Important!） ！
——实例讲解——
 
Nested Loop Join慢查SQL语句
先了解一下 mb 表有 千万级记录，mbei 表要少得多。慢查实例如下：
explain
SELECT mb.id, ……
FROMmb   LEFT JOIN mbei ON mb.id=mbei.mb_id   INNER JOIN u ON mb.uid=u.uid  
WHERE 1=1  
ORDER BY mbei.apply_time DESC
limit 0,10
够复杂吧。Nested Loop Join 就是这样，
以驱动表的结果集作为循环的基础数据，然后将结果集中的数据作为过滤条件一条条地到下一个表中查询数据，最后合并结果；此时还有第三个表，则将前两个表的 Join 结果集作为循环基础数据，再一次通过循环查询条件到第三个表中查询数据，如此反复。
这条语句的执行计划如下：
    id  select_type  table   type    possible_keys   key             key_len  ref                     rows  Extra                                       
------  -----------  ------  ------  --------------  --------------  -------  -------------------  -------  --------------------------------------------
     1  SIMPLE       mb      index   userid          userid          4        (NULL)               6060455  Using index; Using temporary; Using filesort
     1  SIMPLE       mbei    eq_ref  mb_id  mb_id  4        mb.id             1                                              
     1  SIMPLE       u       eq_ref  PRIMARY         PRIMARY         4        mb.uid        1  Using index                                
由于动用了“LEFT JOIN”，所以攻城狮已经指定了驱动表，虽然这张驱动表的结果集记录数达到百万级！

如何优化？
优化第一步：LEFT JOIN改为JOIN
干嘛要 left join 啊？直接 join！
explain
SELECT mb.id……

FROM mb  JOIN mbei ON mb.id=mbei.mb_id  INNER JOIN u ON mb.uid=u.uid  
WHERE 1=1  
ORDER BY mbei.apply_time DESC
limit 0,10
立竿见影，驱动表立刻变为小表 mbei 了， Using temporary 消失了，影响行数少多了：
    id  select_type  table   type    possible_keys   key      key_len  ref                             rows  Extra         
------  -----------  ------  ------  --------------  -------  -------  ----------------------------  ------  --------------
     1  SIMPLE       mbei    ALL     mb_id  (NULL)   (NULL)   (NULL)                         13383  Using filesort
     1  SIMPLE       mb      eq_ref  PRIMARY,userid  PRIMARY  4        mbei.mb_id       1                
     1  SIMPLE       u       eq_ref  PRIMARY         PRIMARY  4        mb.uid                1  Using index  

优化第一步之分支1：根据驱动表的字段排序，好吗？
left join不变。干嘛要根据非驱动表的字段排序呢？我们前面说过“对驱动表可以直接排序，对非驱动表（的字段排序）需要对循环查询的合并结果（临时表）进行排序！”的。

explain
SELECT mb.id……

FROM mb LEFT JOIN mbei ON mb.id=mbei.mb_id  INNER JOIN u ON mb.uid=u.uid  
WHERE 1=1  
ORDER BY  mb.id DESC
limit 0,10
也满足业务场景，做到了rows最小：
    id  select_type  table   type    possible_keys   key             key_len  ref                    rows  Extra      
------  -----------  ------  ------  --------------  --------------  -------  -------------------  ------  -----------
     1  SIMPLE       mb      index   userid          PRIMARY         4        (NULL)                   10             
     1  SIMPLE       mbei    eq_ref  mb_id  mb_id  4        mb.id            1  Using index
     1  SIMPLE       u       eq_ref  PRIMARY         PRIMARY         4        mb.uid       1  Using index
 

优化第二步：去除所有JOIN，让MySQL自行决定！
写这么多密密麻麻的 left join/inner join 很开心吗？

explain
SELECT mb.id……
FROM mb,mbei,u   
WHERE
    mb.id=mbei.mb_id
    and mb.uid=u.user_id
order by mbei.apply_time desc
limit 0,10 立竿见影，驱动表一样是小表 mbei：
    id  select_type  table   type    possible_keys   key      key_len  ref                             rows  Extra         
------  -----------  ------  ------  --------------  -------  -------  ----------------------------  ------  --------------
     1  SIMPLE       mbei    ALL     mb_id  (NULL)   (NULL)   (NULL)                         13388  Using filesort
     1  SIMPLE       mb      eq_ref  PRIMARY,userid  PRIMARY  4        mbei.mb_id       1                
     1  SIMPLE       u       eq_ref  PRIMARY         PRIMARY  4        mb.uid                1  Using index  

最后的总结：
强调再强调：
不要过于相信你的运气！
不要相信你的开发环境里SQL的执行速度！
请拿起 explain 武器，
如果你看到以下现象，请优化：

出现了Using temporary；
rows过多，或者几乎是全表的记录数；
key 是 (NULL)；
possible_keys 出现过多（待选）索引。

 
记住，explain 是一种美德！
 
 

参考资源：
1）wwh999，2006， 进行多表查时的排序问题,其多表查询时的原理论证！   ；
2）de.cel，2012， MySQL中的Join 原理及优化思路   ；
3）火丁，2013， MySQL优化的奇技淫巧之STRAIGHT_JOIN ；

```