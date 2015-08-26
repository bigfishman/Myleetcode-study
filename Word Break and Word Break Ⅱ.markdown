>##Word Break && Word Break II

<p>

>###Word Break

<div>题目：

```java

Given a string s and a dictionary of words dict, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

For example, given
s = "leetcode",
dict = ["leet", "code"].

Return true because "leetcode" can be segmented as "leet code".

```

<div>意思：判断一个字符串，是否可以由字典中的单词组成，如上面的leetcode可以由字典中的leet 和 code组成，可以的话，返回true，否则，返回false

<div>思路：定义一个数组dp[]，dp[i]表示字符串的前i个字符能否用字典表示，初始化dp[0]=true，i从1开始，那么对于前i+1个字符，我们需要判断在前i个字符串中存不存在一个点j，这个点dp[j]为true，并且[j,i]的字符串在字典中出现

    dp[j]&&substr(j,i-j)   此时的j 从 0 到 i-1
    
    拿上面的leetcode来说，当  i=4，此时表示leet单词是否在字典中存在，因为 dp[0]&&substr(0,4-0) 
    此时substr(0,4-0)=leet 在字典中存在，所以 dp[4]=true,依次往后求，直到求的dp[size].
    
代码：

```java

class Solution {
public:
    bool wordBreak(string s, unordered_set<string>& wordDict) {
        
        int size=s.size();
        vector<bool> dp(s.length() + 1, false);
       
        dp[0]=true;
        
        for(int i=1;i<=size;i++)
        {
            string temp;
            for(int j=0;j<i;j++)
            {
               
                if(dp[j]&&wordDict.find(s.substr(j,i-j))!=wordDict.end())
                {
                    dp[i]=true;
                    break;
                }
            }
        }
        
        return dp[size];
        
    }
};
```
    

<hr>

>###Word Break II

<div>思路，从第一个字符开始，往后找，找到一个在字典中出现的单词，然后用一个temp拼接，接着在生下来的字符串中进行查找，将temp传过去，一次类推，直到遍历完整个字符串。


代码如下：

```java

class Solution {
public:
    vector<string> wordBreak(string s, unordered_set<string>& wordDict) {
        
      vector<string> result;
      int len=s.size();
      string temp;
      for(int i=0;i<len;i++)
      {
          temp=s.substr(0,i+1);
          
          if(wordDict.find(temp)!=wordDict.end())
          {
              findNext(s,i+1,temp,wordDict,result);
              
          }
          
          temp="";
      }
      
      return result;
       
    }
    
    void findNext(string s,int start,string temp,unordered_set<string>& wordDict,vector<string>&result)
    {
        if(start>=s.length())
        {
            result.push_back(temp);
            return;
        }
        
        string now;
        for(int i=start;i<s.length();i++)
        {
            now=s.substr(start,i-start+1);
            if(wordDict.find(now)!=wordDict.end())
            {
                findNext(s,i+1,temp+' '+now,wordDict,result);
            }
        }
    }
};
```

下面有一中优化做法，参考博客:[Word berak Ⅱ](http://blog.csdn.net/kenden23/article/details/19543349 "")

    1 使用二维表vector<vector<int> >tbl记录，记录从i点，能否跳到下一个break位置。如果不能，那么tbl[i]就为空。如果可以，就记录可以跳到哪些位置。
    
    2 利用二维表优化递归回溯法。
    优化点：
    如果当前位置是start，但是tbl[start]为空，那么就是说，这个break位置不能break整个s串的，直接返    回上一层，不用搜索到下一层了。

```java

class Solution {
public:
    //2014-2-19 update
	vector<string> wordBreak(string s, unordered_set<string> &dict) 
	{
		vector<string> rs;
		string tmp;
		vector<vector<int> > tbl = genTable(s, dict);
		word(rs, tmp, s, tbl, dict);
		return rs;
	}
	void word(vector<string> &rs, string &tmp, string &s, vector<vector<int> > &tbl,
		unordered_set<string> &dict, int start=0)
	{
		if (start == s.length())
		{
			rs.push_back(tmp);
			return;
		}
		for (int i = 0; i < tbl[start].size(); i++)
		{
			string t = s.substr(start, tbl[start][i]-start+1);
			if (!tmp.empty()) tmp.push_back(' ');
			tmp.append(t);
			word(rs, tmp, s, tbl, dict, tbl[start][i]+1);
			while (!tmp.empty() && tmp.back() != ' ') tmp.pop_back();//tmp.empty()
			if (!tmp.empty()) tmp.pop_back();
		}
	}
	vector<vector<int> > genTable(string &s, unordered_set<string> &dict)
	{
		int n = s.length();
		vector<vector<int> > tbl(n);
		for (int i = n - 1; i >= 0; i--)
		{
			if(dict.count(s.substr(i))) tbl[i].push_back(n-1);
		}
		for (int i = n - 2; i >= 0; i--)
		{
			if (!tbl[i+1].empty())//if we can break i->n
			{
				for (int j = i, d = 1; j >= 0 ; j--, d++)
				{
					if (dict.count(s.substr(j, d))) tbl[j].push_back(i);
				}
			}
		}
		return tbl;
	}
};
```

<div>上面的代码我改了一点点代码，使得更加容易阅读

```java
class Solution {
public:
    //2014-2-19 update
	vector<string> wordBreak(string s, unordered_set<string> &dict) 
	{
		vector<string> rs;
		string tmp;
		vector<vector<int> > tbl = genTable(s, dict);
		word(rs, tmp, s, tbl, dict);
		return rs;
	}
	void word(vector<string> &rs, string tmp, string &s, vector<vector<int> > &tbl,
		unordered_set<string> &dict, int start=0)
	{
		if (start == s.length())
		{
			rs.push_back(tmp);
			return;
		}
		for (int i = 0; i < tbl[start].size(); i++)
		{
			string t = s.substr(start, tbl[start][i]-start+1);
			if (!tmp.empty()) 
		    {
			    word(rs, tmp+' '+t, s, tbl, dict, tbl[start][i]+1);
		    }
		    else
		    {
		         word(rs, t, s, tbl, dict, tbl[start][i]+1);
		    }
		
		}
	}
	vector<vector<int> > genTable(string &s, unordered_set<string> &dict)
	{
		int n = s.length();
		vector<vector<int> > tbl(n);
		for (int i = n - 1; i >= 0; i--)
		{
			if(dict.count(s.substr(i))) tbl[i].push_back(n-1);
		}
		for (int i = n - 2; i >= 0; i--)
		{
			if (!tbl[i+1].empty())//if we can break i->n
			{
				for (int j = i, d = 1; j >= 0 ; j--, d++)
				{
					if (dict.count(s.substr(j, d))) tbl[j].push_back(i);
				}
			}
		}
		return tbl;
	}
};

```
<div>在判断的时候，如果为空，就表示是第一个单词，不用加空格，否则加空格，而上面的代码谢了很多还不易于理解