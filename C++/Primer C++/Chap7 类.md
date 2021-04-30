# 1. 构造函数
- 构造函数在const对象构造过程中可以向其写值。
## 1.1. 构造函数初始值列表
- 初始值列表比显示赋值更好。如果显示赋值，在执行之前会先进行默认初始化。
- 初始化const或者引用类型的数据成员的**唯一机会就是通过构造函数初始值**，也即通过初始值列表，不可以通过显示赋值来进行初始化。
```C++
Person::Person():name("abc"){}
Person::Person(){name="abc";}

class cc
{
public:
	cc(int ii);
private:
	int i;
	const int i2;
	int &i3;
}

cc::cc(int ii)		//WRONG!
{
	i = ii;
	i2 = ii;	//ERROR!
	i3 = i;		//ERROR!
}

cc::cc(int ii):i(ii),i2(ii),i3(i){}	//CORRECT!
```
- 构造函数初始值列表只说明用于初始化成员的值，而**不限定初始化的具体执行顺序**。**成员的初始化顺序与它们在类定义中的出现顺序一致**。
- 应尽量避免用某些数据成员初始化其他数据成员。
## 1.2. 委托构造函数（C++11）
```C++
class cc
{
public:
	cc(int i1,int i2,int i3):i(i1),i2(i2),i3(i3){}	//非委托构造函数
	//其余构造函数全部委托给另一个构造函数
	cc(): cc(1,2,3){}
	cc(int i0):cc(i0,2,3){}
private:
	int i;
	const int i2;
	int i3;
}
```

# 2. class与struct
- 使用class和struct定义类的唯一区别就是默认访问权限。
- struct：定义在第一个访问说明符之前的成员是public的。
- class：定义在第一个访问说明符之前的成员是private的。
# 3. 友元
```C++
class Point
{
public:
	friend int CalculateArea(Point& p1, Point& p2); //申明此函数为友元函数，此函数在外部能访问类的私有成员
}
```
# 4. 名字查找与类的作用域
```C++
typedef double Money;
string bal;
class Account
{
public:
	Money balance(){return bal;}
private:
	Money bal;
}
```
- 编译器看到balance的语句时，会在Account类的作用域内寻找Money的声明。由于没有找到，会在外层作用域中继续查找。在先找到typedef double Money后，该类型会被用作balance的返回类型以及数据成员bal的类型。
- 类型名的定义通常出现在类的开始处，确保所有使用该类型的成员都出现在类名的定义之后。
# 5. explicit
- explicit关键字**只能用于修饰只有一个参数的类构造函数**, 它的作用是表明该构造函数是显示的, 而非隐式的, 跟它相对应的另一个关键字是implicit, 意思是隐藏的,类构造函数默认情况下即声明为implicit(隐式).
- implicit：
```C++
class CxString  // 没有使用explicit关键字的类声明, 即默认为隐式声明  
{  
public:  
    char *_pstr;  
    int _size;  
    CxString(int size)  
    {  
        _size = size;                // string的预设大小  
        _pstr = malloc(size + 1);    // 分配string的内存  
        memset(_pstr, 0, size + 1);  
    }  
    CxString(const char *p)  
    {  
        int size = strlen(p);  
        _pstr = malloc(size + 1);    // 分配string的内存  
        strcpy(_pstr, p);            // 复制字符串  
        _size = strlen(_pstr);  
    }  
    // 析构函数这里不讨论, 省略...  
};  
  
CxString string1(24);     // 这样是OK的, 为CxString预分配24字节的大小的内存  
CxString string2 = 10;    // 这样是OK的, 为CxString预分配10字节的大小的内存  
CxString string3;         // 这样是不行的, 因为没有默认构造函数, 错误为: “CxString”: 没有合适的默认构造函数可用  
CxString string4("aaaa"); // 这样是OK的  
CxString string5 = "bbb"; // 这样也是OK的, 调用的是CxString(const char *p)  
CxString string6 = 'c';   // 这样也是OK的, 其实调用的是CxString(int size), 且size等于'c'的ascii码  
string1 = 2;              // 这样也是OK的, 为CxString预分配2字节的大小的内存  
string2 = 3;              // 这样也是OK的, 为CxString预分配3字节的大小的内存  
string3 = string1;        // 这样也是OK的, 至少编译是没问题的, 但是如果析构函数里用free释放_pstr内存指针的时候可能会报错, 完整的代码必须重载运算符"=", 并在其中处理内存释放 

//在C++中, 如果的构造函数只有一个参数时, 那么在编译的时候就会有一个缺省的转换操作:将该构造函数对应数据类型的数据转换为该类对象
CxString string2 = 10;
//等价于
CxString string2(10);  
//或  
CxString temp(10);  
CxString string2 = temp;  
```
- explicit：
```C++
class CxString  // 使用关键字explicit的类声明, 显示转换  
{  
public:  
    char *_pstr;  
    int _size;  
    explicit CxString(int size)  
    {  
        _size = size;  
        // 代码同上, 省略...  
    }  
    CxString(const char *p)  
    {  
        // 代码同上, 省略...  
    }  
};  
  
    CxString string1(24);     // 这样是OK的  
    CxString string2 = 10;    // 这样是不行的, 因为explicit关键字取消了隐式转换  
    CxString string3;         // 这样是不行的, 因为没有默认构造函数  
    CxString string4("aaaa"); // 这样是OK的  
    CxString string5 = "bbb"; // 这样也是OK的, 调用的是CxString(const char *p)  
    CxString string6 = 'c';   // 这样是不行的, 其实调用的是CxString(int size), 且size等于'c'的ascii码, 但explicit关键字取消了隐式转换  
    string1 = 2;              // 这样也是不行的, 因为取消了隐式转换  
    string2 = 3;              // 这样也是不行的, 因为取消了隐式转换  
    string3 = string1;        // 这样也是不行的, 因为取消了隐式转换, 除非类实现操作符"="的重载  
```
- 只能在类内声明构造函数时使用explicit，在类外部定义时不应重复。
- 如果类构造函数参数大于或等于两个时, 是不会产生隐式转换的, 所以explicit关键字也就无效了。
- 但是, 也有一个例外, 就是当除了第一个参数以外的其他参数都有默认值的时候, explicit关键字依然有效, 此时, 当调用构造函数时只传入一个参数, 等效于只有一个参数的类构造函数。
```C++
class CxString  // 使用关键字explicit声明  
{  
public:  
    int _age;  
    int _size;  
    explicit CxString(int age, int size = 0)  
    {  
        _age = age;  
        _size = size;  
        // 代码同上, 省略...  
    }  
    CxString(const char *p)  
    {  
        // 代码同上, 省略...  
    }  
};  

    CxString string1(24);     // 这样是OK的  
    CxString string2 = 10;    // 这样是不行的, 因为explicit关键字取消了隐式转换  
    CxString string3;         // 这样是不行的, 因为没有默认构造函数  
    string1 = 2;              // 这样也是不行的, 因为取消了隐式转换  
    string2 = 3;              // 这样也是不行的, 因为取消了隐式转换  
    string3 = string1;        // 这样也是不行的, 因为取消了隐式转换, 除非类实现操作符"="的重载  
```

# 6. 聚合类
- 所有成员都是public的。
- 没有构造函数。
- 没有类内初始值。
- 没有基类，也没有virtual函数。
```C++
struct Data{
	int val;
	string s;
}

Data val1 = {0, "ANNA"};
```

# 7. 字面值常量类
- 数据成员都是字面值类型的聚合类。
- 必须至少含有一个constexpr构造函数。
- 必须使用析构函数的默认定义。
