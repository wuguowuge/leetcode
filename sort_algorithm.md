# **排序**

## 常见排序
- 参考链接：
    https://www.cnblogs.com/Mufasa/p/10527387.html   
    https://blog.csdn.net/Dby_freedom/article/details/82154869   
    


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



### 3、选择排序

- 思路：
    选择最小的放在前边   

python版本
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

### 4、插入排序

- 思路：
    追个插入到前面的有序数组中
    
python版本

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

