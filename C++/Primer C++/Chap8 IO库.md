# 1. IO类
- IO类定义在三个头文件中：  
  iostream定义了基于读写流的基本类型  
  fstream定义了读写命名文件的类型  
  sstream定义了读写内存string对象的类型  
- IO对象不能拷贝和赋值。
# 1.1. 条件状态
```C++
while(cin>>temp)  //遇到EOF后cin置为错误，跳出循环

cin.clear();  //将cin置为正常状态
```
# 1.2. 输出缓冲
