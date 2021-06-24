# 1. 动态内存与动态指针
## 1.1. shared_ptr类
- 两种智能指针：  
    shared_ptr：允许多个指针指向同一个对象；  
    unique_ptr：独占所指向的对象。  
weak_prt：弱引用，指向shared_ptr所管理的对象。  
- 默认初始化的智能指针中包含一个空指针。  

```C++
//shared_ptr和unique_ptr都支持的操作
shared_ptr<T> sp;
unique_ptr<T? up;
p                   //条件判断，若指向对象为true
*p                  //解引用p，获取所指对象
p->mem
p.get()             //返回p中保存的指针
swap(p,q)
p.swap(q)

//shared_ptr独有操作
make_shared<T>(args)    //返回一个shared_ptr，指向一个动态分配的类型为T的对象，使用args初始化该对象
shared_ptr<T>p(q)       //p是shared_ptr q的拷贝，此操作会递增q中的计数器。q中的指针必须能转化为T*
p = q                   //p和q都是shared_ptr，所保存的指针必须能相互转换。此操作会递减p的计数，递增q的计数。若p的引用计数变为0，则将其原内存释放。
p.use_count()           //返回与p共享对象的智能指针数量
p.unique()              //若p.use_count()为1，返回true；否则返回false
```

- make_shared函数：  
```C++
shared_ptr<int> sp = make_shared<int>(42);
shared_ptr<string> sp = make_shared<string>(10, '9');   //"9999999999"
shared_ptr<int> sp = make_shared<int>();                //0
```

- shared_ptr自动销毁所管理的对象，还会自动释放关联的内存。
- shared_ptr拷贝的情况下计数器会递增：  
1. 用shared_ptr初始化另一个shared_ptr；  
2. 将shared_ptr传递给一个函数作参数；  
3. 作为函数的返回值。  

- 计数器递减的情况：  
1. 给shared_ptr赋予一个新值（自身指向了另外一个地址，原来指向的地址已经没有了引用者，会自动释放）；  
2. 一个shared_ptr离开作用域会被销毁。  

## 1.2. 直接管理内存
- 用new分配const对象是合法的。
- 如果分配失败，会抛出bad_alloc。

## 1.3. shared_ptr和new结合使用
- 结合使用时，必须采用直接初始化的形式：
```C++
shared_ptr<int> p1 = new int(1024); //错误
shared_ptr<int> p2(new int(1024));  //正确
```
- **不应该使用内置指针来访问shared_ptr所管理的内存**。
- **不要用get()初始化另一个智能指针，会导致各自的计数器都为1，有可能对同一内存二次释放**。

## 1.4. unique_ptr类
- 同shared_ptr，必须直接初始化。

# 2. 动态数组
## 2.1. new和数组
- 可以new[0]，不可以解引用。
```C++
char arr[0];    //错误
char *cp = new char[0]; //正确
```
- delete指向数组的指针时，不加括号可能不会报错，但行为未定义。
- shared_ptr不支持动态数组。

## 2.2. allocator类
```C++
allocator<T> a;
a.allocate(n);  //分配一段原始的，未构造的内存，保存了n个类型为T的对象
a.deallocate(p,n);  //释放从T*指针p中地址开始的内存，这块内存保存了n个类型为T的对象。在调用deallocate之前，必须对这块内存中每个对象destroy
a.construct(p,args);//args被传递到类型为T的构造函数
a.destroy(p);   //对p指向的对象执行析构函数
```
