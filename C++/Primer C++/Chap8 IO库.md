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
- 缓冲刷新时，数据才真正写到设备或文件。
- 导致缓冲刷新的原因：  
  1. return；  
  2. 缓冲区满；  
  3. endl；  
  4. 设置unitbuf；（Flush buffer after insertions，每次写操作之后都进行一次flush）  
  5. 输出流关联到另一个流（cin和cerr关联到cout）。
- 刷新输出缓冲：
  1. endl：输出内容和一个换行符，刷新缓冲；
  2. flush：输出内容，不附加任何字符，刷新缓冲；
  3. ends：输出内容和一个空字符，刷新缓冲。
