# 1. 顺序容器
## 1.1. 类型
1. vector：可变大小数组，支持随机访问。在尾部之外的地方插入/删除元素可能很慢。
2. deque：双端队列，支持随机访问。在头尾位置插入/删除速度很快。
3. list：双向链表，只支持双向顺序访问。在list中任何位置插入/删除都很快。
4. forward_list：单向链表，只支持单向顺序访问。在任何位置插入/删除都很快。
5. array：固定大小数组。支持快速随机访问。不能添加或删除元素。
6. string：与vector相似。  

- 只有array固定大小。
- string和vector将元素保存在连续的内存空间，由于元素连续存储，用下标直接计算地址是非常快速的。但是在元素的中间位置添加/删除元素会非常耗时，因为之后的所有元素都需要进行移动。
- list和forward_list在访问时只能遍历整个容器。
- C++程序应该使用标准库容器，而不是更原始的数据结构，比如内置数组。
## 1.2. 确定使用哪种顺序容器
- 通常，使用vector是最好的选择。
- 如果要求随机访问，应使用vector或deque。
- 如果要在容器的中间插入/删除元素，使用list。
- 如果要在头尾插入/删除元素，使用deque。

# 2. 容器库
## 2.1. begin和end成员
- begin和end有多个版本，带r的版本返回反向迭代器，以c开头的版本返回const迭代器。
```C++
list<string> a;
auto it1 = a.begin();   //list<string>::iterator
auto it2 = a.rbegin();  //list<string>::reverse_iterator
auto it3 = a.cbegin();  //list<string>::const_iterator
auto it4 = a.crbegin(); //list<string>::const_reverse_iterator
```
- 以c开头的版本是C++新标准引入的，老版本只能显示声明使用哪种迭代器。
## 2.2. 定义和初始化
```C++
C c;            //默认构造函数
C c1(c2);       //c1初始化为c2的拷贝。对于array类型，c1c2长度需要相等。
C c1 = c2;      //同上
C c{a,b,c...};  //c初始化为列表中元素的拷贝。对于array类型，列表元素个数必须小于等于array大小。
C c(b,e);       //c初始化为迭代器b和e
//以下为大小参数，只有顺序容器（不包括array）才能接受
C seq(n);       //seq包含n个元素，每个元素进行值初始化
C seq(n,t);     //seq包含n个初始化为值t的元素
```
## 2.3. 赋值和初始化
- 赋值运算后，容器大小与右值相同。
- 由于右边运算对象的大小可能与容器大小不同，array不允许assign，也不允许用花括号包围的值列表进行赋值。
```C++
c1 = c2;
c = {a,b,c...};
swap(c1,c2);    //交换元素。swap通常比拷贝元素快得多。
c1.swap(c2);    //同上
//assign适用于顺序容器（不包括array）
seq.assign(b,e);//将seq中元素替换为迭代器范围内元素。迭代器不能指向seq中的元素。
seq.assign(il);//将seq中元素替换为初始化列表il中的元素。
seq.assign(n,t);//将seq中元素替换为n个值为t的元素。
```
## 2.4. 容器大小操作
- size
- empty
- max_size
## 2.5. 关系运算符
- 比较两个容器实际上是对容器中的元素逐对比较，比较方式与string的比较方式类似。

# 3. 顺序容器操作
## 3.1. 添加元素
- push_back(t)/emplace_back(args)：emplace_back会在容器管理的内存空间中直接创建对象。push_back则会创建一个局部临时对象，并将其压入容器中。
- push_front(t)/emplace_front(args)
- insert(p,t)/emplace(p,args)：在迭代器p指向的元素之前创建一个值为t或由args创建的元素。返回指向新添加的元素的迭代器。
- insert(p,n,t)：在迭代器p指向的元素之前插入n个值为t的元素。返回指向新添加的第一个元素的迭代器；若n为0，返回p。
- insert(p,b,e)：在迭代器p指向的元素之前插入迭代器b和e范围内的元素。返回指向新添加的第一个元素的迭代器；若n为0，返回p。
- insert(p,il)：il是一个花括号包围的元素值列表。在迭代器p指向的元素之前插入il。返回指向新添加的第一个元素的迭代器；若n为0，返回p。
## 3.2. 访问元素
- 每个顺序容器都有一个front函数，除forward_list外都有back函数。这两个操作分别返回首元素和尾元素的引用。
- 在调用front和back之前，要保证容器非空。
## 3.3. 删除元素
- pop_back()：删除尾元素，返回void。  
- pop_front()：删除首元素，返回void。
- erase(p)：删除迭代器p所指元素，返回被删元素之后元素的迭代器。
- erase(b,e)：删除迭代器区间内的元素，返回被删元素之后元素的迭代器。
- clear()：删除所有元素，返回void。
## 3.4. 改变容器大小
- 可以用resize来增大或减小容器。array不支持resize。
```C++
list<int> ilist(10,42);     //10个int，每个值为42
ilist.resize(15);           //将5个值为0的元素添加到末尾
ilist.resize(25,-1);        //将10个值为-1的元素添加到末尾
ilist.resize(5);            //从末尾删除20个元素
```
