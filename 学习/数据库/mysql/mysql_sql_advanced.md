[TOC]
##查询是否有记录
```
 select if((SELECT 1 FROM information_schema.`TABLES` where TABLE_SCHEMA = '数据库名' and TABLE_NAME = '表名'), 1, 0); 

```
##sql 分析 
```
各位，我请教个问题。现在有一张表，对应这四个字段,我想返回的是另外一张表的

gc_name，请问一条语句返回这4个列，该怎么弄啊?
用了case when不知道为什么返回null
不是行转列，就是返回4列。但是是返回gc_name

 select c1.gc_name,
       c2.gc_name,
       c3.gc_name,
       c4.gc_name from ch_goods g
left join ch_goods_class c1 on g.gc_id = c1.gc_id
left join ch_goods_class c2 on g.gc_id_1 = c2.gc_id
left join ch_goods_class c3 on g.gc_id_2 = c3.gc_id
left join ch_goods_class c4 on g.gc_id_3 = c4.gc_id;
 
```
##sql列转行
```
CREATE TABLE `test_score` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(50) DEFAULT NULL,
  `chinese_score` int(11) DEFAULT NULL,
  `maths_score` int(11) DEFAULT NULL,
  `english_score` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8

INSERT INTO test_score(`name`,chinese_score,maths_score,english_score) VALUES  ('张三',60,65,82),('李四',52,85,68),('王五',88,69,89),('allen',96,82,69);

要求：查询每个学生的单科最大科目，以及对应的成绩。

已知如下：
表数据：
    id  name    chinese_score  maths_score  english_score  
------  ------  -------------  -----------  ---------------
     1  张三                 60           65               82
     2  李四                 52           85               68
     3  王五                 88           69               89
     4  allen              96           82               69

1.求出所有人的语文成绩:
SELECT id,NAME,'chinese_score' SUBJECT, chinese_score 'score' FROM test_score

2.求出所有人的所有成绩再排序
SELECT id,NAME,'chinese_score' SUBJECT, chinese_score 'score' FROM test_score
UNION ALL
SELECT id,NAME,'maths_score' SUBJECT, maths_score 'score' FROM test_score
UNION ALL
SELECT id,NAME,'english_score' SUBJECT,english_score 'score' FROM test_score
ORDER BY id,NAME,score DESC;

    id  name    subject         score  
------  ------  -------------  --------
     1  张三      english_score        82
     1  张三      maths_score          65
     1  张三      chinese_score        60
     2  李四      maths_score          85
     2  李四      english_score        68
     2  李四      chinese_score        52
     3  王五      english_score        89
     3  王五      chinese_score        88
     3  王五      maths_score          69
     4  allen   chinese_score        96
     4  allen   maths_score          82
     4  allen   english_score        69

3.按照每人分组，并求出最大值
SELECT id,NAME,SUBJECT,MAX(score)fscore
FROM
(
SELECT id,NAME,'chinese_score' SUBJECT, chinese_score 'score' FROM test_score
UNION ALL
SELECT id,NAME,'maths_score' SUBJECT, maths_score 'score' FROM test_score
UNION ALL
SELECT id,NAME,'english_score' SUBJECT,english_score 'score' FROM test_score
ORDER BY id,NAME,score DESC
) b GROUP BY id,NAME ORDER BY fscore DESC;

    id  name    subject        fscore  
------  ------  -------------  --------
     4  allen   chinese_score        96
     3  王五      english_score        89
     2  李四      maths_score          85
     1  张三      english_score        82

```
##mysql 根据条件更新记录
```
UPDATE sales SET product = CASE WHEN product='tnt1' THEN 'apple'
WHEN product='tnt2' THEN 'banana'
WHEN product='tnt3' THEN 'orange'
ELSE product
END;
```