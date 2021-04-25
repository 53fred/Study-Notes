# 1. 优先级
- 优先级：算术 > 关系 > 逻辑。
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
- **混用解引用和递增：**
```C++
auto pbeg = v.begin();
while (pbeg != v.end())
{
  cout << *pbeg++ << endl;  //++优先级高于*，故在输出解引用的值后迭代器++，而非所指内容++
}
```
