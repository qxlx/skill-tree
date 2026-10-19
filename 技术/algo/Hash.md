# Hash

## 242.有效的字母异位词  ☆

> #### [242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)
>
> 难度简单173收藏分享切换为英文关注反馈
>
> 给定两个字符串 *s* 和 *t* ，编写一个函数来判断 *t* 是否是 *s* 的字母异位词。
>
> **示例 1:**
>
> ```
> 输入: s = "anagram", t = "nagaram"
> 输出: true
>
> ```
>
> **示例 2:**
>
> ```
> 输入: s = "rat", t = "car"
> 输出: false
> ```

### 1.依次遍历比较

思路:异位次的含义是 一个字符串中字母的顺序改变，但是个数不变 就是异位词。

比较长度 如果长度不相等 返回false 然后将两个字符串进行排序，时间复杂度为O(logN) 

然后进行依次比较 如果不相等直接返回true  不好的一点是 排序使时间复杂度提高了。

time :O(NlogN)

space:O(1)

```java
//time O(NlogN)  space O(n)
    public boolean isAnagram(String s, String t) {
        //排序后比较
        if(s.length() != t.length()){
            return false;
        }

        char [] chs = s.toCharArray();
        char [] cht = t.toCharArray();

        Arrays.sort(chs);
        Arrays.sort(cht);

        return Arrays.equals(chs,cht);
    }
```

### 2.哈希表

思路：如果长度不同 返回false 定义一个数组 26  s.charAt(i)-'a' 存储进数组  对应的下标  t.charAt(i)-'a'  不存储对应的下标。前者++  后者-- 当最后数组为0是异位词，否则不是异位词。

time:O(n)

space:O(1)

```java
public boolean isAnagram(String s, String t) {
        //哈希表
        if(s.length() != t.length()){
            return false;
        }

        int [] alpha = new int [26];
        for(int i=0;i<s.length();i++){
            alpha[s.charAt(i) - 'a']++;
            alpha[t.charAt(i) - 'a']--;
        }

        for(int i=0;i<alpha.length;i++){
            if(alpha[i]!=0){
                return false;
            }
        }
        return true;
    }
```

## 49.字母异位词分组  ☆

> #### [49. 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)
>
> 难度中等309
>
> 给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。
>
> **示例:**
>
> ```
> 输入: ["eat", "tea", "tan", "ate", "nat", "bat"],
> 输出:
> [
>   ["ate","eat","tea"],
>   ["nat","tan"],
>   ["bat"]
> ]
> ```



### 1.排序数组分类

时间复杂度:O(NK logK)

空间复杂度:O(NK)

```java
public List<List<String>> groupAnagrams(String[] strs) {
        if(strs.length == 0 || strs == null){
            return null;
        }
        //1.按照排序
        Map<String,List> ans = new HashMap<String,List>();
        for(String str : strs){
            char [] chs = str.toCharArray();
            Arrays.sort(chs);
            String key = String.valueOf(chs);//转换成字符串 当做key计算
            //如果不包含 说明之前没有添加 以排序好的string做key 计算
            if(!ans.containsKey(key))
                ans.put(key,new ArrayList());
            //不管之前添加过没有 都添加到ans Map中 
            ans.get(key).add(str);
        }

        return new ArrayList(ans.values());
    }
```

##  [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

> #### [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)
>
> 难度困难464
>
> 给定一个未排序的整数数组，找出最长连续序列的长度。
>
> 要求算法的时间复杂度为 *O(n)*。
>
> **示例:**
>
> ```
> 输入: [100, 4, 200, 1, 3, 2]
> 输出: 4
> 解释: 最长连续序列是 [1, 2, 3, 4]。它的长度为 4。
> ```

```java
	//hashset
    //1.将元素添加到set中。然后在遍历nums.如果没有直接跳过。(去重)
    //2.拿到当前元素n 分别去看n-1 和n+1 是否在set中存储过，如果有迭代删除，计算出长度len
    //3.max记录当前最大值。
    // 时间复杂度为O(n) 空间复杂度为O(n)
    public int longestConsecutive(int[] nums) {
        if(nums == null || nums.length == 0){
            return 0;
        }

        HashSet<Integer> set = new HashSet<>();
        int max = 0;
        for(int n : nums){
            set.add(n);
        }

        for(int n : nums){
            if(!set.contains(n)){
                continue;
            }
            int a = n-1,b = n+1;
            int len = 1;
            while(set.contains(a)){
                len++;
                set.remove(a--);
            }

            while(set.contains(b)){
                len++;
                set.remove(b++);
            }
            max = Math.max(max,len);
        }
        return max;
    }
```

## [454. 四数相加 II](https://leetcode-cn.com/problems/4sum-ii/)

> #### [454. 四数相加 II](https://leetcode-cn.com/problems/4sum-ii/)
>
> 难度中等174
>
> 给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 `(i, j, k, l)` ，使得 `A[i] + B[j] + C[k] + D[l] = 0`。
>
> 为了使问题简单化，所有的 A, B, C, D 具有相同的长度 N，且 0 ≤ N ≤ 500 。所有整数的范围在 -228 到 228 - 1 之间，最终结果不会超过 231 - 1 。
>
> **例如:**
>
> ```
> 输入:
> A = [ 1, 2]
> B = [-2,-1]
> C = [-1, 2]
> D = [ 0, 2]
> 
> 输出:
> 2
> ```

```java
    // hashMap存储AB的组合的值，如果出现相同值value+1，说明当前某一个值，可以有
    // 多种匹配的方案。CD通过计算相反的值，通过查找Map中是否有就可以判断是否可以计算
    // 出多种匹配0的方法次数。
    // 时间复杂度为O(N^2) 
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        Map<Integer,Integer> map = new HashMap<>();
        int res = 0;
        for(int i=0;i<A.length;i++){
            for(int j=0;j<B.length;j++){
                int ab = A[i]+B[j];
                if(map.containsKey(ab)){
                    map.put(ab,map.get(ab)+1);
                }else{
                    map.put(ab,1);
                }
            }
        }

        for(int i=0;i<C.length;i++){
            for(int j=0;j<D.length;j++){
                int cd = -(C[i]+D[j]);
                if(map.containsKey(cd)){
                    res+=map.get(cd);
                }
            }
        }
        return res;
    }
```

## [939. 最小面积矩形](https://leetcode-cn.com/problems/minimum-area-rectangle/)

> #### [939. 最小面积矩形](https://leetcode-cn.com/problems/minimum-area-rectangle/)
>
> 难度中等43
>
> 给定在 xy 平面上的一组点，确定由这些点组成的矩形的最小面积，其中矩形的边平行于 x 轴和 y 轴。
>
> 如果没有任何矩形，就返回 0。
>
>  
>
> **示例 1：**
>
> ```
> 输入：[[1,1],[1,3],[3,1],[3,3],[2,2]]
> 输出：4
> ```
>
> **示例 2：**
>
> ```
> 输入：[[1,1],[1,3],[3,1],[3,3],[4,1],[4,3]]
> 输出：2
> ```

```java
	public int minAreaRect(int[][] points) {
        Set<Integer> pointSet = new HashSet<Integer>();
        //将每个下标相加添加到set中，如果有重复的会去重。
        for(int [] point : points){
            pointSet.add(40001* point[0]+point[1]);
        }

        int ans = Integer.MAX_VALUE;//求最小值 应该设置一个最大值
        for(int i=0;i<points.length;i++){
            for(int j=i+1;j<points.length;j++){
                //两个坐标不在同一行 同一列，那么势必就是对角线关系。
                if(points[i][0]!=points[j][0] && points[i][1]!=points[j][1]){
                    if(pointSet.contains(40001 * points[i][0]+ points[j][1]) && pointSet.contains(40001 * points[j][0]+points[i][1])){
                        ans = Math.min(Math.abs(points[j][0]-points[i][0])*Math.abs(points[j][1]-points[i][1]),ans);
                    }
                }
            }
        }
        return ans < Integer.MAX_VALUE ? ans : 0;
    }
```



## [594. 最长和谐子序列](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)

> 难度简单132
>
> 和谐数组是指一个数组里元素的最大值和最小值之间的差别正好是1。
>
> 现在，给定一个整数数组，你需要在所有可能的子序列中找到最长的和谐子序列的长度。
>
> **示例 1:**
>
> ```
> 输入: [1,3,2,2,5,2,3,7]
> 输出: 5
> 原因: 最长的和谐数组是：[3,2,2,2,3].
> ```

```java
	//利用hashmap记录当前值
    public int findLHS(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        int size = 0;
        
        Map<Integer,Integer> hashMap = new HashMap<>();
        //a.记录当前每个值出现的次数
        for (int num : nums) {
            hashMap.put(num,hashMap.getOrDefault(num,0)+1);
        }
		//b.找到对应的最大值
        for (int num : hashMap.keySet()) {
            if (hashMap.containsKey(num+1)) {
                size = Math.max(hashMap.get(num) + hashMap.get(num+1),size);
            }
        }
        return size;
    }
```

