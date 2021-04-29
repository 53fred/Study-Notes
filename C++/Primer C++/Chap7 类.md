# 1. 构造函数
- 构造函数在const对象构造过程中可以向其写值。
## 1.1. 构造函数初始值列表
```C++
Person::Person():name("abc"){}
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
