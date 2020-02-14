# 华为C++机试
----------------------
# String
### &emsp;string 与 int 转换  
- ### int 转 string
      to_string()
      string = int + ""
- ### string 转 int
### string类型的字符串长度获取的三种方法
### 输入含空格的字符串
  - scanf("%[a-z A-Z0-9]",str)  
  - getline()（包含头文件</string/>）
      - getline(cin, str)
  - cin.get (char *str, int maxnum)*
  - cin.getline (char *str, int maxnum)*
-------------------
# New的使用
### &emsp;使用new建立动态数组
        '''
          int num;
          cin>>num;
          int *index = new int[num];*
        '''
------------------
# STL
### reverse()
      reverse(beg,end)
      reverse_copy(sourceBeg,sourceEnd,destBeg)
      reverse()会将区间[beg,end)内的元素全部逆序；
      reverse_copy()会将源区间[sourceBeg,sourceEnd)内的元素复制到"以destBeg起始的目标区间"，并在复制过程中颠倒安置次序.
