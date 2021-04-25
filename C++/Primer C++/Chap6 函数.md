# 1. 局部静态变量
```C++
size_t calls()
{
  static size_t ctr = 0;  //第一次执行ctr=0，此后ctr递增，再进入函数不会进行初始化。
  return ++ctr;
}
```

# 2. 参数传递
## 2.1. 常量引用
- 使用常量引用避免拷贝。
- 尽量使用常量引用，可以接收更多参数类型：const可以接收非const，但非const不可以接收const。
```C++
bool isshorter(const string &s1, const string &s2);
```
## 2.2. const形参和实参
- const与否不属于重载。
```C++
void func(int i);
void func(const int i); //ERROR!
```
## 2.3. 数组引用形参
```C++
void func(int (&arr)[10]);  //下标[]优先级比&高，不加括号会变成引用的数组

void swap(int *&a, int*&b); //不加引用的话，可以改变*a与*b的值，不可以改变指针所指地址。也不可以写作int &*a，会变成指向引用的指针。
```

## 2.4. argc与argv
- **可选的实参从argv[1]开始**，argv[0]保存程序的名字，而非用户输入。
- **不加参数的话argc为1**.

## 2.5. 含有可变形参的函数
### 2.5.1. initializer_list(C++ 11)
### 2.5.2. 省略符形参
- 只能放在参数列表最后。
```C++
void func(param_list,...);  
```

# 3. 返回值
## 3.1. 引用返回左值
- 引用作为返回值可以做左值。
```C++
char &getval(string &str);
getval("abc") = 'A';
```
## 3.2. 返回数组指针
- 声明数组/数组指针的几种方法：
```C++
1. typedef int arrT[10];  //类型别名，表示的类型是含有10个整型的数组

2. using arrT = int[10];  //等价声明
 
3. int *p[10];  //含有10个指针的数组

4. int (*p2)[10] = &arr;  //一个指针，指向含有10个整型的数组
```
- 如果想定义一个返回数组指针的函数，数组的维度必须跟在函数名后。形参列表也跟在数组名后，且应先于数组维度。
```C++
Type(*function(param_list))[dimension];

eg: int (*func(int i))[10];
//func(int i)：表示调用函数时需要int型实参。
//(*func(int i))：表示可以对函数结果进行解引用。
//(*func(int i))[10]：表示解引用函数的结果会得到大小是10的数组。
//int (*func(int i))[10]：表示数组中的元素是int型。
```
