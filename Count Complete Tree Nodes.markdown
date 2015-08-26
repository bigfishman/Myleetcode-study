>##Count Complete Tree Nodes

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
    int countNodes(TreeNode* root) {
        
        if(root==NULL)
            return 0;
        int left=LeftNode(root);
        int right=RightNode(root);
        
        if(left==right)
        {
            return (2<<left)-1;
        }
        return countNodes(root->left)+countNodes(root->right)+1;
        
    }
    
    int LeftNode(TreeNode *root)
    {
        int count=0;
        
        while(root->left!=NULL)
        {
            count++;
            root=root->left;
        }
        
        return count;
    }
    
    int RightNode(TreeNode *root)
    {
        int count=0;
        while(root->right!=NULL)
        {
            count++;
            root=root->right;
        }
        return count;
    }
};

```