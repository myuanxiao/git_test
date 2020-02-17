# 华为C++机试
# **String** 
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
  str = str.erase(pos,n);  //从pos这个位置开始删除n个字符，后边的字符自动向前覆盖
```
# **New的使用**
### &emsp;使用new建立动态数组
```c
  int num;
  cin>>num;
  int *index = new int[num];
```
------------------
# **STL**
## ***1. 迭代器***
```c
vector<int>::iterator iter=ivec.bengin();  //将迭代器iter初始化为指向ivec容器的第一个元素
``` 
## ***[2. vector][vector_adress]***
  [vector_adress]: https://blog.csdn.net/qq_30534935/article/details/82353068
 ### 2.1 创建vector对象
  ```c
    vector<int> vec;  //声明一个int型向量
    vector<int> vec(8); //声明一个初始大小为8的int型向量
    vector<int> vec(10, -1) //声明一个初始大小为10且值都是-1的int型向量
  ```
  ### 2.2 vector赋值与修改
  ```c
  //赋值
    vec.assign(v1.begin(), v1.begin()+8);  //将动态数组v1的[v1.begin,v1.begin()+8)赋值给动态数组v2
    //修改
    auto &val = vec.back(); //val为指向最后一个元素的引用  
    val = 2;  //vector需要使用引用修改，不可以使用数组下标直接修改
  ```
  ### 2.3 vector访问
  ```c
    //通过下标访问、修改向量元素，但不可以通过下标添加元素
    for(int i = 0; i < vec.size(); i++){
        cout << vec[i];
    }
    //通过迭代器访问数组
    for(vector<int>::iterator iter = vec.begin(); iter != vec.end(); iter++){
        cout << *iter;
    }
    //通过指针访问数组
    int *p = vec.data();
    for (int i = 0; i < 5; i++){
      cout << *p++;
    }
  ```
  ### 2.4 添加元素
  ```c
    vec.push_back(p);  //在数组vec的末尾添加元素p
    vec.insert(v.it,p);  //在数组vec.it指向位置插入元素p
    vec.insert(v.it,n,p);  //在数组vec.it指向位置插入n个元素p
  ```
  ### 2.5 删除元素
  ```c
    v.pop_back();  //删除数组v末尾的元素
    v.erase(v.it);  //删除数组v.it指向位置的元素
  ```
  ### 2.6 反转reverse()
  ```c
  reverse(beg,end);
  reverse_copy(sourceBeg,sourceEnd,destBeg);
  //reverse()会将区间[beg,end)内的元素全部逆序；
  //reverse_copy()会将源区间[sourceBeg,sourceEnd)内的元素复制到
  //"以destBeg起始的目标区间"，并在复制过程中颠倒安置次序.
  ```
  ### 2.7 排序
  ```c
    vector<int> v{ 0, 3, 5, 7, 8, 1, 4, 9, 2, 6};
    //默认情况下升序排列
    sort(v.begin(), v.end()); // v{ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
    //可自定义排序方式（此为降序）
    bool Comp(const int &a, const int &b){
      return a>b;
    }
    sort(v.begin(), v.end(), Comp); // v{ 9, 8, 7, 6, 5, 4, 3, 2, 1, 0}
    //亦可函数内部自定义 
    sort(v.begin(), v.end(), [](int &a, int &b){return a>b;});  // v{ 9, 8, 7, 6, 5, 4, 3, 2, 1, 0}
  ```
## ***3. map***
  ### 3.1 pair类型的定义和初始化
    pair类型包含了两个数据值，通常有以下的一些定义和初始化的一些方法：

    pair<T1, T2> p; ： 定义了一个空的pair对象p，T1和T2的成员都进行了值初始化
    pair<T1, T2> p(v1, v2); ： p是一个成员类型为T1和T2的pair; first和second成员分别用v1和v2进行初始化。
    pair<T1, T2> p = {v1, v2} ：等价于p(v1, v2)
    make_pair(v1, v2) ： 以v1和v2值创建的一个新的pair对象
  ### 3.2 创建map对象
    map是键-值对的组合，即map的元素是pair，其有以下的一些定义的方法：

    map<k, v> m;	： 定义了一个名为m的空的map对象
    map<k, v> m2(m); ： 创建了m的副本m2
    map<k, v> m3(m.begin(), m.end()); ： 创建了map对象m3，并且存储迭代器范围内的所有元素的副本
 ### 3.3 map元素访问 
    - mymap['a'] = "an element";
    - mymap.at('a') = "an element";

  ### 3.4 map中元素的插入
  ```c
  - 3.4.1 使用下标[]插入
    mymap[0] = 'a';
  - 3.4.2 使用insert()插入元素
    // （1）插入单个值
    mymap.insert(std::pair<char, int>('a', 100));
    // （2）指定位置插入
    std::map<char, int>::iterator it = mymap.begin();
    mymap.insert(it, std::pair<char, int>('b', 300));
    // （3）范围多值插入
    std::map<char, int> anothermap;
    anothermap.insert(mymap.begin(), mymap.find('c'));
  ```
  ### 3.5 
  ```c
    // erase() 删除元素 
    mymap.erase(0);          	 // （1）删除key为0的元素
    mymap.erase(mymap.begin());  // （2）删除迭代器指向的位置元素
    // 查找关键字1在容器map中出现的次数，如果不存在则为0
    mymap.count(1);
    // find()若存在，返回指向该key的迭代器
    // 若不存在，则返回迭代器的尾指针，即 mymap.end()，即 -1
    map<int, int>::iterator it_find;
    it_find = mp.find(0);
    //equal_range() 返回一个迭代器pair，表示关键字 == k的元素的范围。
    //若k不存在，pair的两个成员均等于c.end()
    //lower_bound()/upper_bound()函数需要加载头文件#include<algorithm>,
    //其基本用途是查找有序区间中第一个大于或等于/大于某给定值的元素的位置，
    //其中排序规则可以通过二元关系来表示。
    //rbegin() 返回一个指向map尾部的逆向迭代器
    //rend() 返回一个指向map头部的逆向迭代器
    //key_comp() 比较key_type值大小
    //value_comp() 比较value_type值大小
  ```
  ### 3.6 两种遍历方式
  ```c
      for (map<int, char>::iterator iter = mymap.begin(); iter != mymap.end(); iter++)
      cout << iter->first << " ==> " << iter->second << endl;
  ```
  ```c
      for (auto& x : mymap) {
          std::cout << x.first << ": " << x.second << '\n';
      }
  ```
  ### 3.7 map排序
  #### 3.7.1 map转vector
  ```c
  vector<pair<int, float> > map_vec(map.begin(), map.end());
  ```
  #### 3.7.2 对vector排序
  sort(vect.begin(), vect.end());
-------------------

# **经典问题**  
## 动态规划
  - [经典动态规划](https://blog.csdn.net/ailaojie/article/details/83014821) 
--------
## 回溯算法
---------
## 贪心算法
--------------
## 分治算法
------------------
## 分支界限算法
------------
## 01背包问题  
