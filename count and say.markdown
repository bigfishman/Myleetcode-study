>##Count and Say --leetcode

<p>

<div>题目如下

``` java
The count-and-say sequence is the sequence of integers beginning as follows:
1, 11, 21, 1211, 111221, ...

1 is read off as "one 1" or 11.
11 is read off as "two 1s" or 21.
21 is read off as "one 2, then one 1" or 1211.
Given an integer n, generate the nth sequence.

Note: The sequence of integers will be represented as a string.

```

<div>题目的意思是要递推，求第N词出现的字符串.（提取句子中出现的数字，如下解析）

如第一次: 1 (1个1)     

第二次，在一次的基础上1求： 1个1       11（2个1）

第三次在第二次的基础上11求： 2个1      21（1个2，1个1）

第四次在第三次的基础上21求： 1个2，1个1         1211（1个1，1个2，2个1）

第五次在第四次基础上1211求：1个1，1个2，2个1     111221   

<div>注意，如果出现相同的 ，如第四次有1211 ，后面两位11出现了重复，那么需要统计总次数2，就是2个1，没有出现重复的就用1个表示

<div>代码思路就是采用递推的方法，初始为“1”，然后递推求的下一个字符串result，然后替换掉初始字符串,中间需要用一个count记录出现的重复次数，我们用上面的第四次求的第五次为例：

对于 
```java
  temp="1211"  此时初始的值

  result=”“  此时的值，中间值
  
然后用 j=0;while（j<temp.length()）循环遍历 temp，用 t=j+1指向j的下一个字符，看看是不是相等，求的count的个数

第一次：  t指向2  此时 不 相等  那么count=1，  result=count+temp[j]=11;  令j=t；
第二次：j指向2，t指 向 1 ， 此时不相等，那么count=1；result=count+temp[j]=11+12=1112;
第三次：j指向1，t指向1，相等，那么t继续往前，count+1,直到字符结束，count=2,result=1112+count+temp[j]=1112+2+1=111221
      
```


如下：

```java

class Solution {
public:
    string countAndSay(int n) {
        
        if(n<1)
            return "";
        
        string temp="1";
		string result;
	
		for(int i=2;i<=n;i++)
		{
			result="";
			int j=0;
			while(j<temp.length())
			{
				int t=j+1;
				int count=1;

				while(t<temp.length()&&temp[j]==temp[t])
				{
					count++;
					t++;
				}

				result+=(count+'0');
				result+=temp[j];
				j=t;
			}

			temp=result;
		}
		
		return temp;
        
    }
};
```

