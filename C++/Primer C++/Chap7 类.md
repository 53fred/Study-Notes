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
