# **链表**

## 剑指offer

### 1、链表环的入口
给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。

- 思路：

    设置快慢指针，都从链表头出发，快指针每次走两步，慢指针一次走一步，假如有环，一定相遇于环中某点(结论1)。接着让两个指针分别从相遇点和链表头出发，两者都改为每次走一步，最终相遇于环入口(结论2)。以下是两个结论证明： 
    - **两个结论：**
    **1、设置快慢指针，如果有环，肯定相遇。**
    **2、两个指针分别从链表头和相遇点继续出发，每走一步，最后一定相遇与环入口。**

    **证明结论1：** 
    设置快慢指针fast和low，fast每次走两步，low每次走一步。假如有环，两者一定会相遇（因为low一旦进环，可看作fast在后面追赶low的过程，每次两者都接近     一步，最后一定能追上）
    
    **证明结论2：**
    设：链表头到环入口长度为--a，环入口到相遇点长度为--b，相遇点到环入口长度为--c
   
        ![示意图](/img/环入口.jpg)
    
    
    则相遇时，快指针路程=a+(b+c)k+b ，k>=1  其中b+c为环的长度，k为绕环的圈数（k>=1,即最少一圈，不能是0圈，不然和慢指针走的一样长，矛盾）。
    
    慢指针路程=a+b，快指针走的路程是慢指针的两倍，所以：（a+b）*2=a+(b+c)k+b  
    
    化简可得：a=(k-1)(b+c)+c 这个式子的意思是： 链表头到环入口的距离=相遇点到环入口的距离+（k-1）圈环长度。其中k>=1,所以k-1>=0圈。所以两个指针分别从链表头和相遇点出发，最后一定相遇于环入口。 
                 
                
C++版本：
```c++
class Solution {
public:
    ListNode* EntryNodeOfLoop(ListNode* pHead)
    {
        ListNode*fast=pHead,*low=pHead;
        while(fast&&fast->next){
            fast=fast->next->next;
            low=low->next;
            if(fast==low)
                break;
        }
        if(!fast||!fast->next)return NULL;
        low=pHead;//low从链表头出发
        while(fast!=low){//fast从相遇点出发
            fast=fast->next;
            low=low->next;
        }
        return low;
    }
};
```

### 2、删除重复节点

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5 

- 思路：
    - 头结点可能会被删除，所以需新创建一个节点；
    - 设置 pre ，last 指针， pre指针指向当前确定不重复的那个节点，而last指针相当于工作指针，一直往后面搜索；
    
```c++
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution {
public:
    ListNode* deleteDuplication(ListNode* pHead)
    {
          if(pHead==NULL||pHead->next==NULL) return pHead;
          else
          {
              //新建一个节点，防止头结点要被删除
              ListNode* newHead=new ListNode(-1);
              newHead->next=pHead;
              ListNode* pre=newHead;
              ListNode* p=pHead;
              ListNode* next=NULL;
              while(p!=NULL && p->next!=NULL)
              {
                  next=p->next;
                  if(p->val==next->val)//如果当前节点的值和下一个节点的值相等
                  {
                      while(next!=NULL && next->val==p->val)//向后重复查找
                          next=next->next;
                    pre->next=next;//指针赋值，就相当于删除
                    p=next;
                  }
                  else//如果当前节点和下一个节点值不等，则向后移动一位
                  {
                      pre=p;
                      p=p->next;
                  }
              }
           return newHead->next;//返回头结点的下一个节点
               
          }
    }
};
```

### 3、复杂链表的复制

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）    


C++版本
```javascript
class Solution {
public:
    /*
        1、复制每个节点，如：复制节点A得到A1，将A1插入节点A后面
        2、遍历链表，A1->random = A->random->next;
        3、将链表拆分成原链表和复制后的链表
    */
     
    RandomListNode* Clone(RandomListNode* pHead)
    {
        if(!pHead) return NULL;
        RandomListNode *currNode = pHead;
        while(currNode){
            RandomListNode *node = new RandomListNode(currNode->label);
            node->next = currNode->next;
            currNode->next = node;
            currNode = node->next;
        }
        currNode = pHead;
        while(currNode){
            RandomListNode *node = currNode->next;
            if(currNode->random){               
                node->random = currNode->random->next;
            }
            currNode = node->next;
        }
        //拆分
        RandomListNode *pCloneHead = pHead->next;
        RandomListNode *tmp;
        currNode = pHead;
        while(currNode->next){
            tmp = currNode->next;
            currNode->next =tmp->next;
            currNode = tmp;
        }
        return pCloneHead;
    }
};
```

python版本(写的啰嗦，随后修改)
```python

# -*- coding:utf-8 -*-
# class RandomListNode:
#     def __init__(self, x):
#         self.label = x
#         self.next = None
#         self.random = None
class Solution:
    # 返回 RandomListNode
    def Clone(self, pHead):
        if not pHead:
            return None
         
        dummy = pHead
         
        # first step, N' to N next
        while dummy:
            dummynext = dummy.next
            copynode = RandomListNode(dummy.label)
            copynode.next = dummynext
            dummy.next = copynode
            dummy = dummynext
         
        dummy = pHead
         
        # second step, random' to random'
        while dummy:
            dummyrandom = dummy.random
            copynode = dummy.next
            if dummyrandom:
                copynode.random = dummyrandom.next
            dummy = copynode.next
         
        # third step, split linked list
        dummy = pHead
        copyHead = pHead.next
        while dummy:
            copyNode = dummy.next
            dummynext = copyNode.next
            dummy.next = dummynext
            if dummynext:
                copyNode.next = dummynext.next
            else:
                copyNode.next = None
            dummy = dummynext
 
        return copyHead
```

### 4、从尾到头打印

- 方法1：
    递归    
    
```javascript
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> printListFromTailToHead(struct ListNode* head) {
        vector<int> value;
        if(head != NULL)
        {
            value.insert(value.begin(),head->val);
            if(head->next != NULL)
            {
                vector<int> tempVec = printListFromTailToHead(head->next);
                if(tempVec.size()>0)
                value.insert(value.begin(),tempVec.begin(),tempVec.end());  
            }         
             
        }
        return value;
    }
};
```

- 方法2：堆栈

```javascript
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int> value;
        ListNode *p=NULL;
        p=head;
        stack<int> stk;
        while(p!=NULL){
            stk.push(p->val);
            p=p->next;
        }
        while(!stk.empty()){
            value.push_back(stk.top());
            stk.pop();
        }
        return value;
    }
};
```
 
 python版本（递归，python里面list型添加元素可以直接+[num]）
 ```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution: 
# 返回从尾部到头部的列表值序列，例如[1,2,3] 
    def printListFromTailToHead(self, listNode): 
    # write code here 
    if listNode is None: 
        return [] 
    return self.printListFromTailToHead(listNode.next) + [listNode.val]
    
```

### 5、链表倒数第k个节点

- 思路：
    双指针
ptyhon版本：
```python
class Solution:
    def FindKthToTail(self, head, k):
        # write code here
        if head==None or k<=0:
            return None
        #设置两个指针，p2指针先走（k-1）步，然后再一起走，当p2为最后一个时，p1就为倒数第k个 数
        p2=head
        p1=head
        #p2先走，走k-1步，如果k大于链表长度则返回 空，否则的话继续走
        while k>1:
            if p2.next!=None:
                p2=p2.next
                k-=1
            else:
                return None
#两个指针一起 走，一直到p2为最后一个,p1即为所求
        while p2.next!=None:
            p1=p1.next
            p2=p2.next
        return p1
```

C++版本
```javascript
class Solution {
public:
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        if(pListHead==NULL||k==0)
            return NULL;
        ListNode*pTail=pListHead,*pHead=pListHead;
        for(int i=1;i<k;++i)
        {
            if(pHead->next!=NULL)
                pHead=pHead->next;
            else
                return NULL;
        }
        while(pHead->next!=NULL)
        {
            pHead=pHead->next;
            pTail=pTail->next;
        }
        return pTail;
    }
};
```

### 6、翻转链表


- 思路：
    好像没有python的非递归实现。思路很简单：1->2->3->4->5，遍历链表，把1的next置为None，2的next置为1，以此类推，5的next置为4。得到反转链表。需要考虑链表只有1个元素的情况。图中有具体的每步迭代的思路，最后输出pre而不是cur是因为最后一次迭代后cur已经指向None了，而pre是完整的反向链表。   
```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    # 返回ListNode
    def ReverseList(self, pHead):
        # write code here
        if pHead==None or pHead.next==None:
            return pHead
        pre = None
        cur = pHead
        while cur!=None:
            tmp = cur.next
            cur.next = pre
            pre = cur
            cur = tmp
        return pre
```

### 7、合并两个链表

两个非递减的链表进行合并，保证非递减

python非递归版本：
pHead始终指向第一个节点，tmp会一直往后移动 tmp = tmp.nex

- 非递归思路：
     比较两个链表的首结点，哪个小的的结点则合并到第三个链表尾结点，并向前移动一个结点。     
    步骤一结果会有一个链表先遍历结束，或者没有；    
    第三个链表尾结点指向剩余未遍历结束的链表；     
    返回第三个链表首结点；       
```python

class Solution:
    # 返回合并后列表
    def Merge(self, pHead1, pHead2):
        # write code here
        #初始化
        tmp = ListNode(0)
        pHead = tmp        
        while pHead1 and pHead2:
            if pHead1.val < pHead2.val:
                tmp.next = pHead1
                pHead1 = pHead1.next
            else:
                tmp.next = pHead2
                pHead2 = pHead2.next
            tmp = tmp.next
        if not pHead1:
            tmp.next = pHead2
        if not pHead2:
            tmp.next = pHead1
        return pHead.next
```

python递归版本：
```python

class Solution:
    # 返回合并后列表
    def Merge(self, pHead1, pHead2):
        # write code here
        if pHead1 is None:
            return pHead2
        if pHead2 is None:
            return pHead1
        if pHead1.val < pHead2.val:
            pHead1.next = self.Merge(pHead1.next,pHead2)
            return pHead1
        else:
            pHead2.next = self.Merge(pHead1,pHead2.next)
            return pHead2
```

### 8、二叉搜索树和双向链表

- 思路：
    中序遍历即可。只需要记录一个pre指针即可；     
    
C++版本：
```javascript
class Solution {
public:
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        if(pRootOfTree == nullptr) return nullptr;
        TreeNode* pre = nullptr;
         
        convertHelper(pRootOfTree, pre);
         
        TreeNode* res = pRootOfTree;
        while(res ->left)
            res = res ->left;
        return res;
    }
     
    void convertHelper(TreeNode* cur, TreeNode*& pre)
    {
        if(cur == nullptr) return;
         
        convertHelper(cur ->left, pre);
         
        cur ->left = pre;
        if(pre) pre ->right = cur;
        pre = cur;
         
        convertHelper(cur ->right, pre);
         
    }
};
```

python版本：

非递归版本：
    1、叉树的中序遍历；     
    2、中序遍历中每个结点的链接；
         
```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def Convert(self, pRootOfTree):
        if not pRootOfTree:
            return None
         
        p = pRootOfTree
         
        stack = []
        resStack = []
         
        while p or stack:
            if p:
                stack.append(p)
                p = p.left
            else:
                node = stack.pop()
                resStack.append(node)
                p = node.right
             
        resP = resStack[0]
        while resStack:
            top = resStack.pop(0)
            if resStack:
                top.right = resStack[0]
                resStack[0].left = top
         
        return resP
```

递归版本：
```python
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def Convert(self, root):
        if not root:
            return None
        if not root.left and not root.right:
            return root
         
        # 将左子树构建成双链表，返回链表头
        left = self.Convert(root.left)
        p = left
         
        # 定位至左子树的最右的一个结点
        while left and p.right:
            p = p.right
         
        # 如果左子树不为空，将当前root加到左子树链表
        if left:
            p.right = root
            root.left = p
         
        # 将右子树构造成双链表，返回链表头
        right = self.Convert(root.right)
        # 如果右子树不为空，将该链表追加到root结点之后
        if right:
            right.left = root
            root.right = right
             
        return left if left else root
```
