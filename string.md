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

