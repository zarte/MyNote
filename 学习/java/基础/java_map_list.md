##map 有序
```

最近的项目需要读取对照表放在MAP里面;
开始用TreeMap发现顺序不对 换成 HashMap顺序也不对;
LinkedHashMap 才能保证迭代的时候取出的顺序和存入的顺序相同
```
##比较 map

```
有两个map,分别为map1和map2，其中map1中部分key是和map2中的相同，如何遍历这两个map，并把这map1中和map2匹配的选出来?
匿名 | 浏览 7363 次  2012-02-10 20:15
2012-02-10 20:38 最佳答案
Map map1 = new HashMap();
Map map2 = new HashMap();
map1.put("a", "aa");
map1.put("b", "bb");
map1.put("c", "cc");
map2.put("1", "11");
map2.put("b", "22");
map2.put("3", "33");


Iterator it = map1.keySet().iterator();
while(it.hasNext()){
Object key = it.next();
if(map2.containsKey(key)){
System.out.println(map1.get(key));
System.out.println(map2.get(key));
}
}
```




##比较list
```
List<String> lt1 = new ArrayList<String>();
  lt1.add("ab");
  lt1.add("bb");
  lt1.add("cc");
List<String> lt2 =  new ArrayList<String>();
  lt2.add("ab");
  lt2.add("cc");
  lt2.add("dd");
  for(String kk:lt1){
  if(lt2.contains(kk)){
  System.out.println(kk);
  }
```
##StringBuffer清空
```
StringBuffer-setLength:63

StringBuffer--delete:109
StringBuffer--new StringBuffer:78
结论：



    要通过使用sbi.setLength(0);来清空StringBuffer对象中的内容效率最高。
```