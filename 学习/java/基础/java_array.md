##java list 去重
```
如果用Set ,倘若list里边的元素不是基本数据类型而是对象,
那么请覆写Object的boolean   equals(Object   obj)   和int   hashCode()方法.
return new ArrayList(new HashSet(list)); 
 
方法一：循环元素删除 
图片点击可在新窗口打开查看// 删除ArrayList中重复元素 
public static void removeDuplicate(List list) {  

   for ( int i = 0 ; i < list.size() - 1 ; i ++ ) {  
     for ( int j = list.size() - 1 ; j > i; j -- ) {  
       if (list.get(j).equals(list.get(i))) {  
         list.remove(j);  
       }   
      }   
    }   
    System.out.println(list);  
}   
 
方法二：通过HashSet剔除
图片点击可在新窗口打开查看// 删除ArrayList中重复元素 
public static void removeDuplicate(List list) {  

      HashSet h = new HashSet(list);  
      list.clear();  
      list.addAll(h);  
      System.out.println(list);  
}   
 
方法三： 删除ArrayList中重复元素，保持顺序
// 删除ArrayList中重复元素，保持顺序 
public static void removeDuplicateWithOrder(List list) {  

     Set set = new HashSet();  
      List newList = new ArrayList();  
   for (Iterator iter = list.iterator(); iter.hasNext();) {  
          Object element = iter.next();  
          if (set.add(element))  
             newList.add(element);  
       }   
      list.clear();  
      list.addAll(newList);  
     System.out.println( " remove duplicate " + list);  
}  
 
如果用HashSet的话，如果是对象，则要将对象实现equals和hashCode方法
``` ##将string[] 转为list
```
Vector<String>  skus  =  new  Vector<String>(Arrays. asList ( String[]  skuArray ));  
Vector<String>  skuInfo  =  new  Vector<String>();

for  (SkuInfoPo  sku  :  skuInfoList ) {
     skuInfo .add( sku .getSkuId() +  "" );
}
// 去除重复 skus .removeAll( skuInfo );
```