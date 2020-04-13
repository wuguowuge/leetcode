# **动态规划**


动态规划的三要素:最优子结构,边界和状态转移函数,最优子结构是指每个阶段的最优状态可以从之前某个阶段的某个或某些状态直接得到(子问题的最优解能够决定这个问题的最优解),边界指的是问题最小子集的解(初始范围),状态转移函数是指从一个阶段向另一个阶段过度的具体形式,描述的是两个相邻子问题之间的关系(递推式)    

动态规划的三要素:最优子结构,边界和状态转移函数,最优子结构是指每个阶段的最优状态可以从之前某个阶段的某个或某些状态直接得到(子问题的最优解能够决定这个问题的最优解),边界指的是问题最小子集的解(初始范围),状态转移函数是指从一个阶段向另一个阶段过度的具体形式,描述的是两个相邻子问题之间的关系(递推式)    

重叠子问题,对每个子问题只计算一次,然后将其计算的结果保存到一个表格中,每一次需要上一个子问题解时,进行调用,只要o(1)时间复杂度,准确的说,动态规划是利用空间去换取时间的算法.      


## 基础题目

### 1、爬楼梯
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

示例 1：

输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
示例 2：

输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
https://leetcode-cn.com/explore/interview/card/top-interview-questions-easy/23/dynamic-programming/54/

- 分析：
    分析:
        假定n=10,首先考虑最后一步的情况,要么从第九级台阶再走一级到第十级,要么从第八级台阶走两级到第十级,因而,要想到达第十级台阶,最后一步一定是从第八级或者第九级台阶开始.也就是说已知从地面到第八级台阶一共有X种走法,从地面到第九级台阶一共有Y种走法,那么从地面到第十级台阶一共有X+Y种走法.
即F(10)=F(9)+F(8)    
     分析到这里,动态规划的三要素出来了.    
        边界:F(1)=1,F(2)=2     
        最优子结构:F(10)的最优子结构即F(9)和F(8)     
        状态转移函数:F(n)=F(n-1)+F(n-2)      
        
- python 非递归：
```python
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """

        if n<=2:
            return n
        a=1　　#边界
        b=2　　#边界
        temp=0
        for i in range(3,n+1):
            temp=a+b　　　　#状态转移
            a=b　　　　　　　　#最优子结构
            b=temp　　　　　　#最优子结构
        return temp
```

- C++ 递归：
```javascript

public class Solution {
    public int JumpFloor(int target) {
        if (target <= 0) {
            return -1;
        } else if (target == 1) {
            return 1;
        } else if (target ==2) {
            return 2;
        } else {
            return  JumpFloor(target-1)+JumpFloor(target-2);
        }
    }
}
```

### 2、最大子序列和

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。   

示例:   

输入: [-2,1,-3,4,-1,2,1,-5,4],   
输出: 6    
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。   

https://leetcode-cn.com/problems/maximum-subarray/    

- 思路：
    利用动态规划的思路解题: 首先寻找最优子问题,[-2,1,-3,4,-1,2,1,-5,4],第一个最优子问题为-2,那么到下一个1时,其最优为当前值或者当前值加上上一个最优值,因而可以得到其递推公式     
状态转移方程    
dp[i] = max(nums[i], nums[i] + dp[i - 1])    
解释    

i代表数组中的第i个元素的位置    
dp[i]代表从0到i闭区间内，所有包含第i个元素的连续子数组中，总和最大的值    

- python版本
```python
nums = [-2,1,-3,4,-1,2,1,-5,4]
dp = [-2, 1, -2, 4, 3, 5, 6, 1, 5]
class Solution(object):-- 
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        
#        判断边界
        if len(nums)==0:
            return 0
#         定义一个表格进行存储上一个子问题的最优解
        d=[]
        d.append(nums[0])    #第一个最优解为第一个元素
        max_=nums[0]      #返回的最大值
        for i in range(1,len(nums)):
            if nums[i]>nums[i]+d[i-1]:
                d.append(nums[i])
            else:
                d.append(nums[i]+d[i-1])
            if max_<d[i]:
                max_=d[i]
        return max_
```


### 3、打家劫舍

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下，能够偷窃到的最高金额。


- 思路：
示例 1:    

输入: [1,2,3,1]    
输出: 4    
解释: 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。    
     偷窃到的最高金额 = 1 + 3 = 4 。    
示例 2:   

输入: [2,7,9,3,1]    
输出: 12    
解释: 偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。     
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。       
https://leetcode-cn.com/problems/house-robber/    


- python
```python
class Solution(object):
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """

#      思路:由该例子解析:[1,2,3,1],最后一个1,该点有两种操作,一个是偷取,那么的加上在2处的最优解,不偷取,获取3处的最优解.因而f(1)的最优子解为max(f(2)+1,f(3))转移方程:d[i]=max(d[i-1],d[i-2]+nums[i]) , 边界 d[0]=nums[0],d[1]=max(nums[0],nums[1]) 

        if len(nums)==0:
            return 0
        if len(nums)<=2:
            return max(nums)
        dp=[]
        dp.append(nums[0])
        dp.append(max(nums[0],nums[1]))
        for i in range(2,len(nums)):
            dp.append(max(dp[i-1],dp[i-2]+nums[i]))
        return dp[-1]
```

### 4、买卖股票最佳时机

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。   

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。   

注意你不能在买入股票前卖出股票。   
 
示例 1:   

输入: [7,1,5,3,6,4]   
输出: 5    
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。    
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。     
示例 2:    

输入: [7,6,4,3,1]    
输出: 0    
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。    
https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/    

- python
```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
#         思路：动态规划，设置一个变量记录买入的最小金额

#           [7,1,5,3,6,4]  从最后一个4开始分析，比如我从4卖出，那么其获得的最大利润为（6）的时候最大利润与（4）-最小值之间的最大值，
#           递推式为  f（4）=max（f（6），4-最小金额） 边界： f（0）=0，最优子结构：f（4）的最有子结构为f（6）

        if len(prices)<=1:
            return 0
        dp=[]
        dp.append(0)
        min_value=prices[0]
        for i in range(1,len(prices)):
            dp.append(max(dp[i-1],prices[i]-min_value))
            if prices[i]<min_value:
                min_value=prices[i]
        return dp[-1]
```


### 5、使用最小花费