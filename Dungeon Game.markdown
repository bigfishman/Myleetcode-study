>##Dungeon Game 

<p>
<div>题目

```java

The demons had captured the princess (P) and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of M x N rooms laid out in a 2D grid. Our valiant knight (K) was initially positioned in the top-left room and must fight his way through the dungeon to rescue the princess.

The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.

Some of the rooms are guarded by demons, so the knight loses health (negative integers) upon entering these rooms; other rooms are either empty (0's) or contain magic orbs that increase the knight's health (positive integers).

In order to reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.


Write a function to determine the knight's minimum initial health so that he is able to rescue the princess.

For example, given the dungeon below, the initial health of the knight must be at least 7 if he follows the optimal path RIGHT-> RIGHT -> DOWN -> DOWN.

```
<table>
<tr>
<td>-2 (开始点)</td>
<td>-3  &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td>3 &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
</tr>
<tr>
<td>-5</td>
<td>-10  </td>
<td>1 </td>
</tr>
<tr>
<td>10</td>
<td>30  &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td>-5（结束点） </td>
</tr>
</table>
```
Notes:

The knight's health has no upper bound.
Any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.

```

<div>题目大致意思：

```
恶魔抓走了公主(P)并把她囚禁在地牢的右下角。地牢包含M x N个房间，排列成一个二维的格子。我们英勇的骑士(K)一开始位于左上角的房间里，需要一路披荆斩棘营救公主。

骑士拥有一个正整数的初始生命值。如果在任何一点其生命值≤0，他立刻会死去。

一些房间由恶魔守卫着，因此当骑士进入这些房间时就会损失生命值（负整数）；其他房间或者是空的（数值为0），或者包含一些魔力宝珠（magic orbs）可以增加骑士的生命值（正整数）。

为了尽可能快的解救公主，骑士决定每一步只向右或者向下移动。

编写一个函数决定骑士的最小初始生命值，确保他可以成功营救公主。

例如，给定下面的地牢，骑士的初始生命值至少应当为7，如果他按照下面的最优路线行进：右 -> 右 -> 下 -> 下.
```
<table>
<tr>
<td>-2 (开始点)</td>
<td>-3  &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td>3 &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
</tr>
<tr>
<td>-5</td>
<td>-10  </td>
<td>1 </td>
</tr>
<tr>
<td>10</td>
<td>30  &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td>-5（结束点） </td>
</tr>
</table>
```
备注：

骑士的生命值没有上界

任何房间都可能包含威胁或者补给，即使骑士进入的第一个房间或者囚禁公主的最后一个房间也一样。

```

<hr>
<div>思路:从表格开始入手，因为要使得达到最后的节点（结束点）血量大于0

<table>
<tr>
<td>-2 (开始点)</td>
<td>-3  &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td>3 &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
</tr>
<tr>
<td>-5</td>
<td>-10  </td>
<td>1 </td>
</tr>
<tr>
<td>10</td>
<td>30  &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td>-5（结束点） </td>
</tr>
</table>


<div>我们定义一个数组dp[i][j],表示骑士在点i,j处至少需要的血量，又因为血量必须大于0，并且从最后一个点往前面类推，运用动态规划的思路，对于结束点，我们有如下dp数组

<table>
<tr>
<td> (开始点)</td>
<td>  &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td> &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
</tr>
<tr>
<td></td>
<td>   &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td> </td>
</tr>
<tr>
<td></td>
<td>  &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td>6（结束点） </td>
</tr>
</table>
<div>这里的6怎么来的：因为在最后一格需要消耗-5，那么骑士达到此处是至少需要6点血量，使得6+（-5）=1>0,这样才不会死掉

<div>这样我们可以类推，最后一行，和最后一列的数据

    为什么是最后一行，最后一列，因为如果骑士此时处于最后一行或者最后一列的某一个点，只有一条路径达到结束点（最后一行，往右走，最后一列，往下走）
    
```java
  for(int i=row-2;i>=0;i--)
            dp[i][col-1]=Max(dp[i+1][col-1]-dungeon[i][col-1],1);
            
        for(int i=col-2;i>=0;i--)
            dp[row-1][i]=Max(dp[row-1][i+1]-dungeon[row-1][i],1);

```
<div>初始化得到最后一行，和最后一列的数据

<table>
<tr>
<td> (开始点)</td>
<td>  &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td> 2&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
</tr>
<tr>
<td></td>
<td>   &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td>5 </td>
</tr>
<tr>
<td>1</td>
<td>1  &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td>6（结束点） </td>
</tr>
</table>

<div>然后根据求的的最后一行和最后一列的数据，用动态规划思想，求的其他点的最少血量

<table>
<tr>
<td> 7(开始点)</td>
<td>  5&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td> 2&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
</tr>
<tr>
<td>6</td>
<td> 11  &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td>5 </td>
</tr>
<tr>
<td>1</td>
<td>1  &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp</td>
<td>6（结束点） </td>
</tr>
</table>

最后返回 dp[0][0]即可

代码如下：

```java

class Solution {
public:
    int calculateMinimumHP(vector<vector<int>>& dungeon) {
        
        int row=dungeon.size();
        int col=dungeon[0].size();
        
        int dp[row][col]; //表示到某一点是至少需要 的 血 ，从最后一个点开始，最少需要6，因为6-5=1>0
        
         dp[row-1][col-1]=Max(1-dungeon[row-1][col-1],1) ;//表示最后一个需要的
        
        for(int i=row-2;i>=0;i--)
            dp[i][col-1]=Max(dp[i+1][col-1]-dungeon[i][col-1],1);
            
        for(int i=col-2;i>=0;i--)
            dp[row-1][i]=Max(dp[row-1][i+1]-dungeon[row-1][i],1);
            
        for(int i=row-2;i>=0;i--)
        {
            for(int j=col-2;j>=0;j--)
            {
                int right=Max(dp[i][j+1]-dungeon[i][j],1);
                int down=Max(dp[i+1][j]-dungeon[i][j],1);
                
                dp[i][j]=Min(right,down);
            }
        }
        
        return dp[0][0];
    }
    
    int Max(int a,int b)
    {
        return a>b?a:b;
    }
    int Min(int a,int b)
    {
        return a>b?b:a;
    }
};
```