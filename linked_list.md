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

### 3、二叉树的下一个节点

给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。 

- 思路：
    ![示意图](/img/中序遍历节点的下一个.png)
    
    结合图，我们可发现分成两大类：
    1、有右子树的，那么下个结点就是右子树最左边的点；（eg：D，B，E，A，C，G）；
    2、没有右子树的，也可以分成两类；
        a) 是父节点左孩子（eg：N，I，L） ，那么父节点就是下一个节点 ； 
        b)是父节点的右孩子（eg：H，J，K，M）找他的父节点的父节点的父节点...直到当前结点是其父节点的左孩子位置。如果没有eg：M，那么他就是尾节点。


```c++
/*
struct TreeLinkNode {
int val;
struct TreeLinkNode *left;
struct TreeLinkNode *right;
struct TreeLinkNode *next;
TreeLinkNode(int x) :val(x), left(NULL), right(NULL), next(NULL) {
}
};
*/
//分析二叉树的下一个节点，一共有以下情况：
//1.二叉树为空，则返回空；
//2.节点右孩子存在，则设置一个指针从该节点的右孩子出发，一直沿着指向左子结点的指针找到的叶子节点即为下一个节点；
//3.节点不是根节点。如果该节点是其父节点的左孩子，则返回父节点；否则继续向上遍历其父节点的父节点，重复之前的判断，返回结果。
classSolution{
public:
    TreeLinkNode* GetNext(TreeLinkNode* pNode)
    {
        if (pNode == NULL)
            returnNULL;
        if (pNode->right != NULL)
        {
            pNode = pNode->right;
            while (pNode->left != NULL)
                pNode = pNode->left;
            returnpNode;
        }
        while (pNode->next != NULL)
        {
            TreeLinkNode *proot = pNode->next;
            if (proot->left == pNode)
                returnproot;
            pNode = pNode->next;
        }
        returnNULL;
    }
};
```