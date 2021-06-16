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

## 1.2. 直接管理内存
