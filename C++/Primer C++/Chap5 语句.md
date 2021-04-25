**1. Case**
```C++
switch(index)
{
  case 1,2,3,4,5: //WRONG!
  case 1:case 2:case 3: //CORRECT
}
```
**2. do while**
- 至少执行一次。  
**3. try catch**
```C++
try{
  int i = 0;
  throw i;
}catch(int i)
{
  cout << i << endl;
}
```
