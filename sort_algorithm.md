# **排序**

## 常见排序
- 参考链接：
    https://www.cnblogs.com/Mufasa/p/10527387.html   
    https://blog.csdn.net/Dby_freedom/article/details/82154869
    https://blog.csdn.net/morewindows/article/details/6678165    
    白话经典算系列：   
    https://blog.csdn.net/MoreWindows/category_859207.html      
    


### 1、堆排序

堆是具有以下性质的完全二叉树：每个结点的值都大于或等于其左右孩子结点的值，称为大顶堆；或者每个结点的值都小于或等于其左右孩子结点的值，称为小顶堆。如下图：

  ![示意图](/img/duipaixu.png)
  
- 思路:
    堆排序过程将待排序的序列构造成一个堆，选出堆中最大的移走，再把剩余的元素调整成堆，找出最大的再移走，重复直至有序
    
```javascript
//堆排序
void HeapSort(int arr[],int len){
    int i;
    //初始化堆，从最后一个父节点开始
    for(i = len/2 - 1; i >= 0; --i){
        Heapify(arr,i,len);
    }
    //从堆中的取出最大的元素再调整堆
    for(i = len - 1;i > 0;--i){
        int temp = arr[i];
        arr[i] = arr[0];
        arr[0] = temp;
        //调整成堆
        Heapify(arr,0,i);
    }
}

// 调整成堆的函数
//堆排序 
void Heapify(int arr[], int first, int end){
    int father = first;
    int son = father * 2 + 1;
    while(son < end){
        if(son + 1 < end && arr[son] < arr[son+1]) ++son;
        //如果父节点大于子节点则表示调整完毕
        if(arr[father] > arr[son]) break;
        else {
         //不然就交换父节点和子节点的元素
            int temp = arr[father];
            arr[father] = arr[son];
            arr[son] = temp;
            //父和子节点变成下一个要比较的位置
            father = son;
            son = 2 * father + 1;
        }
    }
}
```

### 2、冒泡排序（bubble sort）
- 步骤：
    1、比较相邻的元素。如果第一个比第二个大，就交换他们两个。   
    2、对第0个到第n-1个数据做同样的工作。这时，最大的数就“浮”到了数组最后的位置上。   
    3、针对所有的元素重复以上的步骤，除了最后一个。   
    4、持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。   
    
- 思路：
    前后比较交换   

python版本1
```python
d0 = [2, 15, 5, 9, 7, 6, 4, 12, 5, 4, 2, 64, 5, 6, 4, 2, 3, 54, 45, 4, 44]
d0_out = [2, 2, 2, 3, 4, 4, 4, 4, 5, 5, 5, 6, 6, 7, 9, 12, 15, 44, 45, 54, 64]  # 正确排序

while 1:
    state = 0  # 假设本次循环没有改变
    for i in range(len(d0) - 1):
        if d0[i] > d0[i + 1]:
            d0[i], d0[i + 1] = d0[i + 1], d0[i]
            state = 1  # 有数值交换，那么状态值置1
    if not state:  # 如果没有数值交换，那么就跳出
        break

print(d0)
print(d0_out)
```

python-错误版本
```python
def bubble_sort(arry):
    n = len(arry)                   #获得数组的长度
    for i in range(n):
        for j in range(i+1, n):
            if  arry[i] > arry[j] :       #如果前者比后者大
                arry[i],arry[j] = arry[j],arry[i]      #则交换两者
    return arry

```
注：上述代码是没有问题的，但是实现却不是冒泡排序，而是选择排序（原理见选择排序），注意冒泡排序的本质是“相邻元素”的顺序交换，而非每次完成一个最小数字的选定。   



python版本2
```python
def bubble_sort(arry):
    n = len(arry)                   #获得数组的长度
    for i in range(n):
        for j in range(1, n-i):    # 每轮找到最大数值 或者用 for j in range(i+1, n)
            if  arry[j-1] > arry[j] :       #如果前者比后者大
                arry[j-1],arry[j] = arry[j-1],arry[j]      #则交换两者
    return arry

```
python 优化版本1

- 思路： 某一趟遍历如果没有数据交换，则说明已经排好序了，因此不用再进行迭代了。用一个标记记录这个状态即可。   

```python
def bubble_sort2(ary):
	n = len(ary)
	for i in range(n):
		flag = True    # 标记
		for j in range(1, n - i):
			if ary[j] < ary[j-1]:
				ary[j], ary[j-1] = ary[j-1], ary[j]
				flag = False
		# 某一趟遍历如果没有数据交换，则说明已经排好序了，因此不用再进行迭代了
		if flag:    
			break
	return ary
			
```

python 优化版本2

记录某次遍历时最后发生数据交换的位置，这个位置之后的数据显然已经有序，不用再排序了。因此通过记录最后发生数据交换的位置就可以确定下次循环的范围了。


```python
def bubble_sort3(ary):
	n = len(ary)
	k = n    #k为循环的范围，初始值n
	for i in range(n):
		flag = True
		for j in range(1, k):    #只遍历到最后交换的位置即可
			if ary[j-1] > ary[j]:
				ary[j-1], ary[j] = ary[j], ary[j-1]
				k = j     #记录最后交换的位置
				flag = False
		if flag:
			break
	return ary
				
```
注：上面for j in range(1,k)，这句很有意思，虽然后面有if ary[j-1] > ary[j]则k = j,但是这个k不会直接就变动，不然试想，当j=1，0与1位置坐了交换之后，k=j=1，j这一步循环直接就挂掉了，事实上，k的改变是在下一轮i坐了改变之后才会真正起作用，所以j可以记录最后交换位置。



### 3、选择排序(selection sort)

- 特点：
    1、**运行时间与输入无关**   
    
    2、**数据移动是最少的**   

- 思路：
    选择最小的放在前边   

- 步骤：
    1、在未排序序列中找到最小（大）元素，存放到排序序列的起始位置。   
    2、再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。   
    3、以此类推，直到所有元素均排序完毕。   
    
python版本1
```python
def select_sort(data):
    d1 = []
    while len(data):
        min = [0, data[0]]
        for i in range(len(data)):
            if min[1] > data[i]:
                min = [i, data[i]]
        del data[min[0]]  # 找到剩余部分的最小值，并且从原数组中删除
        d1.append(min[1])  # 在新数组中添加
    return d1

if __name__ == "__main__":
    d0 = [2, 15, 5, 9, 7, 6, 4, 12, 5, 4, 2, 64, 5, 6, 4, 2, 3, 54, 45, 4, 44]  # 原始乱序
    d0_out = [2, 2, 2, 3, 4, 4, 4, 4, 5, 5, 5, 6, 6, 7, 9, 12, 15, 44, 45, 54, 64]  # 正确排序
    d1 = select_sort(d0)
    print(d1)
    print(d0_out)
```

python 版本2
```python
def select_sort(ary):
    n = len(ary)
    for i in range(0,n):
        min = i                             #最小元素下标标记
        for j in range(i+1,n):
            if ary[j] < ary[min] :
                min = j                     #找到最小值的下标
        ary[min],ary[i] = ary[i],ary[min]   #交换两者
    return ary

```


### 4、插入排序

- 思路：
    插入排序的工作原理是，对于每个未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。   

- 步骤：
    1、从第一个元素开始，该元素可以认为已经被排序   
    2、取出下一个元素，在已经排序的元素序列中从后向前扫描   
    3、如果被扫描的元素（已排序）大于新元素，将该元素后移一位   
    4、重复步骤3，直到找到已排序的元素小于或者等于新元素的位置   
    5、将新元素插入到该位置后   
    6、重复步骤2~5   
    
python版本1
直接插入排序

```python
def direct_insertion_sort(d):   # 直接插入排序，因为要用到后面的希尔排序，所以转成function
    d1 = [d[0]]
    for i in d[1:]:
        state = 1
        for j in range(len(d1) - 1, -1, -1):
            if i >= d1[j]:
                d1.insert(j + 1, i)  # 将元素插入数组
                state = 0
                break
        if state:
            d1.insert(0, i)
    return d1
  

if __name__ == "__main__":
    d0 = [2, 15, 5, 9, 7, 6, 4, 12, 5, 4, 2, 64, 5, 6, 4, 2, 3, 54, 45, 4, 44]  # 原始乱序
    d0_out = [2, 2, 2, 3, 4, 4, 4, 4, 5, 5, 5, 6, 6, 7, 9, 12, 15, 44, 45, 54, 64]  # 正确排序
    d1 = direct_insertion_sort(d0)
    print(d1)
    print(d0_out)
```

python版本2
折半插入排序

```python
d0 = [2, 15, 5, 9, 7, 6, 4, 12, 5, 4, 2, 64, 5, 6, 4, 2, 3, 54, 45, 4, 44]  # 原始乱序
d0_out = [2, 2, 2, 3, 4, 4, 4, 4, 5, 5, 5, 6, 6, 7, 9, 12, 15, 44, 45, 54, 64]  # 正确排序

d1 = [d0[0]]
del d0[0]

for i in d0:
    index_now = [0, len(d1)]
    while 1:
        index = index_now[0] + int((index_now[1] - index_now[0]) / 2)
        if i == d1[index]:
            d1.insert(index+1,i)
            break
        elif index in index_now:  # 如果更新的index值在index_now中存在（也有可能是边界），那么就表明无法继续更新
            d1.insert(index+1,i)
            break
        elif i > d1[index]:
            index_now[0] = index
        elif i < d1[index]:
            index_now[1] = index

print(d1)
print(d0_out)
```

python版本3
```python
# 插入排序
def insert_sort(ary):
	count = len(ary)
	for i in range(1, count):
		key = i - 1
		mark = ary[i]    # 注： 必须将ary[i]赋值为mark，不能直接用ary[i]
		while key >= 0 and ary[key] > mark:
			ary[key+1] = ary[key]
			key -= 1
		ary[key+1] = mark
	return ary

```

### 5、希尔排序（shellsort）

- 思路：
    希尔排序的实质就是分组插入排序，该方法又称缩小增量排序    
    
- 方法：
    该方法的基本思想是：先将整个待排元素序列分割成若干个子序列（由相隔某个“增量”的元素组成的）分别进行直接插入排序，然后依次缩减增量再进行排序，待整个序列中的元素基本有序（增量足够小）时，再对全体元素进行一次直接插入排序。因为直接插入排序在元素基本有序的情况下（接近最好情况），效率是很高的，因此希尔排序在时间效率上比前两种方法有较大提高

    n=10的一个数组49, 38, 65, 97, 26, 13, 27, 49, 55, 4为例    
    
    第一次 gap = 10/2 = 5    
    
    49 38 65 97 26 13 27 49 55 4     
    
    1A 1B   
    2A 2B   
    3A 3B   
    4A 4B   
    5A 5B   
    
    1A, 1B, 2A, 2B等为分组标记，数字相同的表示在同一组，大写字母表示是该组的第几个元素， 每次对同一组的数据进行直接插入排序。即分成了五组(49, 13) (38, 27) (65, 49) (97, 55) (26, 4)这样每组排序后就变成了(13, 49) (27, 38) (49, 65) (55, 97) (4, 26)，下同。   
    第二次 gap = 5 / 2 = 2   
    
    排序后   
    
    13 27 49 55 4 49 38 65 97 26   
    
    1A 1B 1C 1D 1E   
    2A 2B 2C 2D 2E   
    
    第三次 gap = 2 / 2 = 1   
    
    4 26 13 27 38 49 49 55 97 65   
    
    1A 1B 1C 1D 1E 1F 1G 1H 1I 1J   
    
    第四次 gap = 1 / 2 = 0 排序完成得到数组：   
    
    4 13 26 27 38 49 49 55 65 97   
    
```python

def shell_sort(ary):
    count = len(ary)
    gap = round(count / 2)
    # 双杠用于整除（向下取整），在python直接用 “/” 得到的永远是浮点数，
    # 用round()得到四舍五入值
    while gap >= 1:
        for i in range(gap, count):
            temp = ary[i]
            j = i
            while j - gap >= 0 and ary[j - gap] > temp:  # 到这里与插入排序一样了
                ary[j] = ary[j - gap]
                j -= gap
            ary[j] = temp
        gap = round(gap / 2)
    return ary

```


### 6、归并排序

- 思路：
    归并排序是建立在归并操作上的一种有效的排序算法。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。   

    首先考虑下如何将将二个有序数列合并。这个非常简单，只要从比较二个数列的第一个数，谁小就先取谁，取了后就在对应数列中删除这个数。然后再进行比较，如果有数列为空，那直接将另一个数列的数据依次取出即可。   

- 合并方法：
    设r[i…n]由两个有序子表r[i…m]和r[m+1…n]组成，两个子表长度分别为m-i +1、n-m。   
```python
1、j=m+1；k=i；i=i; //置两个子表的起始下标及辅助数组的起始下标
2、若i>m 或j>n，转⑷ //其中一个子表已合并完，比较选取结束
3、//选取r[i]和r[j]较小的存入辅助数组rf
        如果r[i]<r[j]，rf[k]=r[i]； i++； k++； 转⑵
        否则，rf[k]=r[j]； j++； k++； 转⑵
4、//将尚未处理完的子表中元素存入rf
        如果i<=m，将r[i…m]存入rf[k…n] //前一子表非空
        如果j<=n ,  将r[j…n] 存入rf[k…n] //后一子表非空
5、合并结束。
```

```python
# 归并排序

    def merge_sort(ary):
        
        if len(ary) <= 1:
            return ary
        
        median = int(len(ary)/2)    # 二分分解
        left = merge_sort(ary[:median])
        right = merge_sort(ary[median:])
        return merge(left, right)    # 合并数组
    
    def merge(left, right):
     '''合并操作，
    将两个有序数组left[]和right[]合并成一个大的有序数组'''
        res = []
        i = j = k = 0
        while(i < len(left) and j < len(right)):
            if left[i] < right[j]:
                res.append(left[i])
                i += 1
            else:
                res.append(right[j])
                j += 1
                
        res = res + left[i:] + right[j:]
        return res

```

```javascript
//将有二个有序数列a[first...mid]和a[mid...last]合并。
void mergearray(int a[], int first, int mid, int last, int temp[])
{
	int i = first, j = mid + 1;
	int m = mid,   n = last;
	int k = 0;
	
	while (i <= m && j <= n)
	{
		if (a[i] <= a[j])
			temp[k++] = a[i++];
		else
			temp[k++] = a[j++];
	}
	
	while (i <= m)
		temp[k++] = a[i++];
	
	while (j <= n)
		temp[k++] = a[j++];
	
	for (i = 0; i < k; i++)
		a[first + i] = temp[i];
}
void mergesort(int a[], int first, int last, int temp[])
{
	if (first < last)
	{
		int mid = (first + last) / 2;
		mergesort(a, first, mid, temp);    //左边有序
		mergesort(a, mid + 1, last, temp); //右边有序
		mergearray(a, first, mid, last, temp); //再将二个有序数列合并
	}
}
 
bool MergeSort(int a[], int n)
{
	int *p = new int[n];
	if (p == NULL)
		return false;
	mergesort(a, 0, n - 1, p);
	delete[] p;
	return true;
}
```
归并排序的效率是比较高的，设数列长为N，将数列分开成小数列一共要logN步，每步都是一个合并有序数列的过程，时间复杂度可以记为O(N)，故一共为O(N*logN)。因为归并排序每次都是在相邻的数据中进行操作，所以归并排序在O(N*logN)的几种排序方法（快速排序，归并排序，希尔排序，堆排序）也是效率比较高的。


### 7、快速排序（quicksort）

- 思路：
    快速排序通常明显比同为Ο(n log n)的其他算法更快，因此常被采用，而且快排采用了分治法的思想，所以在很多笔试面试中能经常看到快排的影子。可见掌握快排的重要性。
    
    
- 步骤：
    1、从数列中挑出一个元素作为基准数。   
    2、分区过程，将比基准数大的放到右边，小于或等于它的数都放到左边。   
    3、再对左右区间递归执行第二步，直至各区间只有一个数。   
    
python版本
```python

def quick_sort(ary):
    return qsort(ary, 0, len(ary) - 1)


def qsort(ary, start, end):
    if start < end:
        left = start
        right = end
        key = ary[start]
    else:
        return ary
    while left < right:
        while left < right and ary[right] >= key:
            right -= 1
        if left < right:  # 说明打破while循环的原因是ary[right] <= key
            ary[left] = ary[right]
            left += 1
        while left < right and ary[left] < key:
            left += 1
        if left < right:  # 说明打破while循环的原因是ary[left] >= key
            ary[right] = ary[left]
            right -= 1
    ary[left] = key  # 此时，left=right，用key来填坑

    qsort(ary, start, left - 1)
    qsort(ary, left + 1, end)
    return ary
    

```

C++版本
```javascript
#include<iostream>
#include<stack>
#include<vector>
using namespace std;


void quickSortHelper(vector<int>& nums, int begin, int end) {
	if (begin >= end) return;
	int left = begin;
	int right = end;
	int base = nums[left];
	while (left < right) {
		while (left < right && nums[right] >= base) {
			right--;
		}
		if (left < right) {
			nums[left] = nums[right];
			left++;
		}
		while (left < right && nums[left] < base) {
			left++;
		}
		if (left < right) {
			nums[right] = nums[left];
			right--;
		}
	}
	nums[left] = base;
	quickSortHelper(nums, begin, left - 1);
	quickSortHelper(nums, left + 1, end);
	return;
}

vector<int> quickSort(vector<int> nums) {
	if (nums.size() <= 1) return nums;
	int length = nums.size() - 1;
	quickSortHelper(nums, 0, length);
	return nums;

}

int main()
{
	int n;
	cin >> n;
	vector<int> nums(n);
	for (int i = 0; i < n; ++i)
	{
		cin >> nums[i];
	}
	vector<int> res;
	res = quickSort(nums);

	for (auto x : res) {
		cout << x;
	}
	return 0;
}

```

方法2：
    先从待排序的数组中找出一个数作为基准数（取第一个数即可），然后将原来的数组划分成两部分：小于基准数的左子数组和大于等于基准数的右子数组。然后对这两个子数组再递归重复上述过程，直到两个子数组的所有数都分别有序。最后返回“左子数组” + “基准数” + “右子数组”，即是最终排序好的数组。


```python
# 实现快排
def quicksort(nums):
    if len(nums) <= 1:
        return nums

    # 左子数组
    less = []
    # 右子数组
    greater = []
    # 基准数
    base = nums.pop()

    # 对原数组进行划分
    for x in nums:
        if x < base:
            less.append(x)
        else:
            greater.append(x)

    # 递归调用
    return quicksort(less) + [base] + quicksort(greater)
```



    



