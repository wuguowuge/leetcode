# **数组**


## 剑指offer


### 1、和为S的连续正数序列

连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列

- 题解：

    双指针技术，就是相当于有一个窗口，窗口的左右两边就是两个指针，我们根据窗口内值之和来确定窗口的位置和宽度。非常牛逼的思路，虽然双指针或者所谓的滑动窗口技巧还是蛮常见的，但是这一题还真想不到这个思路

```c++
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {
        vector<vector<int> > allRes;
        int phigh = 2,plow = 1;
         
        while(phigh > plow){
            int cur = (phigh + plow) * (phigh - plow + 1) / 2;
            if( cur < sum)
                phigh++;
             
            if( cur == sum){
                vector<int> res;
                for(int i = plow; i<=phigh; i++)
                    res.push_back(i);
                allRes.push_back(res);
                plow++;
            }
             
            if(cur > sum)
                plow++;
        }
         
        return allRes;
    }
};
```

### 2、连续子数组的最大和

给定一个数组 array[1, 4, -5, 9, 8, 3, -6]，在这个数字中有多个子数组，子数组和最大的应该是：[9, 8, 3]，输出20，再比如数组为[1, -2, 3, 10, -4, 7, 2, -5]，和最大的子数组为[3, 10, -4, 7, 2]，输出18。

- 题解：
	给定一个数组 array[1, 4, -5, 9, 8, 3, -6]，在这个数字中有多个子数组，子数组和最大的应该是：[9, 8, 3]，输出20，再比如数组为[1, -2, 3, 10, -4, 7, 2, -5]，和最大的子数组为[3, 10, -4, 7, 2]，输出18。

```c++
class Solution {
public:
    int FindGreatestSumOfSubArray(vector<int> array) {
        int m = array[0];
        int b = 0;
       for(int i = 0;i < array.size();i++)
        {
            if(b < 0) b = array[i];
            else b += array[i];
            m = max(m,b); 
        }
        return m;
    }
};
```



