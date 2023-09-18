#### [493. 翻转对](https://leetcode-cn.com/problems/reverse-pairs/)

> 难度困难118
>
> 给定一个数组 `nums` ，如果 `i < j` 且 `nums[i] > 2*nums[j]` 我们就将 `(i, j)` 称作一个***重要翻转对\***。
>
> 你需要返回给定数组中的重要翻转对的数量。
>
> **示例 1:**
>
> ```
> 输入: [1,3,2,3,1]
> 输出: 2
> ```
>
> **示例 2:**
>
> ```
> 输入: [2,4,3,5,1]
> 输出: 3
> ```



```java
	//归并排序中进行统计 
    public int reversePairs(int[] nums) {
        if(nums == null || nums.length == 0) return 0;
        return mergeSort(nums,0,nums.length-1);
    }

    public int mergeSort(int [] nums,int l,int r){
        if(l>=r) return 0;
        int mid = l+(r-l)/2;
        int count = mergeSort(nums,l,mid)+mergeSort(nums,mid+1,r);
        int [] cache =  new int [r-l+1];
        int i = l,t = l, c = 0;
        for(int j = mid+1;j<=r;j++,c++){
            while(i<=mid && nums[i]<=2*(long)nums[j]) i++;
            while(t<=mid && nums[t]<=nums[j]) cache[c++] = nums[t++];
            cache[c] = nums[j];
            count+= mid-i+1;
        }

        while(t<=mid) cache[c++] =nums[t++];
        System.arraycopy(cache,0,nums,l,r-l+1);
        return count;
    }
```

