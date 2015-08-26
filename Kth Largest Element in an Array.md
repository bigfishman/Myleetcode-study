>##Kth Largest Element in an Array 

<p>

<div>题目：找到数组中第K大的数

```java

Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

For example,
Given [3,2,1,5,6,4] and k = 2, return 5.

Note: 
You may assume k is always valid, 1 ≤ k ≤ array's length.

```

>###解法一

<div>思路：先按照从大到小排序，然后求的第K个数,采用快速排序

```java

void quicksort(vector<int> &nums,int l,int h)
{
     int i;
     int low=l;
     int high=h;
     int x=nums[l];

     if(l>=h)
          return;

     while(low<high)
     {
          while(low<high&&nums[high]<=x)
               high--;
          if(low<high)
               nums[low]=nums[high];
 

          while(low<high&&nums[low]>=x)
               low++;
          if(low<high)
               nums[high]=nums[low];
   
     }
    nums[low]=x;

     quicksort(nums,l,low-1);
     quicksort(nums,low+1,h);

}

 int findKthLargest(vector<int>& nums, int k) {
        
        int len=nums.size()-1;
        
        quicksort(nums,0,len);
        
        return nums[k-1];
        
    }
```

>###解法二

<div>思路，其实在上面用到快速排序的时候，我们求了其中一个key，这个key是用来划分的，大于key的放左边，小于key的值放右边，如果这个key真是位于我们求的K位置上面，那么就可以直接返回了，因为一趟过后key的位置是不会变了的，也就是已经提前找到了所求的值

代码如下：

```java
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        
        int len=nums.size()-1;
        
        return findKth(nums,0,len,k);
        
    }
    
    int findKth(vector<int>&nums,int l,int h,int k)
    {
        if(l>=h)
            return nums[l];
            
        int low=l;
        int high=h;
        int key=nums[l];
        
        while(low<high)
        {
            while(low<high&&nums[high]<=key)
                high--;
            if(low<high)
                nums[low]=nums[high];
            while(low<high&&nums[low]>=key)
                low++;
            if(low<high)
                nums[high]=nums[low];
        }
        
        nums[low]=key;
        
        if(k==low+1)  //表示已经 找 到了，直接返回K处的值即可
            return nums[low];
        else if(k<low+1)   //K小于当前枢轴，那么在前面部分找
            return findKth(nums,l,low-1,k);
        else                      //K大于当前枢轴，那么在后面部分找
            return findKth(nums,low+1,h,k);
    }
};

```