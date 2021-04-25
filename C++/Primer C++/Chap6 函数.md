# 1. 局部静态变量
```C++
size_t calls()
{
  static size_t ctr = 0;  //第一次执行ctr=0，此后ctr递增，再进入函数不会进行初始化。
  return ++ctr;
}
```

# 2. 参数传递
# 2.1. 常量引用
- 使用常量引用避免拷贝。
- 尽量使用常量引用，可以接收更多参数类型：const可以接收非const，但非const不可以接收const。
```C++
bool isshorter(const string &s1, const string &s2);
```
# 2.2. const形参和实参
- const与否不属于重载。
```C++
void func(int i);
void func(const int i); //ERROR!
```
# 2.3. 数组引用形参
```C++
void func(int (&arr)[10]);  //
```
