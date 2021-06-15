# 1. 动态内存与动态指针
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

```
