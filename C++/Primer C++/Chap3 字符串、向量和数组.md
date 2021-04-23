# 1. string
## 1.1. 初始化
```C++
string s1;
string s2(s1);    //s2 is copy of s1
string s2 = s1;   //equal to upper one
string s3("abc"); //s3 is copy of "abc"
string s4(n, 'c');//s4 contains n * 'c'
```
- **直接初始化与拷贝初始化：**  
**拷贝初始化：** 使用等号进行初始化；  
**直接初始化：** 使用括号进行初始化。

## 1.2. 操作
```C++
getline(in, s); 
s.empty();      //return true if is empty
s.size();     
s[n];           
s1+s2           //append s2 to s1
```
- s.size()返回值为无符号的size_type类型，因此如果n为int型负值，size()<n会为true，因为n会被转换为无符号数。
- 字面值可以和string对象相加，此时**必须保证+运算符两侧至少一个是string** 。
```C++
string s4 = s1 + ",";   //true
string s5 = "a" + "b";  //false
string s6 = s4 + "a" + "b"; //true, s4 + "a" become string
string s7 = "a" + "b" + s4; //false
```  

- **字符串中删除字符：** str.erase();
```C++
erase(pos, n);  //erase n characters starting from pos
erase(position);  //erase one char at position, position need to be an iterator eg:str.begin()+i
erase(first, last)  //erase characters in between first and last. First and last both have to be iterator.
```

- **getline 与 cin 的区别：**  
对于string类的输入函数，它会**自动忽略开头的空白**（空格、制表符、换行等等），从第一个真正的字符开始直到下一个空白。cin不会丢弃换行符，**它会把换行符留在输入队列中**。  
对于getline()函数，它会**保存字符串中的空白符**，它读入数据，直到遇到换行符位置。getline读取换行符，并且将换行符替换成'\0'，**并且会丢弃换行符**。  

## 1.3. 函数
```C++
isalnum(c)  //num or alphabet
isalpha(c)  //alphabet
isdigit(c)  //num
isupper(c)  //caps
islower(c)  
tolower(c)
toupper(c)
```

# 2. vector
## 2.1. 初始化
```C++
vector<T> v1;
vector<T> v2(v1); //包含v1副本
vector<T> v2 = v1;
vector<T> v3(n,val); //包含n个重复元素，每个元素值是val
vector<T> v3(n); //包含n个执行了初始化的元素
vector<T> v4{a,b,c...}; //包含初始值个数的元素
vector<T> v4 = {a,b,c...}; //包含初始值个数的元素
```
- **区分vector<int> v1{10}与vector<string> v2{10}** ： 前者包含一个值为10的元素，后者包含10个默认值初始化的元素。
- **圆括号：** 构造vector；**花括号：** 进行列表初始化，如果不能初始化就按照圆括号进行构造。

## 2.2. 访问
- vector可以通过下标访问并赋值，但不能用下标形式添加元素。

## 2.3. 迭代器
```C++
vector<T> v1;
auto b = v1.begin(), e = v1.end();
```
- begin返回指向第一个元素的迭代器，end返回指向容器尾下一元素的迭代器，即一个不存在的尾后元素。如果容器为空，二者返回同一个迭代器。  
- 迭代器的运算：
```C++
*iter;
iter->mem;  //equal to (*iter).mem
++iter;
--iter;
iter1 == iter2;
iter1 != iter2;
```
- const_iterator：迭代器本身可以修改，但所指内容只能读，不能写。
- 在向vector中添加元素后，迭代器会失效，要重新设置。

# 3. 数组
## 3.1. 定义和初始化
- 数组的大小在编译时必须是已知的，即必须是常量表达式。
- 如果没有写明大小，会根据初始值的数量进行推断。eg: int i = {0,1,2};
- 字符串数组在使用列表进行初始化的时候没有空字符，使用字符串字面值进行初始化时结尾包含空字符。
## 3.2. 复杂数组声明
```C++
int *ptrs[10];  //an array of 10 pointers
int &refs[10]=... //ERROR!!array of reference
int (*P)[10] = &arr; //P point to an array of 10 pointers
int (&arrRef)[10] = arr;  //reference to an array of 10 pointers
```


