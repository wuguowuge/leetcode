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

-----

### 2、连续子数组的最大和

给定一个数组 array[1, 4, -5, 9, 8, 3, -6]，在这个数字中有多个子数组，子数组和最大的应该是：[9, 8, 3]，输出20，再比如数组为[1, -2, 3, 10, -4, 7, 2, -5]，和最大的子数组为[3, 10, -4, 7, 2]，输出18。

- 题解：
	给定一个数组 array[1, 4, -5, 9, 8, 3, -6]，在这个数字中有多个子数组，子数组和最大的应该是：[9, 8, 3]，输出20，再比如数组为[1, -2, 3, 10, -4, 7, 2, -5]，和最大的子数组为[3, 10, -4, 7, 2]，输出18。


C++版本：

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

python版本：

```python
class Solution:
    def FindGreatestSumOfSubArray(self, array):
        # write code here
        length = len(array)
        if(length == 0):
            return 0
        else:
            max_num = array[0]
            temp_sum = array[0]
            for i in range(1,length):
                if temp_sum <= 0:
                    temp_sum = array[i]
                else:
                    temp_sum += array[i]
                if temp_sum > max_num:
                    max_num = temp_sum
            return max_num
```

### 3、约瑟夫环问题
随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个，求最后一个的位置。


- 思路：
    利用数组模仿环，进行操作；
 
 C++版本
 
 ```javascript
 class Solution{
    public:
        int LastRemaining(unsigned int n, unsigned int m)
        {
            if(n<1 || m<1) return -1;
            vector<int> array<n>;
            int i = -1,step=0,count=n;
            while(count>0){
                i++; //为了计算n
                if(i>n) i=0; //模仿环
                if(array[i]==-1) continue; //如果遇到-1，说明已经出局，pass
                step++;  //为了计算m
                if(step==m){
                    array[i]=-1; 
                    step = 0;
                    count--;
                }
                    
            }
        }
        return i;
 }
```

### 4、求和
求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

- 思路：
    递归可以替代循环语句，利用短路求值作为终止条件；
    
```c++
class Solution{
    public:
    int Sum_Solution(int n){
        int ans = n;
        ans && (ans += Sum_Solution(n-1)); //ans=0时终止
        return ans;
    }
};
```

### 5、求和
写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

- 思路：
    - 两个数异或：相当于每一位相加，而不考虑进位；
    - 两个数相与，并左移一位：相当于求得进位；
    - 将上述两步的结果相加

```c++
class Solution{
    public:
    int Add(int num1, int num2){
        while(num2!=0){
            int sum = num1^num2;
            int carray = (num1 & num2)<<1;
            num1 = sum;
            num2 = carray;
        }
        return num1;
    }
}
``` 

### 6、数据流的中位数

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。我们使用Insert()方法读取数据流，使用GetMedian()方法获取当前读取数据的中位数。 

- 思路：

    主要的思想是：因为要求的是中位数，那么这两个堆，大顶堆用来存较小的数，从大到小排列；   
    小顶堆存较大的数，从小到大的顺序排序，显然中位数就是大顶堆的根节点与小顶堆的根节点和的平均数。   
    ⭐保证：小顶堆中的元素都大于等于大顶堆中的元素，所以每次塞值，并不是直接塞进去，而是从另一个堆中poll出一个最大（最小）的塞值   
    ⭐当数目为偶数的时候，将这个值插入大顶堆中，再将大顶堆中根节点（即最大值）插入到小顶堆中；   
    ⭐当数目为奇数的时候，将这个值插入小顶堆中，再讲小顶堆中根节点（即最小值）插入到大顶堆中；   
    ⭐取中位数的时候，如果当前个数为偶数，显然是取小顶堆和大顶堆根结点的平均值；如果当前个数为奇数，显然是取小顶堆的根节点   

理解了上面所述的主体思想，下面举个例子辅助验证一下。   

例如，传入的数据为：[5,2,3,4,1,6,7,0,8],那么按照要求，输出是"5.00 3.50 3.00 3.50 3.00 3.50 4.00 3.50 4.00 "   

那么整个程序的执行流程应该是（用min表示小顶堆，max表示大顶堆）：   

    5先进入大顶堆，然后将大顶堆中最大值放入小顶堆中，此时min=[5],max=[无]，avg=[5.00]   
    2先进入小顶堆，然后将小顶堆中最小值放入大顶堆中，此时min=[5],max=[2],avg=[(5+2)/2]=[3.50]   
    3先进入大顶堆，然后将大顶堆中最大值放入小顶堆中，此时min=[3,5],max=[2],avg=[3.00]   
    4先进入小顶堆，然后将小顶堆中最小值放入大顶堆中，此时min=[4,5],max=[3,2],avg=[(4+3)/2]=[3.50]   
    1先进入大顶堆，然后将大顶堆中最大值放入小顶堆中，此时min=[3,4,5],max=[2,1]，avg=[3/00]   
    6先进入小顶堆，然后将小顶堆中最小值放入大顶堆中，此时min=[4,5,6],max=[3,2,1],avg=[(4+3)/2]=[3.50]   
    7先进入大顶堆，然后将大顶堆中最大值放入小顶堆中，此时min=[4,5,6,7],max=[3,2,1],avg=[4]=[4.00]   
    0先进入小顶堆，然后将小顶堆中最大值放入小顶堆中，此时min=[4,5,6,7],max=[3,2,1,0],avg=[(4+3)/2]=[3.50]   
    8先进入大顶堆，然后将大顶堆中最小值放入大顶堆中，此时min=[4,5,6,7,8],max=[3,2,1,0],avg=[4.00] 

