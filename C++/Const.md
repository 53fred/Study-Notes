# 常量
- const常量有数据类型，而宏常量没有数据类型。编译器可以对const常量进行类型安全检查，而对宏常量只进行字符替换，没有安全检查。
- const定义常量从汇编的角度看只是**给出了内存的对应地址**，而不是像#define一样给出立即数。因此const常量在程序运行过程中**只有一份拷贝**，而#define定义的常量在内存中有若干拷贝。
> TYPE const ValueName = value;  
> const TYPE ValueName = value;
```C++
Eg：const int Max = 100；
```

# 指针
看关键字const右边来确定什么被声明为常量 ，如果该关键字的右边是类型，则值是常量；如果关键字的右边是指针变量，则指针本身是常量。  
沿着 * 号划一条线：  
如果**const位于* 的左侧**，则const就是用来**修饰指针所指向的变量**，即指针指向为常量；  
如果**const位于* 的右侧**，const就是**修饰指针本身**，即指针本身是常量。  
## 1.指向整形常量的指针
- 指针指向的值是常量，不能修改。
- 可以将指针指向其他变量。
```C++
Eg：const int* pOne = &i;
//(*pOne)++; WRONG!
//pOne = &j; CORRECT!
```
## 2.指向整形的常量指针
- 指针本身是常量不可变，即它不能再指向别的变量，但指向（变量）的值可以修改。
```C++
Eg：int* const pTwo = &i;
//(*pTwo)++; CORRECT!
//pTwo = &j; WRONG!
```
## 3.指向整形常量的常量指针
- 既不能再指向别的常量，指向的值也不能修改。
```C++
Eg：const int* const pThree = &i;
```

# 函数
## 1.函数参数
### 1.1.传递过来的参数在函数内不可以改变(无意义，因为Var本身就是形参)
```C++
Eg：void function(const int Var);
```
### 1.2.参数指针所指内容为常量不可变
```C++
Eg：void function(const char* Var);
```
### 1.3.参数指针本身为常量不可变(也无意义，因为char* Var也是形参)
```C++
Eg：void function(char* const Var);
```

### 1.4.参数为引用，为了增加效率同时防止修改
- const引用传递和最普通的函数按值传递的**效果是一模一样的**,他禁止对引用的对象的一切修改,唯一不同的是按值传递会先建立一个类对象的副本, 然后传递过去,而它**直接传递地址**,所以这种传递比按值传递更有效。
- 对于**内部数据类型**的输入参数，**不要将“值传递”的方式改为“const 引用传递”**。否则既达不到提高效率的目的，又降低了函数的可理解性。
```C++
Eg：void function(const TYPE& Var); //引用参数在函数内为常量不可变
```
> 初始化 const引用时允许用任意表达式，只要该表达式的结果能转换为 引用类型即可。  
> 也就是说，允许为一个const引用 绑定 非常量对象、字面值、甚至是一般表达式。  

```C++
const int &ci = 3； //正确，整型字面值常量绑定到 const引用

int i = 1；
const int &cj = i;    //正确，非常量对象绑定到 const引用

const int i = 4；
const int &ck =  i; //正确，常量对象绑定到 const引用

const int i = 5；
int &r = i;    //错误，常量对象绑定到非const引用
```

## 2.函数返回值
### 2.1.修饰函数返回值（返回指针）
- 如果给以“指针传递”方式的函数返回值加 const 修饰，那么函数返回值（即指针）的内容不能被修改，该返回值**只能被赋给加const 修饰的同类型指针**。  
```C++
Eg：const char * GetString(void);
//char *str = GetString(); WRONG!
const char *str = GetString(); //CORRECT!
```

### 2.2.修饰“值传递方式”函数的返回值(无意义)
```C++
Eg：const int GetInt(void);
```

### 2.3.修饰“返回引用”函数的返回值
- 如果返回值不是内部数据类型，将函数MyClass GetObj(void) 改写为const Myclass & GetObj(void)的确能提高效率。但此时千万千万要小心，一定要搞清楚函数究竟是想返回一个对象的“拷贝”还是仅返回“别名”就可以了，否则程序会出错。
> 通常，不建议用const修饰函数的返回值类型为某个对象或对某个对象引用的情况。原因如下：如果返回值为某个对象为const（const A test = A 实例）或某个对象的引用为const（const A& test = A实例） ，则返回值具有const属性，则返回实例只能访问类A中的公有（保护）数据成员和const成员函数，并且不允许对其进行赋值操作，这在一般情况下很少用到。

# 面向对象
## 1.成员变量
- 表示成员常量，不能被修改。
- 类的const成员变量必须在**构造函数的参数初始化列表**中进行初始化。
- **构造函数内部，不能对const成员赋值**，编译器直接报错。
- 构造函数列表初始化**执行顺序与成员变量在类中声明相同**，**与初始化列表中语句书写先后无关**。  

**方法1：构造函数初始化列表方式(C++98)**
```C++
Eg：
class CExample
{
public:
	CExample():m_a(1),m_b(2){/*m_a = 1; compile error*/}
	CExample(const CExample&c):m_a(1){/*m_a = 1; compile error*/}
	~CExample(){}
private:
	const int m_a;
	int m_b;
};
```
**方法2：类内初始化(c++11支持)**
```C++
class Tag
{
public:
    const int a = 9;
};
```
- static const 数据成员变量初始化：
其一：const  修饰表示其为const成员，因而为类共享的数据在类内初始化提供了可能。
其二：static修饰表示其为类所有对象所共有，因而如果使用构造函数初始化列表的方式的话，每构造一个对象都会构造一个static数据成员变量，这并不合理。    

**方法1：**
```C++
class Test
{
public:
    const static int c; // 与static const int c;相同,可以在这里定义(如果以后在类中需要使用该变量的话).
}

const int Test：： c ＝ 0；//注意：给静态成员变量赋值时，不在需要加static修饰。但const要加。
```
**方法2：**
```C++
class Tag
{
public:
    static const int a = 9; // static和const顺序先后不影响
};
```

## 2.成员函数
- const（函数定义体）修饰类的成员函数时，表示在函数体中**成员变量不能改变，也不能调用类中任何非const成员函数**。
- 任何不会修改数据成员(即函数中的变量)的 函数都应该声明为const 类型。如果在编写const 成员函数时，不慎修改了数据成员，或者调用了其它非const 成员函数，编译器将指出错误，这无疑会提高程序的健壮性。
```C++
Eg：int ff(void) const;
```
## 3.类对象/对象指针/对象引用
-  const修饰类对象表示该对象为常量对象，**其中的任何成员都不能被修改**。对于对象指针和对象引用也是一样。
-  const修饰的对象，该对象的**任何非const成员函数都不能被调用**，因为任何非const成员函数会有修改成员变量的企图。
```C++
Eg：class AAA
{
void func1();
void func2() const;
}
const AAA aObj;
aObj.func1(); //WRONG!
aObj.func2(); //CORRECT!
```
# 转换
**将Const类型转化为非Const类型的方法：**
采用const_cast 进行转换。  
**用法：** const_cast (expression)
- 常量指针被转化成非常量指针，并且仍然指向原来的对象；
- 常量引用被转换成非常量引用，并且仍然指向原来的对象；
- 常量对象被转换成非常量对象。
