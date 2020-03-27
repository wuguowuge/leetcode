# **树**


## 剑指offer


### 1、求树的深度
    求树的深度，可以从层次遍历考虑，层次遍历可以使用队列完成，也可以使用递归完成；

python版本
```python
class Solution:
    # 层次遍历 
    # 非递归版本
    def levelOrder(self, root):
        res=[] # save result value 层次遍历
        count = 0 # save 层数
        if root is None:
            return count
        q=[] # 模拟一个队列
        # 首先将根节点入队
        q.append(root)
        # 队列为空时，停止循环
        while len(q)!=0:
            tmp=[] # 使用列表存储同层节点
            length = len(q) # 记录同层节点的个数
            for i in range(length):
                r = q.pop(0) # 将同层节点依次出队
                if r.left is not None:
                    q.append(r.left) # 如果该节点的左孩子存在，则加入队列；
                if r.right is not None:
                    q.appemd(r.right) 
                tmp.appned(r.val)
            if tmp:
                count+=1 # 统计层数
            res.append(tmp)
        return count
    def TreeDepth(self,pRoot):
        if pRoot is None:
            return 0
        count = self.levelOrder(pRoot)
        return count
        
        
```
```python
class Solution:
    def TreeDepth(self,pRoot):
        if pRoot is None:
            return 0
        # 若只有根节点，则为1，若只有左子树，则深度为左子树的深度+1，同样，若只有右子树...
        # 若同时有左右子树，则左右子树的最大值+1；
        count = max(self.TreeDepth(pRoot.left),self.TreeDepth(pRoot.right))+1
        return count

```


C++版本
```javascript
//非递归版本
class Solution{
    public:
        int TreeDepth(TreeNode* pRoot){
            if(!pRoot) return 0;
            queue<TreeNode*> que;
            que.push(pRoot);
            int depth=0;
            while(!que.empty()){
                int size = que.size()
                depth++;
                for(int i=0;i<size;i++){
                    TreeNode* node = que.front();
                    que.pop();
                    if(node->left){
                        que.push(node->left)
                    }
                    if(node->right){
                        que.push(node->right)
                    }
                }
            return depth;
            }
        }
}
```
```javascript
//递归版本
class Solution{
    public:
        int TreeDepth(TreeNode* pRoot){
            if(!pRoot) return 0;
            return max(TreeDepth(pRoot->left),TreeDepth(pRoot->right))+1;
        }
}
```

-----
-----

### 2、平衡二叉树

输入一棵二叉树，判断是否是平衡二叉树。

C++版本

- 最直接的做法，遍历每个结点，借助一个获取树深度的递归函数，根据该结点的左右子树高度差判断是否平衡，然后递归地对左右子树进行判断。 
- 由上到下进行遍历，先判断根节点到叶子节点的左右高度是否是平衡的，然后再分别判断左右子树。
- 缺点：节点重复遍历。
```javascript
class Solution {
public:
    bool IsBalanced_Solution(TreeNode* pRoot) {
        if(pRoot==NULL)
            return true;
        int left=deep(pRoot->left);
        int right=deep(pRoot->right);
        int diff=left-right;
        if(diff>1||diff<-1)
            return false;
        return IsBalanced_Solution(pRoot->left)&&IsBalanced_Solution(pRoot->right);
    }
//求节点高度
private:
    int deep(TreeNode* pRoot)
    {
        if(pRoot==NULL)
            return 0;
        int left=deep(pRoot->left);
        int right=deep(pRoot->right);
        
        return (left>right)?left+1:right+1;
    }
};
```

- 改为从下往上遍历，如果子树是平衡二叉树，则返回子树的高度；如果发现子树不是平衡二叉树，则直接停止遍历，这样至多只对每个结点访问一次；
```javascript
class Solution {
public:
    bool IsBalanced_Solution(TreeNode* pRoot){
        return deep(pRoot)!=-1;
    }
private:
    int deep(TreeNode* pRoot)
    {
        if(pRoot==NULL)
            return 0;
        int left = deep(pRoot->left)
        if(left==-1)
            return -1;
        int right = deep(pRoot->right)
        if(right==-1)
            return -1;
        return abs(left-right)>1?-1:1+max(left,right);
    }
    
}
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


### 4、镜像/对称二叉树

- 思路:
    首先根节点以及其左右子树，左子树的左子树和右子树的右子树相同；   
    左子树的右子树和右子树的左子树相同即可，采用递归；   
    非递归也可，采用栈或队列存取各级子树根节点；   
    
    
递归版本：

```javascript
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};
*/
/*
思路：如果先序遍历的顺序分为两种先左后右和先右后左两种顺序遍历，如果两者相等说明二叉树是对称的二叉树
 
*/
class Solution {
public:
    bool isSymmetrical(TreeNode* pRoot)
    {
        return isSymmetrical(pRoot,pRoot);
    }
     
    bool isSymmetrical(TreeNode* pRoot1,TreeNode* pRoot2)
    {
        if(pRoot1==NULL&&pRoot2==NULL)
            return true;
        if(pRoot1==NULL || pRoot2==NULL)           
            return false;
        if(pRoot1->val!=pRoot2->val)
            return false;
        return isSymmetrical(pRoot1->left,pRoot2->right) && isSymmetrical(pRoot1->right,pRoot2->left);
         
    }
 
};
```

非递归版本：

只要采用前序、中序、后序、层次遍历等任何一种遍历方法，分为先左后右和先
右后左两种方法，只要两次结果相等就说明这棵树是一颗对称二叉树。
```javascript

class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root==NULL) return true;
        queue<TreeNode*> q1,q2;
        TreeNode *left,*right;
        q1.push(root->left);
        q2.push(root->right);
        while(!q1.empty() and !q2.empty())
        {
            left = q1.front();
            q1.pop();
            right = q2.front();
            q2.pop();
            //两边都是空
            if(NULL==left && NULL==right)
                continue;
            //只有一边是空
            if(NULL==left||NULL==right)
                return false;
             if (left->val != right->val)
                return false;
            q1.push(left->left);
            q1.push(left->right);
            q2.push(right->right);
            q2.push(right->left);
        }
         
        return true;
         
    }
};
```

### 5、之字形打印二叉树

请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

- 思路：
    打印第一层结点的时候要把第二层结点保存到一个容器里，由于打印第二层结点的顺序是从右至左，容器为后进先出，故容器为栈
打印第一层时，依次把第一层的左子结点和右子结点保存到栈中；打印第二层结点时要把第三层结点保存到栈中，由于第三层打印顺序为从左到右，
故保存到容器中的顺序为先右结点后左结点，和第一层是不一样，因此我们需要两个栈来分别保存奇数层和偶数层结点

C++版本

```javascript
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :val(x), left(NULL), right(NULL) {}
};
 
class Solution {
public:
	vector<vector<int> > Print(TreeNode* pRoot) {
		vector<vector<int>> Value{};
		if (pRoot == NULL)
			return Value; 
		stack<TreeNode*> levels[2];                   //levels[0]用来保存奇数层结点，levels[1]用来保存偶数层结点
		vector<int> currentLevel{};                   //用来存储当前层所有结点的值
		int current = 0;                              //0表示当前遍历的是奇数层，1表示当前遍历的是偶数层
		int next = 1;
		levels[current].push(pRoot);
		while (!levels[0].empty() || !levels[1].empty())
		{
			TreeNode* pNode = levels[current].top();
			levels[current].pop();
			currentLevel.push_back(pNode->val);
			if (current == 0)                     //如果当前遍历的是奇数层，依次将其左子结点和右子结点保存在levels[1]中
			{
				if (pNode->left != NULL)                       
					levels[next].push(pNode->left);
				if (pNode->right != NULL)
					levels[next].push(pNode->right);
			}
			else                                  //如果当前遍历的是偶数层，依次将其左子结点和右子结点保存在levels[0]中
			{
				if (pNode->right != NULL)
					levels[next].push(pNode->right);
				if (pNode->left != NULL)
					levels[next].push(pNode->left);
			}
			if (levels[current].empty())
			{
				current = 1 - current;
				next = 1 - next;
				Value.push_back(currentLevel);
				currentLevel.clear();
			}
		}
		return Value;
	}
};

```

### 6、打印二叉树

- 思路：层次遍历

C++版本

```javascript
class Solution {
public:
        vector<vector<int> > Print(TreeNode* pRoot) {
            vector<vector<int> > vec;
            if(pRoot == NULL) return vec;
 
            queue<TreeNode*> q;
            q.push(pRoot);
 
            while(!q.empty())
            {
                int lo = 0, hi = q.size();
                vector<int> c;
                while(lo++ < hi)
                {
                    TreeNode *t = q.front();
                    q.pop();
                    c.push_back(t->val);
                    if(t->left) q.push(t->left);
                    if(t->right) q.push(t->right);
                }
                vec.push_back(c);
            }
            return vec;
        }
};

```

