>##Lowest Common Ancestor of a Binary Search Tree 

<p>

<div>题目：在二叉查找树中给定两个点p，q，求p，q的最低公共父节点，p，q可以是父子关系

```java
Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”

        _______6______
       /              \
    ___2__          ___8__
   /      \        /      \
   0      _4       7       9
         /  \
         3   5
For example, the lowest common ancestor (LCA) of nodes 2 and 8 is 6. Another example is LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```
<div>如上面的2，8的最低公共父节点是6，2，4的最低父节点是2

>###解法一

<div>思路：采用跟求一般树公共父节点的做法，用递归的方法，如果p，q分别在左右节点则，当前根节点就是最低父节点，如果p,q都在右孩子或者p，q都在左孩子，那么就在右孩子，或者左孩子中递归求解。

代码如下：

```java


TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root || root == p || root == q)
         return root;
         
    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    
    TreeNode* right = lowestCommonAncestor(root->right, p, q);
    
    if(left&&rigth)
        return root;
    else if(left!=NULL)
        return left;
    else
        return right;
}

```

>###解法二

<div>上面的做法，效率太低，有很多节点会重复遍历，那么如果我们事先用一个list把两个节点找到，然后从根开始分别比较，最后一个相同的节点就是要求的节点

代码如下

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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        
        
        list<TreeNode*> p_path;
        list<TreeNode*> q_path;
        
        TreeNode *p_temp=root;
        TreeNode *q_temp=root;
        
        while(p_temp)
        {
             p_path.push_back(p_temp);
            if(p_temp->val>p->val)
            {
               
                p_temp=p_temp->left;
                
            }
            else if(p_temp->val<p->val)
            {
                p_temp=p_temp->right;
            }
            else
                break;
        }
        
        while(q_temp)
        {
            q_path.push_back(q_temp);
            if(q_temp->val>q->val)
            {
                q_temp=q_temp->left;
            }
            else if(q_temp->val<q->val)
            {
                q_temp=q_temp->right;
            }
            else
                break;
        }
        
        TreeNode * pLast = NULL;  
    list<TreeNode*>::const_iterator iter1 = p_path.begin();  
    list<TreeNode*>::const_iterator iter2 = q_path.begin();  
    while(iter1 != p_path.end() && iter2 != q_path.end())  
    {  
        if(*iter1 == *iter2)  
            pLast = *iter1;  
        else  
            break;  
        iter1++;  
        iter2++;  
    }  
    
    return pLast;
        
    }
};

```

>###解法三

<div>解法二相比一来说提高了效率，但是并不是最简洁的，浪费了空间，并且代码变的复杂多了，其实我们应该利用的是二叉查找树的特点来进行。

```
1、解法1中提到，如果p,q分别在root的左右孩子，那么root就是父节点，这在二叉查找树中就表示为，p,q中有一个大于root，一个小于root
2、同样，如果两个都大于或者两个都小于root，那就表示去右孩子或者左孩子中进行查找

```

代码如下：

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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
       
    while(root!=NULL)
    {
        if(root->val<p->val&&root->val<q->val)
            root=root->right;
        else if(root->val>p->val&&root->val>q->val)
            root=root->left;
        else
            break;
    }
    
    return root;
        
    }
};

```