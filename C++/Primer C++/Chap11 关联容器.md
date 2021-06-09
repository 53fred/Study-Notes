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
# 2. 关联容器操作
- 关联容器的额外类型别名:
```C++
key_type:容器类型的关键字类型
mapped_type:每个关键字关联的类型,只适用于map
value_type:对于set,与key_type相同;
            对于map,为pair<const key_type, mapped_type>.
```
- map的迭代器返回一个pair类型,其中first成员保存const关键字,second成员保存值.**可以修改值,不可以修改关键字**.
- set的迭代器是const的,可以读取,不可以修改.

## 2.1. 添加操作
- map:
```C++
word_count.insert({word,1});
word_count.insert(make_pair(word,1));
word_count.insert(pair<string,size_t>(word,1));
word_count.insert(map<string,size_t>::value_type(word,1));
```
- 关联容器insert操作
```C++
c.insert(v);    //v是一个value_type类型的对象
c.emplace(args);//args用来构造一个元素

c.insert(b,e);  //b和e是迭代器
c.insert(il);   //il是一个花括号包围的元素值列表

c.insert(p,v);  //p作为一个提示,指出从哪里开始搜索新元素应该存储的位置
c.emplace(p,args);
```
- insert返回一个pair,告诉我们插入操作是否成功.**first成员是一个迭代器,指向具有给定关键字的元素;second成员是一个bool值,指出元素是插入成功还是已经存在**.

## 2.2. 删除元素
- erase接受一个key_type元素,删除所有匹配元素,返回删除元素数量.
```C++
c.erase(k); //删除关键字为k的元素    
c.erase(p); //p是迭代器
c.erase(b,e); //删除两个迭代器之间的元素
```

## 2.3. map的下标操作
- set不支持下标,因为set没有与关键字相关联的值.
- 如果关键字不在map中,会为它创建一个元素并插入到map中.
- 由于下标运算符可能插入一个新的元素,我们只可以对非const的map进行下标操作.
```C++
c[k];   //返回关键字为k的元素
c.at(k);//访问关键字为k的元素,若不存在,抛出异常
```
- **对map进行下标操作时,返回一个mapped_type对象;当解引用一个map迭代器时,返回一个value_type对象**.
- map的下标运算符**返回一个左值,既可以读也可以写**.
- **下标与insert的区别：如果重复出现多次同样的key值 ，用下标操作后面的value会替换前面的value，而insert则不会替换**。

## 2.4. 访问元素
```C++
iset.find(1);   //返回一个迭代器，指向key=1的元素
iset.find(11);  //返回一个迭代器，指向iset.end()
iset.count(1);  //返回1
iset.count(11); //返回0

c.lower_bound(k);   //返回一个迭代器，指向第一个关键字不小于k的元素
c.upper_bound(k);   //返回一个迭代器，指向第一个关键字大于k的元素
//lower_bound和upper_bound找不到关键词的话，会指向一个不影响排序的关键词插入位置
c.equal_range(k);   //返回一个迭代器pair，表示关键字等于k的元素的范围。若k不存在，两个成员均等于c.end()
```
- 对map使用find代替下标操作。

# 3. 无序容器
- 无序容器在存储上组织为一组桶，每个桶保存零个或多个元素。无序容器使用一个哈希函数将元素映射到桶。
- 无序容器管理操作：
```C++
//桶接口
c.bucket_count();       //正在使用的桶的数目
c.max_bucket_count();   //容器能容纳的最多的桶的数量
c.bucket_size(n);       //第n个桶中有多少元素
c.bucket(k);            //关键字为k的元素在哪个桶中

//桶迭代
local_iterator          //可以用来访问桶中元素的迭代器类型
const_local_iterator
c.begin(n), c.end(n)    //桶n的首元素迭代器和尾后迭代器
c.cbegin(n), c.cend(n)    //返回const迭代器

//哈希策略
c.load_factor();        //每个桶的平均元素数量，返回float
c.max_load_factor();    //c试图维护的平均桶大小，返回float。c会在需要时添加新的桶，以使得load_factor<=max_load_factor
c.rehash(n);            //重组存储，使得bucket_count>=n且bucket_count>size/max_load_factor
c.reserve(n);           //重组存储，使得c可以保存n个元素且不需要rehash
```
