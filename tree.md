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
