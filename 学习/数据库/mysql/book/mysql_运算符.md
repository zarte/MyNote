##算术运算符
```
mod(a,b)等同于 a%b
= 等于, <=>安全的等于
<> 或 != 不等于
REGEXP或RLIKE 正则表达式匹配
```

##常用函数
```
1.concat(s1,s2,...sn):将传入的参数连接成一个字符串
select concat('aa','bb','cc') ,concat('aaa',NULL)
aabbcc     NULL
```