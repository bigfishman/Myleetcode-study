>##Kth Smallest Element in a BST 

<p>

<div>题目如下：

```java

Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

Note: 
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.


```

<div>题目意思是说求的二叉查找树的第K小的值

<div> 思路：

<li>我们知道二叉查找树的中序遍历为有序的，从小到大排列</li>
<li>求第K小的值就是从大到小排列的第K个树，既然这样，我们可以先访问左孩子，然后根节点，然后右孩子，这样得到的序列就是从大到小，在记录K的值就是</li>

<div>代码如下：（用栈记录节点，一个节点出栈，k--，直到k==1）

    重点注意，先访问左孩子，然后根节点，然后右孩子
    
```java

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        
        stack<TreeNode *> result;
        TreeNode *p=root;
        
        while(!result.empty()||p!=NULL)
        {
            while(p)
            {
                result.push(p);
                p=p->left;
            }
            
            if(!result.empty())
            {
                p=result.top();
                result.pop();
                if(k==1)
                    return p->val;
                else
                    k--;
                p=p->right;
            }
        }
    }
};
```

