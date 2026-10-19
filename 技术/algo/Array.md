# Array

[TOC]

高频面试题

![](e://pic/7430894_1569830591035_8361FA23DFD51FBD74ED3E8B998C124D.png)

## 283.移动零 ☆

> 给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。
>
> **示例:**
>
> ```
> 输入: [0,1,0,3,12]
> 输出: [1,3,12,0,0]
> ```
>
> **说明**:
>
> 1. 必须在原数组上操作，不能拷贝额外的数组。
> 2. 尽量减少操作次数。

### -1.空间最优，操作局部优化（双指针）

```java
//主要思路  loop 2 次  第一次将所有非0元素移动到应该在的地方。
//第二次将后面的全部设置为0 
public void moveZeroes(int[] nums) {
        if (nums == null) return;
        int cnt = 0;
        for (int num : nums) {
            if (num != 0) {
                nums[cnt++] = num; 
            }
        }

        while (cnt < nums.length) {
            nums[cnt++] = 0;
        }
    }
```

时间复杂度为O(n)

### 2.一次loop

```java
   /***
   * 思路:快排的思想找到一个中间点的数 比如 -21 -23 4 5 0 找到一个midNum数 作为标尺 来计算
     * 因此 在本题中找出所有0 用0作为一个标尺 0左边的都是非0 右边是0  
     * 通过一个loop就可以了。
     * @param nums
     */
    public void moveZeroes(int[] nums) {
        if (nums.length == 0){
            return;
        }
		// i=0  j=0
        int j = 0;
        for (int i = 0; i < nums.length ; i++) {
            if (nums[i] != 0){
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j++] = temp;
            }
        }
    }
```

时间复杂度 O(n)

### 3.滚雪球

```java
/***
     * 滚雪球
     *  一次loop 记录0的元素，如果不为0 与前面的元素进行交换  直到最后。
     * @param nums
     */
    public void moveZeroes(int[] nums) {
        if (nums.length == 0){
            return;
        }

        int ballSize = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0){
                ballSize++;
            }else if (ballSize>0){
                nums[i-ballSize] = nums[i];
                nums[i] = 0;
            }
        }
    }
```

时间复杂度:O(n)







## 70.爬楼梯  ☆

> 假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
>
> 每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
>
> 注意：给定 n 是一个正整数。
>
> 示例 1：
>
> 输入： 2
> 输出： 2
> 解释： 有两种方法可以爬到楼顶。
>
> 1. 1 阶 + 1 阶
> 2. 2 阶

### 1.暴力破解

思路:我们可以将问题简单化，一个2个台阶可以分成是走2步和走1步结果的和。依次调用这个函数。进行细化求解。

```java
public static int climbStairs(int n) {
        if ( n == 0){
            return 0;
        }

        if ( n== 1){
            return 1;
        }
        return climbStairs(n-1)+climbStairs(n-2);
    }
```

时间复杂度：O(2^n)  由于时间过程 提交失败 

空间复杂度：O(n)

### -2.动态规划

不难发现，这个问题可以被分解为一些包含最优子结构的子问题，即它的最优解可以从其子问题的最优解来有效地构建，我们可以使用动态规划来解决这一问题。

dp[i]=dp[i−1]+dp[i−2]

```java
 public int climbStairs(int n) {
       if(n == 1){
           return 1;
       }

       int [] num = new int [n+1];

       num[0] = 1;
       num[1] = 1;

       for(int i=2;i<num.length;i++){
           num[i] = num[i-1]+num[i-2];
       }
       return num[n];
    }
```

## 11.container-with-most-water☆

> 给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
>
> 说明：你不能倾斜容器，且 n 的值至少为 2。
>
> ```
> 输入：[1,8,6,2,5,4,8,3,7]
> 输出：49
> ```

### 1.枚举

思路:枚举 类似冒泡一样 遍历每个元素与另一个元素的大小 **下标值为长   数组的值为宽**

时间复杂度:O(n^2)  

```java
public int maxArea(int[] height) {
        int maxArea = 0;
        for (int i = 0; i < height.length-1 ; i++) {
            for (int j = i+1; j < height.length; j++) {
                int area = (j-i)*Math.min(height[i],height[j]);//长*宽
                maxArea = Math.max(maxArea,area);
            }
        }
        return maxArea;
    }
```

### -2.左右夹逼

思路:通过依次loop i从最前面开始 向右移动 j从最后面向前移动 每次比较 如果较小，继续寻找更大的。

时间复杂度:O(n)

```
public int maxArea(int[] height) {
        int maxArea = 0;
        for (int i = 0,j = height.length-1; i < j ; ) {
            //计算出最小的高
            int minHeight = height[i] <height[j] ? height[i++] : height[j--];
            int area = (j-i+1)*minHeight;
            maxArea = Math.max(area,maxArea);
        }
        return maxArea;
    }
```

## 27.[移除元素](https://leetcode-cn.com/problems/remove-element/)

> 给定一个数组 nums 和一个值 val，你需要原地移除所有数值等于 val 的元素，返回移除后数组的新长度。
> * 不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。
> * 元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
> * 示例 1:
> * 给定 nums = [3,2,2,3], val = 3,
> * 函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。
> * 你不需要考虑数组中超出新长度后面的元素。

### 1.拷贝覆盖

主要思路:定义一个变量，对不重复的值计数。for循环完成。

存在的问题 就是值可以计算出当前不相同值，数组的值是不符合要求的。

**这种思路在移除元素较多时更适合使用，最极端的情况是全部元素都需要移除，遍历一遍结束即可**

**时间复杂度:O(n) 空间复杂度O(1)**

```java
public static int removeElement3(int [] nums,int val){
        int count = 0;
        for (int num:nums) {
            if (num!=val){
                nums[count++] = num;
            }
        }
        return count;
    }
```

### 2.交换移除

主要思路:用一个变量去记录数组的长度，然后处理两种情况，一种是当前的值等于val 就将数组中最后的值赋值给当前相同的值，m 用来不断减少数组的长度。否则的话 是另一种情况 如果不相等 直接i++ 判断下一个就可以。

**这种思路在移除元素较少时更适合使用，最极端的情况是没有元素需要移除，遍历一遍结束即可**

时间复杂度：O(n)，空间复杂度：O(1)

```java
public static int removeElement4(int [] nums,int val){
        int m = nums.length;
        for (int i = 0; i < m; ) {//注意for 这里不能i++ i是用来确认哪些数据不是要寻找的
            if (nums[i] == val){
                nums[i] = nums[m-1];
                m--;
            }else {
                i++;
            }
        }
        return m;
    }
```

### 3.双指针

实现思路:一个快指针 一个慢指针。用快指针来判断是否相等 不相等赋值给慢指针，慢指针的作用是来定位前面如果有重复的直接跳过去。

时间复杂度:o(n)

空间复杂度:o(1)

```java
public static int removeElement5(int [] nums,int val){
        int i = 0;
        for (int j = 0; j < nums.length; j++) {
            if (nums[j]!=val){
                nums[i] = nums[j];
                i++;//加1
            }
            //如果相等 不做 等待下标i来进行覆盖
        }
        return i;
    }
```

### 4.双指针-要删除的元素很少时

实现思路:通过快慢指针 在一种比较极端的情况下。nums[1,2,3,5,4] val = 4 在最后才可以找到需要删除的元素，虽然nums中的值没有改变，但是通过m-- 停止了循环 i=4。

当我们遇到 nums[i] = valnums[i]=val 时，我们可以将当前元素与最后一个元素进行交换，并释放最后一个元素。这实际上使数组的大小减少了 1。

时间复杂度:O(n)

空间复杂度:O(1)

```java
public static int removeElement6(int [] nums,int val){
        int i = 0;
        int m = nums.length;
        while (i<m){
            if (nums[i] == val){//相当的话 直接将快指针元素赋值给慢指针
                nums[i] = nums[m-1];
                m--;
            }else {
                i++;//不相等 直接跳过
            }
        }
        return i++;
    }
```

## 35.搜索插入位置

> 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
>
> 你可以假设数组中无重复元素。
>
> 示例 1:
>
> 输入: [1,3,5,6], 5
> 输出: 2

### 1.二分查找

实现思路: 使用二分查找就可以了。如果使用for循环的话，时间复杂度为O(n) 但是使用二分查找 时间复杂度就下降了很多，因此 注意边界的控制。

1.当target与查询的mid值相等 返回

2.当mid的值大于target max = mid-1;

3.当mid的值小于target min = mid+1

时间复杂度:o(logn)

注意边界的控制。

```java
public int searchInsert(int[] nums, int target) {
        int min = 0;
        int max = nums.length-1;
        int mid;
        int index = 0;
        while (min<=max){
            //如果找到
            mid = (min+max)/2;
            if (nums[mid] == target){
                return mid;
            }else if (nums[mid]<target){
                min = mid+1;
            }else {
                max = mid-1;
            }
        }
        return min;
    }
```

## 1.两数之和 ☆

> 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
>
> 你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。
>
> 示例:
>
> 给定 nums = [2, 7, 11, 15], target = 9
>
> 因为 nums[0] + nums[1] = 2 + 7 = 9
> 所以返回 [0, 1]

### 1.暴力破解

thinking:通过两次loop 就可以找到，但是时间复杂度为O(n^2)

```java
public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i+1; j < nums.length; j++) {
                if (target-nums[i] == nums[j]){
                    return new int []{i,j};
                }
            }
        }
        throw  new IllegalArgumentException("no find two sum !");
    }
```

### -2.哈希表

thiking：将数组中元素值作为key 存储到hashmap中 然后取寻找。遍历就可以了。

时间复杂度:O(n)

```java
 public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> hashMap = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            hashMap.put(nums[i],i);//将值作为key 可以保证数据不会出现重复
        }

        for (int i = 0; i < nums.length ; i++) {
            int num = target - nums[i];
            if (hashMap.containsKey(num) && hashMap.get(num)!=i){
                return new int []{i,hashMap.get(num)};
            }
        }
        throw  new IllegalArgumentException("no find!");
    }
```

```java
	public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> hashMap = new HashMap<>();

        for (int i=0; i < nums.length; i++) {
            if (hashMap.containsKey(target-nums[i])) {
                return new int [] {i,hashMap.get(target-nums[i])};
            } else {
                hashMap.put(nums[i],i);
            }
        }
        return new int [2];
    }
```



## 15.三数之和  ☆

> 给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。
>
> 注意：答案中不可以包含重复的三元组。
>
> 示例：
>
> 给定数组 nums = [-1, 0, 1, 2, -1, -4]，
>
> 满足要求的三元组集合为：
> [
>   [-1, 0, 1],
>   [-1, -1, 2]
> ]
>

### 1.暴力求解

thinking:通过三层loop  这里如果使用ArrryList 是有序 可重复，不能避免元素重复问题。但是使用

LikedHashSet就可以**避免元素重复问题**。

时间复杂度:O(n^3)

```java
public List<List<Integer>> threeSum(int[] nums) {
       if(nums == null || nums.length <=2){
           return Collections.emptyList();
       }

       Arrays.sort(nums);

       Set<List<Integer>> result = new LinkedHashSet<>();
       for(int i=0;i<nums.length;i++){
           for(int j=i+1;j<nums.length;j++){
               for(int k = j+1;k<nums.length;k++){
                   if(nums[i]+nums[j]+nums[k] == 0){
                       List<Integer> list = Arrays.asList(nums[i],nums[j],nums[k]);
                       result.add(list);
                   }
               }
           }
       }
       return new ArrayList(result);
}
```

### 2.哈希表

```java
/**
    * hash slow
    * 1406 ms	46 MB
    *
    * @param nums
    * @return
    */
private List<List<Integer>> hashSolution(int[] nums) {
    if (nums == null || nums.length <= 2) {
        return Collections.emptyList();
    }
    Set<List<Integer>> result = new LinkedHashSet<>();

    for (int i = 0; i < nums.length - 2; i++) {
        int target = -nums[i];
        Map<Integer, Integer> hashMap = new HashMap<>(nums.length - i);
        for (int j = i + 1; j < nums.length; j++) {
            int v = target - nums[j];
            Integer exist = hashMap.get(v);
            if (exist != null) {
                List<Integer> list = Arrays.asList(nums[i], exist, nums[j]);
                list.sort(Comparator.naturalOrder());
                result.add(list);
            } else {
                hashMap.put(nums[j], nums[j]);
            }
        }
    }

    return new ArrayList<>(result);
}

```

### 3.夹逼法

thingking:左右夹逼，三数求和 a+b+c = 0  等价于 a+b=c   将数组进行排序，将c固定 a设置到c的下一个问题，也就是head  b设置到最后一个位置 tail位置。  

第一次遍历c固定 head 和tail 分别向中间移动，判断如果-sum < target 说明值大 因为是负数。所以tail 左移动，否则就是head右移动。如此一轮后 如果找不到，接着c++ head 和tail接着判断。

就可以找到。时间复杂度相对于上面两种是比较低的。

时间复杂度:O(n)

```java
public List<List<Integer>> threeSum(int[] nums) {
        if (nums.length == 0 || nums.length <=2){
            return Collections.emptyList();
        }

        //元素去重
        Set<List<Integer>> result = new LinkedHashSet<>();

        //排序
        Arrays.sort(nums);

        for (int i = 0; i < nums.length-2; i++) {
            int head = i+1;//左端
            int tail = nums.length-1;//右端
            while (head<tail){
                int sum = -(nums[head]+nums[tail]);//取负值
                if (sum == nums[i]){
                    List<Integer> list = 			Arrays.asList(nums[i],nums[head],nums[tail]);
                    result.add(list);
                }
                if(sum<=nums[i]){
                    tail--;
                }else {
                    head++;
                }
            }
        }
        return new ArrayList<>(result);
    }
```

## 26.删除排序数组中的重复项   ☆

> 给定一个排序数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
>
> 不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。
>
>  
>
> 示例 1:
>
> 给定数组 nums = [1,1,2], 
>
> 函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 
>
> 你不需要考虑数组中超出新长度后面的元素。

### 1.双指针法

th: 一个快指针 一个慢指针 慢指针记录不重复的个数以及下标的作用，而快指针如果和慢指针中元素中出现相等的情况 不用添加直接跳过。

时间复杂度:O(n)

空间复杂度：只是用到了 临时变量 所以O(1) 

```java
 public int removeDuplicates(int[] nums) {
   if(nums.length == 0 || nums.length <2){
     return 0;
   }
        //双指针法
        int i =0;
        for(int j=1;j<nums.length;j++){
            if(nums[j] != nums[i]){  
                i++;
                nums[i] = nums[j];
            }
        }
        return i+1;//因为i从零 所以实际上不重复的个数为i+1
    }
```

##189.旋转数组

> 给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。
>
> 示例 1:
>
> 输入: [1,2,3,4,5,6,7] 和 k = 3
> 输出: [5,6,7,1,2,3,4]
> 解释:
> 向右旋转 1 步: [7,1,2,3,4,5,6]
> 向右旋转 2 步: [6,7,1,2,3,4,5]
> 向右旋转 3 步: [5,6,7,1,2,3,4]
> 示例 2:
>
> 输入: [-1,-100,3,99] 和 k = 2
> 输出: [3,99,-1,-100]
> 解释: 
> 向右旋转 1 步: [99,-1,-100,3]
> 向右旋转 2 步: [3,99,-1,-100]

### 1.暴力求解

th:k次决定了移动的次数。每移动一次就动全部元素，因为元素在变化 所有取最后一个元素就可以了。

time : O(k*n) 移动n个元素  k次

space:额外空间o(1)

```java
 //暴力
    public void rotate(int[] nums, int k) {
        int previous=0,temp = 0;
        for(int i=0;i<k;i++){
            previous = nums[nums.length-1];
            for(int j = 0;j<nums.length;j++){
                temp = nums[j];
                nums[j] = previous;
                previous = temp;
            }
        }
    }
```

### 2.使用额外的数组

创建一个新的数组，把移动的元素的位置固定好，关系表达式为 (k+i)%nums.length  

time:O(n)

space:O(n)

```java
//使用额外的数组
    public void rotate(int[] nums, int k) {
       int [] newArr = new int [nums.length];
       for(int i=0;i<nums.length;i++){
           newArr[(i+k)%nums.length] = nums[i];
       }
       
       for(int i=0;i<nums.length;i++){
           nums[i] = newArr[i];
       }
    }
```

还有两个答案 有经历看看

<https://leetcode-cn.com/problems/rotate-array/solution/xuan-zhuan-shu-zu-by-leetcode/>

## 88. 合并两个有序数组 ☆

> 给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 num1 成为一个有序数组。
>
>  
>
> 说明:
>
> 初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。
> 你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。
>
>
> 示例:
>
> 输入:
> nums1 = [1,2,3,0,0,0], m = 3
> nums2 = [2,5,6],       n = 3
>
> 输出: [1,2,2,3,5,6]

### 1.合并后在排序

时间复杂度:O((m+n)log(m+n))

空间复杂度：O(1)

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
	// nums2 要拷贝的数组 0 开始下标  nums1 拷贝的位置 m 个位置开始 长度为n
    System.arraycopy(nums2, 0, nums1, m, n);
    Arrays.sort(nums1);
  }
```

### 2.双指针/从前往后

时间复杂度:O(m+n)

空间复杂度:O(m)

```java
//双指针
    public void merge(int[] nums1, int m, int[] nums2, int n) {
         // Make a copy of nums1.
    int [] nums1_copy = new int[m];
    System.arraycopy(nums1, 0, nums1_copy, 0, m);

    // Two get pointers for nums1_copy and nums2.
    int p1 = 0;
    int p2 = 0;

    // Set pointer for nums1
    int p = 0;

    // Compare elements from nums1_copy and nums2
    // and add the smallest one into nums1.
    while ((p1 < m) && (p2 < n))
      nums1[p++] = (nums1_copy[p1] < nums2[p2]) ? nums1_copy[p1++] : nums2[p2++];

    // if there are still elements to add
    if (p1 < m)
      System.arraycopy(nums1_copy, p1, nums1, p1 + p2, m + n - p1 - p2);
    if (p2 < n)
      System.arraycopy(nums2, p2, nums1, p1 + p2, m + n - p1 - p2);

  }
```

### 3.双指针/从后往前

time:O(m+n)

space:O(1)

```java
class Solution {
  public void merge(int[] nums1, int m, int[] nums2, int n) {
    // two get pointers for nums1 and nums2
    int p1 = m - 1;
    int p2 = n - 1;
    // set pointer for nums1
    int p = m + n - 1;

    // while there are still elements to compare
    while ((p1 >= 0) && (p2 >= 0))
      // compare two elements from nums1 and nums2 
      // and add the largest one in nums1 
      nums1[p--] = (nums1[p1] < nums2[p2]) ? nums2[p2--] : nums1[p1--];

    // add missing elements from nums2
    System.arraycopy(nums2, 0, nums1, 0, p2 + 1);
  }
}

==========第二种方式
// p1指向nums1数组末尾  p2指向num2数组末尾 p3指向整个空间m+n-1末尾 
    // 从尾部添加
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int p1 = m-1, p2 = n-1, p3 = m+n-1;
        while (p2>=-0) {
            if (p1>=0 && (nums1[p1] > nums2[p2])) {
                nums1[p3--] = nums1[p1--];
            } else {
                nums1[p3--] = nums2[p2--];
            }
        }
    }
```

## 66.加1

> 给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。
>
> 最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。
>
> 你可以假设除了整数 0 之外，这个整数不会以零开头。
>
> 示例 1:
>
> 输入: [1,2,3]
> 输出: [1,2,4]
> 解释: 输入数组表示数字 123。
> 示例 2:
>
> 输入: [4,3,2,1]
> 输出: [4,3,2,2]
> 解释: 输入数组表示数字 4321。

th:

一共三种情况 

​    1.不存在进位  123  -》 124 

​    2.数字长度不变  199  -> 200  

​    3.数字长度加1   99  -》 100

​    而前两种情况就在for循环中就可以解决了

​    第三种 创建一个新数组，长度加1  默认为0  第一个元素赋值0 

time:O(n)

```java
 /*
    一共三种情况 
    1.不存在进位  123  -》 124 
    2.数字长度不变  199  -> 200  
    3.数字长度加1   99  -》 100
    而前两种情况就在for循环中就可以解决了
    第三种 创建一个新数组，长度加1  默认为0  第一个元素赋值0  
    */
    public int[] plusOne(int[] digits) {
        for(int i = digits.length-1;i>=0;i--){
            digits[i]++;
            digits[i] = digits[i] %10;
            if(digits[i]!=0){
                return digits;
            }
        }
        int [] newArr = new int [digits.length+1];
        newArr[0] = 1;
        return newArr;
    }
```

第二种答案 思想和上面的一致

```java
public int[] plusOne(int[] digits) {
        int length = digits.length;
        for (int i = length-1; i >= 0 ; i--) {
            if ((digits[i]++)<9){
                return digits;
            }
            digits[i] = 0;
        }
        digits = new int [length+1];
        digits[0] = 1;
        return digits;
    }
```

## 448.找到所有数组中的消失的数字

> #### [448. 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)
>
> 难度简单382
>
> 给定一个范围在 1 ≤ a[i] ≤ *n* ( *n* = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。
>
> 找到所有在 [1, *n*] 范围之间没有出现在数组中的数字。
>
> 您能在不使用额外空间且时间复杂度为*O(n)*的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。
>
> **示例:**
>
> ```
> 输入:
> [4,3,2,7,8,2,3,1]
> 
> 输出:
> [5,6]
> ```

时间复杂度O(n)

空间复杂度O(1)

```java
	//将nums中的数放到对应的下标中去。
    public List<Integer> findDisappearedNumbers(int[] nums) {
        if(nums == null || nums.length == 0){
            return new ArrayList<Integer>();
        }
         
        int [] arr = new int [nums.length+1];
        for(int i=0;i<nums.length;i++){
            arr[nums[i]] = nums[i];
        }

        List<Integer> res = new ArrayList<>();
        for(int i=1;i<arr.length;i++){
            if(arr[i] == 0 ){
                res.add(i);
            }
        }
        return res;
    }
```

鸽巢定理

```java
	public List<Integer> findDisappearedNumbers(int[] nums) {
        if(nums == null || nums.length == 0){
            return new ArrayList<>();
        }

        int l = nums.length;
        for(int i=0;i<l;i++){
            while( nums[i] != nums[nums[i]-1] ){
                swap(nums,i,nums[i]-1);
            }
        }
        List<Integer> res = new ArrayList<>();
        for(int i=0;i<l;i++){
            if(nums[i]!=i+1){
                res.add(i+1);
            }
        }
        return res;
    }


    public void swap(int [] nums,int a,int b){
        if(a!=b){
            nums[a] = nums[a] ^ nums[b];
            nums[b] = nums[a] ^ nums[b];
            nums[a] = nums[a] ^ nums[b];
        }
    }
```

## 581.最短无序连续子数组

> #### [581. 最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)
>
> 难度简单335收藏分享切换为英文关注反馈
>
> 给定一个整数数组，你需要寻找一个**连续的子数组**，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。
>
> 你找到的子数组应是**最短**的，请输出它的长度。
>
> **示例 1:**
>
> ```
> 输入: [2, 6, 4, 8, 10, 9, 15]
> 输出: 5
> 解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
> ```

### 1.双指针

```java
/***
    *  核心思想还是双指针，i 和 j 
      a.默认第一个元素为最大值，如果当前元素小于最大值，说明出现逆序情况 也就是3 2 1 这样，right = i
        如果没有 则将当前最大值设定为nums[i]
      b.默认最后一个元素为最小值，如果当前元素大于最小值，说明也出现逆序情况，也就是 1 3 2 
        如果没有 则将最小值赋值为nums[length-i-1] 
    好了 来分析一下时间复杂度 因为只需要一次遍历 所以为O(n)
    */
    public int findUnsortedSubarray(int[] nums) {
        int length = nums.length;
        int left = 0,  right = -1;
        int max = nums[0], min = nums[length-1];
        for(int i=0;i<length;i++){
            if(nums[i]<max) right = i;
            else max = nums[i];
            if(nums[length-i-1]>min) left = length-i-1;
            else min = nums[length-i-1];
        }
        return right-left+1;
    }
```

## [238. 除自身以外数组的乘积](https://leetcode-cn.com/problems/product-of-array-except-self/)  ☆

> #### [238. 除自身以外数组的乘积](https://leetcode-cn.com/problems/product-of-array-except-self/)
>
> 难度中等532收藏分享切换为英文关注反馈
>
> 给你一个长度为 *n* 的整数数组 `nums`，其中 *n* > 1，返回输出数组 `output` ，其中 `output[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积。
>
>  
>
> **示例:**
>
> ```
> 输入: [1,2,3,4]
> 输出: [24,12,8,6]
> ```
>
>  
>
> **提示：**题目数据保证数组之中任意元素的全部前缀元素和后缀（甚至是整个数组）的乘积都在 32 位整数范围内。
>
> **说明:** 请**不要使用除法，**且在 O(*n*) 时间复杂度内完成此题。

```java
	// i=0 res[0]=1 k=1
    // i=1 res[1]=1 k=2
    // i=2 res[2]=2 k=3
    // i=3 res[3]=6 k=24

    // i=3 res[3]=6 k=6;
    // i=2 res[2]=8 k=12
    // i=1 res[1]=12 k=24
    // i=0 res[0]=24 k=24

    // [1,2,3,4] = [24,12,8,6]
    // 
    // 时间复杂度O(n) 空间复杂度O(n)
    public int[] productExceptSelf(int[] nums) {
        int [] res = new int [nums.length];
        int k = 1;
        // 计算当前数左边的乘积
        for(int i=0;i<res.length;i++){
            res[i] = k;       // 1  1  2  6
            k = k * nums[i];  // 1  2  6  24
        }
        // 计算当前数右边的乘积
        k = 1;
        for(int i=res.length-1;i>=0;i--){
            res[i]*=k; //k为概述右边的乘积   //  res[3] = 6   res[2] = 24 
            k *= nums[i]; // k = 24  k = 
        }
        return res;
    }
```

## [7. 整数反转](https://leetcode-cn.com/problems/reverse-integer/)  ☆

> #### [7. 整数反转](https://leetcode-cn.com/problems/reverse-integer/)
>
> 难度简单2057
>
> 给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
>
> **示例 1:**
>
> ```
> 输入: 123
> 输出: 321
> ```
>
>  **示例 2:**
>
> ```
> 输入: -123
> 输出: -321
> ```
>
> **示例 3:**
>
> ```
> 输入: 120
> 输出: 21
> ```



```java
	// 本题只需要将数字逆转 很容易做到，但是数字越界问题需要我们考虑好
    public int reverse(int x) {
        int ans = 0;
        while(x!=0){
            int pop = x%10;

            if((ans>Integer.MAX_VALUE/10) || (ans==Integer.MAX_VALUE/10 &&  pop > 7)){
                return 0;
            } 
            if((ans<Integer.MIN_VALUE/10) || (ans==Integer.MIN_VALUE/10 && pop < -8)){
                return 0;
            }
            ans = ans*10+pop;
            x/=10;
        }
        return ans;
    }
```

## [48. 旋转图像](https://leetcode-cn.com/problems/rotate-image/)

> #### [48. 旋转图像](https://leetcode-cn.com/problems/rotate-image/)
>
> 难度中等504
>
> 给定一个 *n* × *n* 的二维矩阵表示一个图像。
>
> 将图像顺时针旋转 90 度。
>
> **说明：**
>
> 你必须在**[原地](https://baike.baidu.com/item/原地算法)**旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要**使用另一个矩阵来旋转图像。
>
> **示例 1:**
>
> ```
> 给定 matrix = 
> [
>   [1,2,3],
>   [4,5,6],
>   [7,8,9]
> ],
> ```



```java
	//时间复杂度为O(N^2) 这道题主要是思路和扣边界
    public void rotate(int[][] matrix) {
        if(matrix == null || matrix.length == 0 || matrix.length != matrix[0].length){
            return;
        }
        int len = matrix.length;
        //先将右上角到左下角为一条线 对换数据
        for(int i=0;i<len;i++){
            for(int j=0;j<len-i;j++){
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[len-1-j][len-1-i];
                matrix[len-1-j][len-1-i] = tmp;
            }
        }

        //按照中间进行划分。 len>>1 是为了只交换上下两部分。
        for(int i=0;i<(len >> 1);i++){
            for(int j=0;j<len;j++){
                int tmp = matrix[i][j];
                //这块len-1-i 保持行  j 是保持同一列数据
                matrix[i][j] = matrix[len-1-i][j];
                matrix[len-1-i][j] = tmp;
            }
        }
    }
```

## [287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

> #### [287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)
>
> 难度中等790
>
> 给定一个包含 *n* + 1 个整数的数组 *nums*，其数字都在 1 到 *n* 之间（包括 1 和 *n*），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。
>
> **示例 1:**
>
> ```
> 输入: [1,3,4,2,2]
> 输出: 2
> ```
>
> **示例 2:**
>
> ```
> 输入: [3,1,3,4,2]
> 输出: 3
> ```

### 快慢指针

```java
	// 快慢指针 如果数组中有重复的数字，那么必定会形成环
    // 时间复杂度为O(N)
    public int findDuplicate(int[] nums) {
        int slow = 0;
        int fast = 0;

        slow = nums[slow];
        fast = nums[nums[fast]];

        while(fast!=slow){
            slow = nums[slow];
            fast = nums[nums[fast]];
        }

        int pre1 = 0;
        int pre2 = slow;
        while(pre1!=pre2){
            pre1 = nums[pre1];
            pre2 = nums[pre2];
        }
        return pre1;
    }
```

### 二分

```java
	// 二分查找
    // 时间复杂度O(NlogN) 二分logN 遍历数组O(n)
    public int findDuplicate(int[] nums) {
        int len = nums.length;
        int l = 1;
        int r = len-1;
        //查找小于mid的值
        while(l<r){
            int mid = (l+r) >>> 1;
            int cnt = 0;
            for(int i=0;i<len;i++){
                if(nums[i] <= mid){
                    cnt++;
                }
            }
            //大于mid 则在左边
            if(cnt > mid){
                r = mid;
            }else {
                // 小于mid 则在右边
                l = mid+1;
            }
        }
        return l;
    }
```

## [204. 计数质数](https://leetcode-cn.com/problems/count-primes/)

> #### [204. 计数质数](https://leetcode-cn.com/problems/count-primes/)
>
> 难度简单400
>
> 统计所有小于非负整数 *n* 的质数的数量。
>
> **示例:**
>
> ```
> 输入: 10
> 输出: 4
> 解释: 小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
> ```

```java
	// a.去掉偶数 
    // b.去掉质数*质数
    // 时间复杂度为O(n/2)*(n/2) O(n^2)
    public int countPrimes(int n) {
        int res = 0;
        boolean [] b = new boolean[n];
        if(n>2) res++; //计算上2 

        for(int i=3;i<n;i+=2){//去掉偶数 
            if(!b[i]){//是质数
                for(int j=3;i*j<n;j+=2){ //质数乘以质数
                    b[i*j] = true;
                }
                res++;
            }
        }
        return res;
    }
```



## [566. 重塑矩阵](https://leetcode-cn.com/problems/reshape-the-matrix/)

> 难度简单148
>
> 在MATLAB中，有一个非常有用的函数 `reshape`，它可以将一个矩阵重塑为另一个大小不同的新矩阵，但保留其原始数据。
>
> 给出一个由二维数组表示的矩阵，以及两个正整数`r`和`c`，分别表示想要的重构的矩阵的行数和列数。
>
> 重构后的矩阵需要将原始矩阵的所有元素以相同的**行遍历顺序**填充。
>
> 如果具有给定参数的`reshape`操作是可行且合理的，则输出新的重塑矩阵；否则，输出原始矩阵。
>
> **示例 1:**
>
> ```
> 输入: 
> nums = 
> [[1,2],
>  [3,4]]
> r = 1, c = 4
> 输出: 
> [[1,2,3,4]]
> ```

```java
	//r 行号
    //c 列号
    public int[][] matrixReshape(int[][] nums, int r, int c) {
        int m = nums.length, n = nums[0].length;
        if (m * n != r * c) 
            return nums;
        int [][] arr = new int [r][c];
        int index = 0;
        for (int i = 0; i < r ; i++) {
            for (int j = 0; j < c ; j++) {
                arr[i][j] = nums[index/n][index%n];
                index++;
            }
        }
        return arr;
    }
```





## [645. 错误的集合](https://leetcode-cn.com/problems/set-mismatch/)

> 难度简单132
>
> 集合 `S` 包含从1到 `n` 的整数。不幸的是，因为数据错误，导致集合里面某一个元素复制了成了集合里面的另外一个元素的值，导致集合丢失了一个整数并且有一个元素重复。
>
> 给定一个数组 `nums` 代表了集合 `S` 发生错误后的结果。你的任务是首先寻找到重复出现的整数，再找到丢失的整数，将它们以数组的形式返回。
>
> **示例 1:**
>
> ```
> 输入: nums = [1,2,2,4]
> 输出: [2,3]
> ```

```java
	//通过交换元素 是元素在应有的位置上
	//特例 [3,2,2]
    public int[] findErrorNums(int[] nums) {
        if (nums == null) return nums;

        for (int i = 0; i < nums.length; i++) {
            
            // 3 != 1 && 2 != 3
            // [2,2,3]
            // 
            while (nums[i] != i+1 && nums[nums[i] - 1] != nums[i]) {
                // System.out.println(i);
                //0 2 
                swap(nums,i,nums[i]-1);
                System.out.println(Arrays.toString(nums));
            }
        }


        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i+1) {
                return new int []{nums[i],i+1};
            }
        }
        return null;
    }

    //
    private void swap(int [] nums , int x, int y) {
        int tmp = nums[x];
        nums[x] = nums[y];
        nums[y] = tmp;
    }
```



## [667. 优美的排列 II](https://leetcode-cn.com/problems/beautiful-arrangement-ii/)

> 难度中等71
>
> 给定两个整数 `n` 和 `k`，你需要实现一个数组，这个数组包含从 `1` 到 `n` 的 `n` 个不同整数，同时满足以下条件：
>
> ① 如果这个数组是 [a1, a2, a3, ... , an] ，那么数组 [|a1 - a2|, |a2 - a3|, |a3 - a4|, ... , |an-1 - an|] 中应该有且仅有 k 个不同整数；.
>
> ② 如果存在多种答案，你只需实现并返回其中任意一种.
>
> **示例 1:**
>
> ```
> 输入: n = 3, k = 1
> 输出: [1, 2, 3]
> 解释: [1, 2, 3] 包含 3 个范围在 1-3 的不同整数， 并且 [1, 1] 中有且仅有 1 个不同整数 : 1
> ```

```java
 	//n表示数据的大小
    //k 表示在不同间隔中，有多少个不同的数字 
    //比如 1 2 3 2个间隔数 1个不同数
    //1 3 2 2个不同间隔数  2 ， 1 2个不同的数字
    public int[] constructArray(int n, int k) {
        int [] nums = new int [n];
        nums[0] = 1;
        //interval 间距
        for ( int i = 1,interval = k; i <=k; i++,interval--) {
            //nums[1,]  i=1  nums[0]+1   interval = 0
            nums[i] = i % 2 == 1 ? nums[i-1]+interval : nums[i-1] - interval;
        }
        //nums[1,2] i=2  nums[2] = 3
        for (int i = k+1 ;i < n;i++) {
            nums[i] = i +1;
        }
        return nums;
    }
```



## [766. 托普利茨矩阵](https://leetcode-cn.com/problems/toeplitz-matrix/)

> 难度简单150
>
> 如果矩阵上每一条由左上到右下的对角线上的元素都相同，那么这个矩阵是 **托普利茨矩阵** 。
>
> 给定一个 `M x N` 的矩阵，当且仅当它是*托普利茨矩阵*时返回 `True`。
>
> **示例 1:**
>
> ```
> 输入: 
> matrix = [
>   [1,2,3,4],
>   [5,1,2,3],
>   [9,5,1,2]
> ]
> ```

```java
	//主要利用二维数组的特点进行查找比较。 
    //以第一个元素为基础，如果后续元素不等于第一个元素，那么就直接返回。
    //仔细分析一下 还是有一定技巧的，每次都在原有基础的行和列上加1
    //时间复杂度为O(n)
    public boolean isToeplitzMatrix(int[][] matrix) {
        if (matrix == null) return false;

        //行进行 查找
        for (int i = 0; i < matrix[0].length; i++) {
            if (!isEquals(matrix,matrix[0][i],0,i)) {
                return false;
            }
        }
        //列查找 
        for (int i = 0; i < matrix.length; i++) {
            if (!isEquals(matrix,matrix[i][0],i,0)) {
                return false;
            }
        }
        return true;
    }

    private boolean isEquals (int [][] matrix,int exceptValue,int r, int c) {
        if (r >= matrix.length || c >= matrix[0].length) {
            return true;
        }

        if (matrix[r][c] != exceptValue) {
            return false;
        }   
        return isEquals(matrix,exceptValue,r+1,c+1);
    }
```



## [565. 数组嵌套](https://leetcode-cn.com/problems/array-nesting/)

> 难度中等120
>
> 索引从`0`开始长度为`N`的数组`A`，包含`0`到`N - 1`的所有整数。找到最大的集合`S`并返回其大小，其中 `S[i] = {A[i], A[A[i]], A[A[A[i]]], ... }`且遵守以下的规则。
>
> 假设选择索引为`i`的元素`A[i]`为`S`的第一个元素，`S`的下一个元素应该是`A[A[i]]`，之后是`A[A[A[i]]]...` 以此类推，不断添加直到`S`出现重复的元素。
>
>  
>
> **示例 1:**
>
> ```
> 输入: A = [5,4,0,3,1,6,2]
> 输出: 4
> 解释: 
> A[0] = 5, A[1] = 4, A[2] = 0, A[3] = 3, A[4] = 1, A[5] = 6, A[6] = 2.
> ```

```java
	public int arrayNesting(int[] nums) {
        if (nums == null) return 0;
        int max = 0;
        //从0开始 查找最大的值 
        for (int i = 0; i < nums.length; i++) {
            int cnt = 0;
            for (int j = i; nums[j] != -1; ) {
                cnt++;
                int t = nums[j];
                nums[j] = -1;
                j = t;
            }
            max = Math.max(cnt,max);
        }
        return max;
    }
```



## [769. 最多能完成排序的块](https://leetcode-cn.com/problems/max-chunks-to-make-sorted/)

> 难度中等99
>
> 数组`arr`是`[0, 1, ..., arr.length - 1]`的一种排列，我们将这个数组分割成几个“块”，并将这些块分别进行排序。之后再连接起来，使得连接的结果和按升序排序后的原数组相同。
>
> 我们最多能将数组分成多少块？
>
> **示例 1:**
>
> ```
> 输入: arr = [4,3,2,1,0]
> 输出: 1
> 解释:
> 将数组分成2块或者更多块，都无法得到所需的结果。
> 例如，分成 [4, 3], [2, 1, 0] 的结果是 [3, 4, 0, 1, 2]，这不是有序的数组。
> ```

```java
	public int maxChunksToSorted(int[] arr) {
        if (arr == null) return 0;
        int ret = 0;
        int right = arr[0];
        for (int i =0 ; i < arr.length; i++) {
            right = Math.max(right,arr[i]);
            if (right == i) 
              ret++;
        }
        return ret;
    }
```

