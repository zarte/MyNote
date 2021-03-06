## 大O算法复杂度速查表
```
http://bigocheatsheet.com/
```
##算法复杂度
###数据结构操作

数据结构    时间复杂度   空间复杂度
平均  最差  最差
访问  搜索  插入  删除  访问  搜索  插入  删除  
Array   O(1)   O(n)    O(n)    O(n)    O(1)    O(n)    O(n)    O(n)    O(n)
Stack   O(n)   O(n)    O(1)    O(1)    O(n)    O(n)    O(1)    O(1)    O(n)
Singly-Linked List  O(n)    O(n)    O(1)    O(1)    O(n)    O(n)    O(1)    O(1)    O(n)
Doubly-Linked List  O(n)    O(n)    O(1)    O(1)    O(n)    O(n)    O(1)    O(1)    O(n)
Skip List   O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(n)    O(n)    O(n)    O(n)    O(n log(n))
Hash Table  -   O(1)    O(1)    O(1)    -   O(n)    O(n)    O(n)    O(n)
Binary Search Tree  O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(n)    O(n)    O(n)    O(n)    O(n)
Cartesian Tree  -   O(log(n))   O(log(n))   O(log(n))   -   O(n)    O(n)    O(n)    O(n)
B-Tree  O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(n)
Red-Black Tree  O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(n)
Splay Tree  -   O(log(n))   O(log(n))   O(log(n))   -   O(log(n))   O(log(n))   O(log(n))   O(n)
AVL Tree    O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(log(n))   O(n)
###数组排序算法
算法  时间复杂度   空间复杂度
最佳  平均  最差  最差
Quicksort   O(n log(n)) O(n log(n)) O(n^2)  O(log(n))
Mergesort   O(n log(n)) O(n log(n)) O(n log(n)) O(n)
Timsort O(n)    O(n log(n)) O(n log(n)) O(n)
Heapsort    O(n log(n)) O(n log(n)) O(n log(n)) O(1)
Bubble Sort O(n)    O(n^2)  O(n^2)  O(1)
Insertion Sort  O(n)    O(n^2)  O(n^2)  O(1)
Selection Sort  O(n^2)  O(n^2)  O(n^2)  O(1)
Shell Sort  O(n)    O((nlog(n))^2)  O((nlog(n))^2)  O(1)
Bucket Sort O(n+k)  O(n+k)  O(n^2)  O(n)
Radix Sort  O(nk)   O(nk)   O(nk)   O(n+k)
###图操作
节点 / 边界管理   存储  增加顶点    增加边界    移除顶点    移除边界    查询
Adjacency list  O(|V|+|E|)  O(1)    O(1)    O(|V| + |E|)    O(|E|)  O(|V|)
Incidence list  O(|V|+|E|)  O(1)    O(1)    O(|E|)  O(|E|)  O(|E|)
Adjacency matrix    O(|V|^2)    O(|V|^2)    O(1)    O(|V|^2)    O(1)    O(1)
Incidence matrix    O(|V| ⋅ |E|)    O(|V| ⋅ |E|)    O(|V| ⋅ |E|)    O(|V| ⋅ |E|)    O(|V| ⋅ |E|)    O(|E|)
###堆操作
类型  时间复杂度
Heapify 查找最大值   分离最大值   提升键 插入  删除  合并
Linked List (sorted)    -   O(1)    O(1)    O(n)    O(n)    O(1)    O(m+n)
Linked List (unsorted)  -   O(n)    O(n)    O(1)    O(1)    O(1)    O(1)
Binary Heap O(n)   O(1) (log(n))   O(log(n))   O(log(n))   O(log(n))   O(m+n)
Binomial Heap   -   O(1)  O(log(n))   O(log(n))   O(1)    O(log(n))   O(log(n))
Fibonacci Heap  -   O(1)  O(log(n))   O(1)    O(1)    O(log(n))   O(1)
