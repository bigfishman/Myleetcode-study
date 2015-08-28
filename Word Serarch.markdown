>##Word Search  和  Word Search Ⅱ


<p>

>###Word Search

    Given a 2D board and a word, find if the word exists in the grid.

    The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

    For example,
    Given board =

    [
      ["ABCE"],
      ["SFCS"],
      ["ADEE"]
    ]
    word = "ABCCED", -> returns true,
    word = "SEE", -> returns true,
    word = "ABCB", -> returns false.
    
<div>题目意思：在矩阵中查找给定单词，如在

    ["ABCE"],
      ["SFCS"],
      ["ADEE"]
中，查找ABCCED，此时可以找到，返回true

<div>思路：利用回溯，定义一个二维数组dp[][],dp[i][j]表示单词在i,j点经过。从[0,0]开始，如果匹配，则往上下左右四个方向继续进行剩下的查找，直到找到完整的单词

```java

class Solution {
public:
    bool exist(vector<vector<char> > &board, string word) {
        
        if(word.length()==0)
        {
            return false;
        }
       const int m = board.size();
        const int n = board[0].size();
        vector<vector<bool> > vivisted(m, vector<bool>(n, false));
        int index=0;
        for(int i=0;i<board.size();i++)
        {
            for(int j=0;j<board[0].size();j++)
            {
                if(board[i][j]==word[0])
                
                {
                    vivisted[i][j]=true;
                    if(index==word.size()-1||dfsserach(board,word,index+1,i,j,vivisted))
                    {
                        return true;
                    }
                    vivisted[i][j]=false;
                }
            }
        }
        return false;
        
    }
    
    bool dfsserach(vector<vector<char>> &board,string word,int index,int i,int j, vector<vector<bool>> &vivisted)
    {
        if(index==word.size()){
            return true;
        }
        
        int  direction[4][2]={-1,0,0,1,1,0,0,-1};
        
        int k;
        for(k=0;k<4;k++)
        {
            int ii=i+direction[k][0];
            int jj=j+direction[k][1];
            
            if(ii>=0&&ii<board.size()&&jj>=0&&jj<board[0].size()&&board[ii][jj]==word[index]&&vivisted[ii][jj]==false)
            {
                vivisted[ii][jj]=true;
                if(index==word.size()-1||dfsserach(board,word,index+1,ii,jj,vivisted))
                {
                    return true;
                }
                vivisted[ii][jj]=false;
            }
        }
        
        return false;
    }
 
};
```

<hr>

>###Word SearchⅡ

    Given a 2D board and a list of words from the dictionary, find all words in the board.

    Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.
    
    For example,
    Given words = ["oath","pea","eat","rain"] and board =
    
    [
      ['o','a','a','n'],
      ['e','t','a','e'],
      ['i','h','k','r'],
      ['i','f','l','v']
    ]
    Return ["eat","oath"].
    
<div>题目意思是要在给定的矩阵中查找出现在单词字典中单词，如上面的eat和oath在 矩阵中可以找到

<div>思路：构建字典树，把字典中的单词，创建成字典树,Insert方法重要

```java

 struct TrieNode
    {
        TrieNode *child[26];
        string node;
        TrieNode():node("")
        {
            for(auto &a:child)
                a=NULL;
        }
    };
    struct Trie
    {
        TrieNode *root;
        Trie():root(new TrieNode()){}
        void insert(string s)
        {
            TrieNode *p=root;
            for(auto &str:s)
            {
                int i=str-'a';
                if(!p->child[i])
                {
                    p->child[i]=new TrieNode();
                    
                }
                p=p->child[i];
            }
            p->node =s;
        }
        
    };

```


<div>构建好字典树之后，根据字母矩阵，然后往字典树中进行查找，时间复杂度N^2 *O(字典查找)

代码如下
    
```java

 for(int i=0;i<board.size();i++)
        {
            for(int j=0;j<board[0].size();j++)
            {
                if(T.root->child[board[i][j]-'a'])
                {
                    dp[i][j]=true;//表示当前 节点 已经走过
                    WordSearch(board,T.root->child[board[i][j]-'a'],i,j,dp,result);
                    dp[i][j]=false;//去掉走过的痕迹
                }
            }
        }

```

<div>总代码如下：

```java

class Solution {
public:

    struct TrieNode
    {
        TrieNode *child[26];
        string node;
        TrieNode():node("")
        {
            for(auto &a:child)
                a=NULL;
        }
    };
    struct Trie
    {
        TrieNode *root;
        Trie():root(new TrieNode()){}
        void insert(string s)
        {
            TrieNode *p=root;
            for(auto &str:s)
            {
                int i=str-'a';
                if(!p->child[i])
                {
                    p->child[i]=new TrieNode();
                    
                }
                p=p->child[i];
            }
            p->node =s;
        }
        
    };
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        
        vector<string> result;
        if(board.empty()||board[0].empty()||words.empty())
            return result;
            
        vector<vector<bool>> dp(board.size(),vector<bool>(board[0].size(),false));
        Trie T;
        for(auto &s:words)
            T.insert(s);
        
        for(int i=0;i<board.size();i++)
        {
            for(int j=0;j<board[0].size();j++)
            {
                if(T.root->child[board[i][j]-'a'])
                {
                    dp[i][j]=true;//表示当前 节点 已经走过
                    WordSearch(board,T.root->child[board[i][j]-'a'],i,j,dp,result);
                    dp[i][j]=false;//去掉走过的痕迹
                }
            }
        }
        
        return result;
    }
    
    void WordSearch(vector<vector<char>>& board,TrieNode *p,int i,int j,vector<vector<bool>> &dp,vector<string> &result)
    {
        if(!p->node.empty())//如果当前字典树位于Node处，表示有单词,找到一次，将该单词处的标记清除，继续往下找
        {
            result.push_back(p->node);
            p->node.clear();
        }
        int  direction[4][2]={-1,0,0,1,1,0,0,-1};//定义四个方向
        
       for(int k=0;k<4;k++)
       {
           int new_i=i+direction[k][0];  //新的i
           int new_j=j+direction[k][1]; //新的j
           
           //跟上面同样的思想，把新的节点看成当前节点继续往前查找
           if(new_i>=0&&new_i<board.size()&&new_j>=0&&new_j<board[0].size()&&dp[new_i][new_j]==false&&p->child[board[new_i][new_j]-'a'])
           {
               dp[new_i][new_j]=true;
               WordSearch(board,p->child[board[new_i][new_j]-'a'],new_i,new_j,dp,result);
               dp[new_i][new_j]=false;
           }
       }
    }
};

```