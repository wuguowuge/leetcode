# **字符串**

## 剑指offer

### 1、字符串翻转
“student. a am I”-->“I am a student.”

- 思路
    tmp是当前正在处理的单词，res是反转之后的句子。当一个单词结束后将它拼接到res的前面。
    

C++版本
```c++
class Solution {
public:
    string ReverseSentence(string str) {
        string res = "", tmp = "";
        for(unsigned int i = 0; i < str.size(); ++i){
            if(str[i] == ' ') res = " " + tmp + res, tmp = "";
            else tmp += str[i];
        }
        if(tmp.size()) res = tmp + res;
        return res;
    }
}; 
```


python版本

- 思路
    [::-1]就是返回反转后的list。list的成员reverse可以，不过只是反转list而不会返回反转之后的list，那样要写成两个语句了
    这是python的slice语法, 可以理解为[begin, end, step], step为步长。当指定step而不指定begin和end时，会根据step的正负来正向或反向地遍历所有元素。
python版本
```python
# -*- coding:utf-8 -*-
class Solution:
    def ReverseSentence(self, s):
 
        return " ".join(s.split(" ")[::-1])
```

---

### 2、循环左移
abcXYZdef --> abcXYZdef

C++版本

- 思路：
    str.substr(pos, n) 返回一个字符串，包含s中从下标pos开始的n个字符
```c++
class Solution {
public:
    string LeftRotateString(string str, int n) {
        int len = str.length();
        if(len == 0) return "";
        n = n % len;
        str += str;
        return str.substr(n, len);
    }
};
```

python版本

```python
# -*- coding:utf-8 -*-
class Solution:
    def LeftRotateString(self, s, n):
        # write code here
        return s[n:]+s[:n]
```

### 3、第一个出现的字符

请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。 

- 思路1：

        1、用一个128大小的数组统计每个字符出现的次数；
        2、用一个队列，如果第一次遇到ch字符，则插入队列；其他情况不在插入；
        3、求解第一个出现的字符，判断队首元素是否只出现一次，如果是直接返回，否则删除继续第3步骤；
         
 可以看出相同的字符只被插入一次，最多push128个，同时大多数情况会直接返回队首。所以大家不要被里面的while循环迷惑 
 时间复杂度O（1），空间复杂度O（n）
 
 ```c++
class Solution
{
public:
  //Insert one char from stringstream
    void Insert(char ch)
    {  
        ++cnt[ch - '\0'];
        if(cnt[ch - '\0'] == 1)
           data.push(ch);
    }
  //return the first appearence once char in current stringstream
    char FirstAppearingOnce()
    {
      while(!data.empty() && cnt[data.front()] >= 2) data.pop();
        if(data.empty()) return '#';
        return data.front();
    }
    Solution()
    {
      memset(cnt, 0, sizeof(cnt));    
    }
private:
    queue<char> data;
    unsigned cnt[128];
};
```

- 思路2：  
        出现的字符 和 它的出现的次数 是一种对应关系，自然联想到 哈希表的 key-value 这种对应，或者应用关联容器 map，可以很方便的解决这个问题。
        map 容器中，它的一个元素 就是一组（key，value）对应的数据 ；
        
```javascript
class Solution
{
public:
  //Insert one char from stringstream
    void Insert(char ch)
    {
  vec.push_back(ch);
         mapdata[ch]++;
    }
  //return the first appearence once char in current stringstream
    char FirstAppearingOnce()
    {
  vector<int>::iterator it;
  for(it=vec.begin();it!=vec.end();it++)
  {
   if(mapdata[*it]==1)
    return *it;
  }
  return '#';
    }
 map<char,int> mapdata;
 vector<int> vec;
 };
```

