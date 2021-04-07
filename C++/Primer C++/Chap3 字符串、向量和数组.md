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

- **getline 与 cin 的区别：**  
对于string类的输入函数，它会**自动忽略开头的空白**（空格、制表符、换行等等），从第一个真正的字符开始直到下一个空白。  
对于getline()函数，它会**保存字符串中的空白符**，它读入数据，直到遇到换行符位置。
