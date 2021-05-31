主要关联容器：map、set。
map：关联数组。
set：关键字简单集合。
# 1. 概述
- 容器的主要用法：
map：存储字典型数据  
set：坏值检验，只有关键字的好处  
list：任意位置任意删除添加数据  
deque：信息处理，只在头部  
vector：相关联数据，顺序处理  

- multimap、multiset：允许多个元素具有相同关键字。  

- list与set的区别：
    list：排列顺序与插入顺序一致，元素可以重复；  
    set：不保证顺序，元素不可重复。  
- 有序容器：map、set，关键字类型必须定义元素比较的方法。

## 1.1. pair类型
定义在头文件utility中.
```C++
pair<string, string> anon;  //保存两个string
pair<string, string> author{"a", "bc"};
```
- pair的默认构造函数对数据成员进行值初始化.
- pair的数据成员是public的,两个成员分别命名为first和second.

```C++
cout << author.first << author.second;
```
- pair上的操作:
```C++
pair<T1, T2> p;
pair<T1, T2> p(v1, v2);
pair<T1, T2> p = {v1, v2};
make_pair(v1, v2)   //返回一个用v1和v2初始化的pair,pair的类型从v1和v2推断出来.
p.first
p.second
p1 relop p2         //关系运算符(<,>,<=,>=)按字典序定义
p1 == p2
p1 != p2
```
