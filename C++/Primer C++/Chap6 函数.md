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
## 2.2. 数组引用形参
```C++
void func(int (&arr)[10]);  //下标[]优先级比&高，不加括号会变成引用的数组

void swap(int *&a, int*&b); //不加引用的话，可以改变*a与*b的值，不可以改变指针所指地址。也不可以写作int &*a，会变成指向引用的指针。
```

## 2.3. argc与argv
- **可选的实参从argv[1]开始**，argv[0]保存程序的名字，而非用户输入。
- **不加参数的话argc为1**.

## 2.4. 含有可变形参的函数
### 2.4.1. initializer_list(C++ 11)
### 2.4.2. 省略符形参
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

# 4. 函数重载
- 形参需不同，不允许仅有返回类型不同。
- **顶层const不属于重载**。
```C++
Record lookup(Phone);
Record lookup(const Phone); //ERROR!

Record lookup(Phone*);
Record lookup(Phone* const); //顶层const,ERROR!

Record lookup(Phone&);
Record lookup(const Phone&); //CORRECT

Record lookup(Phone*);
Record lookup(const Phone*); //底层const,CORRECT!
```

# 5. 默认实参
- **一旦某个形参被赋予默认值，后面的所有形参都要有默认值**。
```C++
int func(int a, int b, char c = 'A');
```
- 默认实参负责填补函数调用时缺少的尾部实参。
```C++
func(0,1);
```

# 6. constexpr(C++ 11)
- 常量表达式，编译时直接返回表达式结果，函数的返回值和形参类型必须是字面值类型，并且函数体内有且只有一条return。

# 7. 调试
## 7.1. assert(expr)
- 如果表达式为0，输出信息并终止程序的执行。
## 7.2. NDEBUG
- 如果定义了NDEBUG，assert什么也不做。

# 8. 函数指针 
```C++
bool (* pf) (const string&, const string&); //括号必须要加，不然会变成返回值bool*的函数
pf = lengthcompare; //pf指向名为lengthcompare的函数

bool b1 = pf("a", "ab");  //直接调用lengthcompare
bool b2 = (*pf)("a", "ab");  //等价
```

