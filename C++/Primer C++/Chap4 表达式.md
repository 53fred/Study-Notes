# 1. 优先级
## 1.1. **优先级：算术 > 关系 > 逻辑**。
```C++
i!=j<k; //i!=(j<k)

int i = 1, j = 2, j = 3;
cout << (i - 1 == j > i + 1); //(i - 1) == (j > (i + 1)), output 1
```
- 赋值运算符优先级较低，需要给赋值表达式加括号：
```C++
while((i = getvalue()) != 42) //赋值表达式返回左值的引用
{
}

while(cin >> i) //cin >> i结束后返回cin对象
{
}
```
## 1.2. **混用解引用和其他运算符：**
```C++
auto pbeg = v.begin();
while (pbeg != v.end())
{
  cout << *pbeg++ << endl;  //++优先级高于*，故在输出解引用的值后迭代器++，而非所指内容++
}

*p.size();  //错误！*优先级低于.
```

```C++
ival++ && ival; //先判断ival，然后ival++，然后&&再判断ivalival
vec[ival++] <= vec[ival]; //执行顺序未定义，改为vec[ival] <= vec[ival + 1]
```

## 1.3. 常用运算符优先级：
1. ::
2. () [] -> . ++ --（后缀）
3. ! ~ + -（一元正负号） ++ --（前缀） * （解引用） &（取地址）sizeof new delete
4. / % *
5. +-
6. << >>（左移 右移）
7. < <= > >=
8. == !=
9. &&
10. ||
11. ?:
12. =（简单赋值）
13. += -= /= *=
14. ,  

```C++
somevalue ? ++x, ++y : --x, --y;  // 逗号优先级低，表达式变为(somevalue ? ++x, ++y : --x), --y;
```

# 2. sizeof运算符
- 对string对象或者vector对象执行sizeof运算，显示的是容器大小，与编译器有关，与容器内有多少数据无关。

# 3. 强制类型转换
```C++
cast-name<type>(expression);
```  
## 3.1. static_cast
- 可以大转小，用于基本类型的转换、non_const转换为const、空指针转换为目标类型指针。
```C++
double s = static_cast<double>(j);

void *p = &d;
double *dp = static_cast<double*>(p);
```  
## 3.2. const_cast
- 用于修改类型的const属性。一般用于强制消除对象的常量性。
- **如果对象是常量，执行写操作会产生未定义的后果**。

## 3.3. reinterpret_cast
- 简单的从一个指针的值到另一个指针的值的二进制拷贝。
- **与static_cast的区别**：static_cast完成相关类型的转换，reinterpret_cast完成不相关类型的转换，通常用作**不同类型指针间的转换**，因为指针的长度是一样的。
