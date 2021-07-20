# 1. 基本概念
- 重载运算符名字由关键字operator和其后要定义的运算符号共同组成。重载运算符也包含返回类型、参数列表以及函数体。
- 重载运算符参数数量：一元运算符有一个参数，二元运算符有两个。
- 如果一个运算符函数是成员函数，则它的第一个运算对象绑定到隐式的this指针上。因此，成员运算符函数的显式参数数量比运算符的运算对象总数少一个。
- 对于一个运算符函数，它或者是类的成员，或者有至少一个类类型的参数，**不能为内置类型重定义运算符**。  
- 可以直接调用一个重载的运算符函数：  

```C++
data1 + data2;
operator+(data1, data2);    //等价的函数调用

//调用成员运算符函数：
data1 += data2;
data1.operator+=(data2);
```
## 1.1. 选择作为成员或者非成员
- 赋值(=)、下标([])、调用(())和成员访问运算符(->)必须是成员；
- 复合赋值运算符一般是成员；
- 改变对象状态的运算符，如递增、递减和解引用，通常是成员；
- 具有对称性的运算符，通常是非成员。

# 2. 输入和输出运算符
## 2.1. 重载输出运算符
- 通常情况下，输出运算符的第一个形参是一个非常量ostream对象的引用。非常量：向流写入内容会改变其状态；引用：因为无法直接复制一个ostream对象。
- 第二个形参一般是一个常量的引用，该常量是要打印的类类型。常量：打印通常不改变对象内容；引用：避免复制实参。
- operator<<一般要返回它的ostream形参。  

```C++
ostream &operator<<(ostream &os, const Sales_item& item)
{
    os << item.isbn();
    return os;
}
```
- 输出运算符尽量减少格式化操作：输出运算符应该主要负责打印对象的内容，而非控制格式。
- 输入输出运算符**必须是非成员函数**，一般被声明为友元。

## 2.2. 重载输入运算符
- 通常情况下，输入运算符的第一个形参是运算符将要读取的流的引用。
- 第二个形参是将要读入到的对象的引用。
- 通常返回给定流的引用。  

```C++
istream &operator>>(istream &is, Sales_item& item)
{
    is << item.isbn();
    if (is)
    {
        //success
    }
    else
    {
        //error
    }
    return is;
}
```
- **输入运算符必须处理输入可能错误的情况，输出运算符不需要**。

### 2.2.1. 输入时可能的错误
- 流含有错误类型的数据；
- 读取操作到达文件的末尾或者遇到输入流的其他错误。

# 3. 算术和关系运算符
- 通常情况下，把算术和关系运算符定义成非成员函数以允许对左侧或右侧的运算对象进行转换。因为这些运算符不需要改变运算对象的状态，所以形参都是常量的引用。
- 应当使用复合赋值来定义算数运算符：  

```C++
Sales_Data operator+(const Sales_Data &lhs, const Sales_Data &rhs)
{
    Sales_Data sum = lhs;
    sum += rhs;
    return sum;
}

Sales_Data& operator+=(const Sales_Data &lhs)
{
    no += lhs.no;
    return *this;
}  
```  

## 3.1. 相等运算符
- 如果一个类定义了operator==，则也应该定义operator!=。
- 相等运算符和不等运算符应该把工作委托给另外一个，意味着其中一个运算符应该负责实际比较对象的工作，另一个运算符只是调用那个真正工作的运算符。

## 3.2. 关系运算符
- 如果存在唯一一种逻辑可靠的<定义，则应该考虑为这个类定义<运算符。如果类同时还包含==，则当且仅当<的定义和==产生的结果一致时才定义<运算符。

# 4. 下标运算符
- 表示容器的类一般会定义下标运算符operator[]，下标运算符必须是成员函数。
- 下标运算符通常以所访问元素的引用作为返回值，这样下标运算符可以出现在赋值运算符的任意一端。
- 最好同时定义下标运算符的常量和非常量版本。  

```C++
class StrVec{
public:
    string& operator[](size_t n)
    {return elements[n];}
    const string& operator[](size_t n) const
    {return elements[n];}
private:
    string *elements;
}
```

# 5. 递增和递减运算符
- 定义递增和递减运算符应该同时定义前置版本和后置版本。
- 区分前置和后置运算符：**后置版本接收一个额外的（不被使用的）int形参**。当我们使用后置运算符时，编译器为这个形参提供一个值为0的实参。
- 为了与内置版本一致，后置运算符应该返回对象的原值（递增或递减之前的值），返回的形式是一个值而非引用。

```C++
class StrBlobPtr{
public:
    StrBlobPtr& operator++();   //前置运算符
    StrBlobPtr& operator--();
    StrBlobPtr operator++(int); //后置运算符
    StrBlobPtr operator--(int);
private:
    //若检查成功，check返回一个指向vector的shared_ptr
    shared_ptr<vector<string>> check(size_t i, const string& msg) const
    {
        auto ret = wptr.lock(); //所指底层vector是否还存在？返回shared_ptr对象
        if (!ret)
            throw runtime_error("");
        if (i >= ret->size())
            throw out_of_range(msg);
        return ret;     //否则，返回指向vector的shared_ptr
    }
    weak_ptr<vector<string>> wptr;
    size_t curr;
};

StrBlobPtr& StrBlobPtr::operator++(){
    ++cur;
    return *this;
}

StrBlobPtr StrBlobPtr::operator++(int){
    StrBlobPtr ret = *this;
    ++*this;
    return ret;
}
```

- 可以显示地调用后置运算符：

```C++
StrBlobPtr p(a1);
p.operator++(0);    //后置
p.operator++();     //前置
```

# 6. 成员访问运算符
- 解引用运算符和箭头运算符：  

```C++
class StrBlobPtr{
public:
    string& operator*() const{
        auto p = check(curr, "");   //p: shared_ptr<vector<string>>
        return (*p)[curr];
    }
    string* operator->() const{
        return &this->operator*();
    }
};
```

- 将这两个运算符定义为const成员，因为获取一个元素并不会改变对象的状态。

## 6.1. 箭头运算符
- 对于形如point->mem的表达式来说，point必须是**指向类对象的指针或者是重载了operator->的类的对象**。根据point类型的不同，point->mem分别等价于：  
<1> (*point).mem                            //point是一个指针  
<2>point.operator()->mem                    //point是类的一个对象

- **重载箭头操作符必须返回指向类类型的指针，或者返回定义了自己的箭头操作符的类类型对象**。  

- <1>如果返回类型是**指针**，则内置箭头操作符可用于该指针，编译器**对该指针解引用并从结果对象获取指定成员**。执行相当于(*(p.operator->()).mem的操作。如果被指向的类型没有定义那个成员，则编译器产生一个错误。
- <2>如果返回类型是**类类型的其他对象**（或是这种对象的引用），则将**递归应用该操作符**。编译器检查返回对象所属类型是否具有成员箭头，如果有，就应用那个操作符；否则，编译器产生一个错误。这个过程继续下去，**直到返回一个指向带有指定成员的的对象的指针**，或者返回某些其他值，在后一种情况下，代码出错。操作相当于(*(p.operator->().operator->())).mem.

- 实例：  

```C++
#include <iostream>
using namespace std;

class A {
public:
	A() { i = 1; }
	void fun() {
		cout << "fun in class A!" << endl;
	}
public:
	int i;
};

class B {
	A a;
public:
	B() { i = 2; }
	A* operator->() {           //重载箭头运算符的函数居然返回的是指针，也就是对象的地址
		return &a;
	}
	void fun() {
		cout << "fun in class B!" << endl;
	}
private:
	int i;
};

class C {
	B b;
public:
    C() { i = 3; }
	B operator->() {             //重载箭头运算符的函数返回的是对象
		return b;
	}
	void fun() {
        cout << "fun in class C!" << endl;
	}
private:
	int i;
};

int main()
{
	C* p = new C();     //p是类C的一个指针
	p->fun();
	C c;               //c是类C的对象
	c->fun();
	int i = c->i;
	cout << i << endl;
	system("pause");
	return 0;
}

```

- 输出结果：

```
fun in class C!
fun in class A!
1
```

- 分析：  
<1>p是指针，按照上述说明一，应该调用的是(* p).fun()，这样结果就是“fun in class C!”    
<2>c是定义了operator的类的一个对象，按照上述说明一，应该调用c.operator()->fun，接着类C的重载箭头运算符的成员函数返回值类型是类类型的其它对象，那么按照说明二，应该递归应用该操作符（b.operator()->fun），那么现在来到了类B的重载箭头运算符的成员函数，此成员函数的返回值类型为指针，所以按照说明二，编译器对该指针解引用并从结果对象获取指定成员函数fun()（即(*a).fun()），由于在类B的重载箭头运算符函数中返回的是类A的指针，所以调用类A的成员函数fun(),结果就是“fun in class A!”

# 7. 函数调用运算符

```C++
struct absint{
    int operator()(int val) const{
        return val < 0 ? -val : val;
    }
}

int i = -42;
absint obj;
int iu = obj(i);
```
- 如果类定义了函数调用运算符，则类的对象称为函数对象。
- 函数对象常常作为泛型算法的实参：

```C++
for_each(vs.begin(), vs.end(), PrintString(cerr, '\n'));
```

## 7.1. lambda是函数对象
```C++
Eg：
stable_sort(words.begin(), words.end(), [](const string &a, const string &b){return a.size() < b.size();});

//其行为类似下面这个类的未命名对象：
class ShorterString{
public:
    bool operator()(const string &a, const string &b) const
    {
        return a.size() < b.size();
    }
};
```
