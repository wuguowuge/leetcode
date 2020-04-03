# **排序**

## 常见排序


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
```