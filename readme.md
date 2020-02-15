# 华为C++机试
# String 
## 1.string 与 int 转换  
- ### int 转 string

```c
to_string()
string = int + ""
```
- ### string 转 int
```c
 1.
 int num = atoi(str.c_str());  //首字符为数字则返回数字，非数字则返回0，atoi的参数必须为const char*
 2.
 int num = str-'0';
```
## 2.string类型的字符串长度获取的三种方法
```c
size(),length(),strlen()
```
## 3.输入含空格的字符串

  - scanf("%[a-z A-Z0-9]",str)  
  - getline()（包含头文件</string/>）
      - getline(cin, str)
  - cin.get (char *str, int maxnum)
  - cin.getline (char *str, int maxnum)
  - 
## 4.string划分与删除字符
- 划分字符串（按';'）
```c
  string str;
  cin>>str;
  int n = str.size();
  for (int i = 0; i < n; ++i){
      if (str[i] == ';'){
          str[i] = ' ';
      }
  }
  istringstream out(str);  //初始化out为str
  string mov[n];
  int temp  = 0;
  while (out >> mov[temp])
      temp++;
``` 
- 删除某个字符
```c
  string str;
  string target;
  int pos = str.find(target);
  n = target.size();
  str = str.erase(pos,n);   //从pos这个位置开始，删除n个字符
```
# New的使用
### &emsp;使用new建立动态数组
```c
  int num;
  cin>>num;
  int *index = new int[num];
```
------------------
# STL
## 1. 迭代器
```c
vector<int>::iterator iter=ivec.bengin();  //将迭代器iter初始化为指向ivec容器的第一个元素
``` 
## 2. vector

## 3. map

## 4. reverse()
  ```c
  reverse(beg,end);
  reverse_copy(sourceBeg,sourceEnd,destBeg);
  ```

    reverse()会将区间[beg,end)内的元素全部逆序；
    reverse_copy()会将源区间[sourceBeg,sourceEnd)内的元素复制到"以destBeg起始的目标区间"，并在复制过程中颠倒安置次序.
-------------------

# 经典问题
## 01背包问题  
