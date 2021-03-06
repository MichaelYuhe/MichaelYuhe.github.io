# 持续更新的LeetCode笔记


# LeetCode刷题记录

## 动态规划

### [121.买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

![image-20210905170242931](/images/LeetCode/image-20210905170242931.png)

#### 思路：

- 动态规划题
- minPrice保存前i天的最低价格，用第i天的现价去减这个最小值，就得到了在第i天卖出的利润
- 取最大利润

```js
var maxProfit = function(prices) {
    let profit = 0, res = 0,  minPrice = prices[0];
    let len = prices.length;
    for(let i = 1; i < len; i++){
        if((prices[i] - minPrice) > profit){
            profit = prices[i] - minPrice;
            res = i + 1;
        }
        minPrice = Math.min(minPrice, prices[i]); //前i-1天最低价格
    }
    return res;
};
```

时间复杂度：O(n)，遍历一次

空间复杂度：O(1)

执行用时：84ms，击败了97.91%的用户

内存消耗：47.5MB，击败了71.92%的用户

#### 官方题解：

无js版本，但思路是一致的，不同的点在于官方是从0开始遍历，然后min最开始取一个无穷大值以保证其大于第一天的价格。

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        inf = int(1e9)
        minprice = inf
        maxprofit = 0
        for price in prices:
            maxprofit = max(price - minprice, maxprofit)
            minprice = min(price, minprice)
        return maxprofit
```

### [714. 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

![image-20210915104706423](../../static/images/LeetCode/image-20210915104706423.png)

#### 思路：

- 动态规划，每天有两种状态，一是今天交易结束后不持有股票，二是持有
- sell表示不持有，buy表示持有
- 不持有的情况：前一天就不持有，或者前一天买入今天卖出
- 持有的情况：前一天就持有，前一天不持有但今天买入
- 因此可以得到状态转移方程 sell = max(sell, buy + prices[i] - fee), buy = max(buy, sell - prices[i])

```js
var maxProfit = function(prices, fee) {
    let sell = 0, buy = -prices[0]
    for(let i = 1; i < prices.length; i++) {
        sell = Math.max(sell, buy + prices[i] - fee)
        buy = Math.max(sell - prices[i], buy)
    }
    return sell // 要想利润最大，结束后肯定不能持有股票
};
```

### [1014.最佳观光组合](https://leetcode-cn.com/problems/best-sightseeing-pair/)

![image-20210915223136730](../../static/images/LeetCode/image-20210915223136730.png)

#### 思路：

- maxScore由三个值组成：距离以及两个景点的好看程度
- 很明显，两个景点都好看，又近的时候最完美
- 因此可以先用一个值记录第一个好看的景点，当第二个离他越来越远，这个景点的好看程度也一直下降

```js
var maxScoreSightseeingPair = function(values) {
    let maxScore = 0, firstSight = 0, secondSight = 1
    for(let i = 0; i < values.length - 1; i++) {
        // 如果第一个景点不够好看，无法支持这个距离，更新
        if(values[firstSight] <= values[i]) {
            // 更新第一个景点（第一个因为太远被抛弃了）
            firstSight = i
            secondSight = firstSight + 1
        }
        // 每次循环，第一个景点的吸引力就下降一点
        values[firstSight]--
        // maxScore维护最大值
        maxScore = Math.max(maxScore, (values[firstSight] + values[secondSight]))
        // 一直在往后找第二个景点
        secondSight++
    }
    return maxScore
```

### [413.等差数列划分](https://leetcode-cn.com/problems/arithmetic-slices/)

![image-20210917144621838](../../static/images/LeetCode/image-20210917144621838.png)

#### 思路：

- 每个等差数列内的子数列，都是等差数列
- 不是别的等差数列的子数列的等差数列:joy:,之间不会有交集

```js
var numberOfArithmeticSlices = function(nums) {
    if(nums.length < 3) return 0
    let res = 0, len = 2
    for(let i = 1; i < nums.length - 1; i++) {
        if((nums[i] - nums[i-1]) === (nums[i+1] - nums[i])) {
            len++
        }
        else {
            if(len >= 3) {
                res = res + (len-1) * (len-2) / 2
            }
            len = 2
        }
        if(i === nums.length - 2) {
            if(len >= 3) {
                res = res + (len-1) * (len-2) /2
            }
        }
    }
    return res
}; 
```

时间复杂度：O(N)，遍历一次

空间复杂度：O(1)，不需要额外空间

执行用时：64ms，击败了88.75%的用户

内存消耗：37.6MB，击败了58.73%的用户

#### 官方题解

```js
var numberOfArithmeticSlices = function(nums) {
    const n = nums.length;
    if (n === 1) {
        return 0;
    }

    let d = nums[0] - nums[1], t = 0;
    let ans = 0;
    // 因为等差数列的长度至少为 3，所以可以从 i=2 开始枚举
    for (let i = 2; i < n; ++i) {
        if (nums[i - 1] - nums[i] === d) {
            ++t;
        } else {
            d = nums[i - 1] - nums[i];
            t = 0;
        }
        ans += t;
    }
    return ans;
};
```



### [53.最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

![image-20210905170213381](/images/LeetCode/image-20210905170213381.png)

#### 思路：

- 若只有一个元素，直接返回
- 从前往后遍历并求和，若sum<0，则sum归零（前面的元素都不要了）
- 到后面遇到了负数，但是这个负数加进来也不足以使sum<0，则从这个负数开始往后看，它和后面的元素依此相加，得到一个newSum，一大于0就把newSum加上。

#### 官方题解：

1. 动态规划

```js
var maxSubArray = function(nums) {
    let pre = 0, maxAns = nums[0];
    nums.forEach((x) => {
        pre = Math.max(pre + x, x);
        maxAns = Math.max(maxAns, pre);
    });
    return maxAns;
};
```

foreach：对数组的每个元素执行一次给定的函数。

### [918. 环形子数组的最大和](https://leetcode-cn.com/problems/maximum-sum-circular-subarray/)

![image-20210913101543776](../../static/images/LeetCode/image-20210913101543776.png)

#### 思路：

- 处理思想和最大子序和基本一致，唯一区别要考虑到环
- 可以分为两种情况考虑，第一种是不需要越过边界，则和最大子序和一样
- 第二种是越过了边界，这是只需要求出数组的总和，再减去**最小子序和**，就是越过环的结果
- 最后在处理结果时还需要考虑全为负数的情况

```js
var maxSubarraySumCircular = function(nums) {
    let max = nums[0], min = nums[0], preMax = nums[0], preMax = nums[0], sum = nums[0]
    for(let i = 1; i < nums.length; i++) {
        preMax = Math.max(nums[i], (preMax + nums[i]))
        preMin = Math.min(nums[i], (preMin + nums[i]))
        max = Math.max(preMax, max)
        min = Math.min(preMin, min)
        sum += nums[i]
    }
    return (max < 0) ? max : Math.max(max, sum - min)
}
```

时间复杂度：O(N)，遍历一次

空间复杂度：O(1)，不需要额外空间

执行用时：84ms，击败了80.75%的用户

内存消耗：43.6MB，击败了78.73%的用户

#### 官方题解

### [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

![image-20210905170343945](/images/LeetCode/image-20210905170343945.png)

#### 思路：

- 爬到某一级台阶的前一步，必定落在上一级或者上上级。
- 所以爬到某一台阶的方法数，等于爬到上一级和上上级的方法数之和。

```js
var climbStairs = function(n) {
	if(n <= 2){
		return n;
	}
    let i1 = 1, i2 = 2, temp = 0; 
    for(let i = 3; i <= n; i++){
        temp = i1 + i2;
        i1 = i2;
        i2 = temp;
    }
    return temp;
}
```

时间复杂度：O(N)，遍历一次

空间复杂度：O(1)，不需要额外空间

执行用时：64ms，击败了94.33%的用户

内存消耗：37.7MB，击败了30.68%的用户

#### 官方题解

```js
/* 方法一，动态规划 */
var climbStairs = function(n) {
    let p = 0, q = 0, r = 1;
    for (let i = 1; i <= n; ++i) {
        p = q;
        q = r;
        r = p + q;
    }
    return r;
};
/* 方法二，通项公式 */
var climbStairs = function(n) {
    const sqrt5 = Math.sqrt(5);
    const fibn = Math.pow((1 + sqrt5) / 2, n + 1) - Math.pow((1 - sqrt5) / 2, n + 1);
    return Math.round(fibn / sqrt5);
};
```

### [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

![image-20210905170422059](/images/LeetCode/image-20210905170422059.png)

#### 思路：

- 小偷身处第 i 间房子时，知道自己在 “ 只偷到(k - 2)间时最多能偷到的钱 i1 ” 和 “ 只偷到(k - 1)间时最多能偷到的钱 i2 ”
- 第 i 间房子的钱加上 i1 比 i2 大的话，就偷，然后到了下一间房子后，i1 变成了之前的 i2 ， i2 变成了现在的sum
- 如果小，就不偷，直接到下一间房子，此时 i1 变为 i2， i2 则不变

```js
var rob = function(nums) {
    let len = nums.length;
    if(len === 1) return nums[0];
    if(len === 2) return Math.max(nums[0], nums[1]);
    let i1 = nums[0], i2 = Math.max(nums[0], nums[1]);
    let sum = 0;
    for(let i = 2; i < len; i++){
        if((nums[i] + i1) > i2){
            sum = nums[i] + i1;
            i1 = i2;
            i2 = sum;
        }
        else{
            sum = i2;
            i1 = i2;
        }
    }
    return sum;
};
```

执行用时：68ms，击败了84.07%的用户

内存消耗：37.7MB，击败了44.61%的用户

时间复杂度：O(n)

空间复杂度：O(1)

#### 官方题解

```java
class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int length = nums.length;
        if (length == 1) {
            return nums[0];
        }
        int first = nums[0], second = Math.max(nums[0], nums[1]);
        for (int i = 2; i < length; i++) {
            int temp = second;
            second = Math.max(first + nums[i], second);
            first = temp;
        }
        return second;
    }
}
```

## 二分法

### [35.搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

![image-20210905170547626](/images/LeetCode/image-20210905170547626.png)

#### 思路：

二分查找法的基础变式

- 如果查找到了，直接返回；
- 如果没找到，返回left(right+1)，也就是当前“光标”所在位置

```js
 var searchInsert = function(nums, target) {
    const len = nums.length;
    let left = 0, right = len - 1, mid;
    while(left <= right){
        mid = parseInt(left + (right-left) / 2);
        if(nums[mid] > target){
            right = mid - 1;
        }
        else if(nums[mid] < target){
            left = mid + 1;
        }
        else{
            return mid; //找到了就直接返回索引值
        }
    }
    return left; //若没找到，则返回目前“光标”位置，该位置前面的数都小于target，后面则都更大
};
```

执行用时：68ms，击败了94.68%的用户

内存消耗：39.1MB，击败了12.19%的用户

时间复杂度：O(logn)

空间复杂度：O(1)

#### 官方题解：

```js
var searchInsert = function(nums, target) {
    const n = nums.length;
    let left = 0, right = n - 1, ans = n;
    while (left <= right) {
        let mid = ((right - left) >> 1) + left;
        if (target <= nums[mid]) {
            ans = mid;
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return ans;
};
```

和自己思路类似，自己的觉得更简洁直白

### [34. 在排序数组中查找元素的第一个和最后一个位置]()

![image-20210905170712820](/images/LeetCode/image-20210905170712820.png)

#### 思路：

- 用二分法，找一个target
- 如果没找到，则直接返回-1，-1
- 找到了的话，从mid开始两头找start和end

```js
var searchRange = function(nums, target) {
    const len = nums.length;
    let left = 0, right = len - 1, start = -1, end = -1;
    let mid = parseInt(left + (right - left) / 2);
    while(left <= right){
        if(nums[mid] > target){
            right = mid - 1;
            mid = parseInt(left + (right - left) / 2);
        }
        else if(nums[mid] < target){
            left = mid + 1;
            mid = parseInt(left + (right - left) / 2);
        }
        else{
            break; /* 找到了 */
        }
    }
    /* 如果是因为left大于right而跳出的循环，则证明没有找到 */
    /* 如果left仍未大于right，则证明找到了，需要继续找 */
    if(left > right){
        return [start,end]; /* 直接返回[-1,-1] */
    } 
    let i = 0, j = 0;
    while(++i){
        if(nums[mid-i] != target || (mid-i) < left){
            start = mid - i + 1;
            break;
        }
    }
    while(++j){
        if(nums[mid+j] != target || (mid+j) > right){
            end = mid + j - 1;
            break;
        }
    }
    return [start,end];
};
```

时间复杂度：O(n)

空间复杂度：O(1)

执行用时：68ms，击败了94.12%的用户

内存消耗：39.9MB，击败了12.97%的用户

**陷入了误区，找到mid后使用了线性查找，导致时间复杂度来到了O(n)而不是O(logn)！！正确的做法是继续二分查找！！！**

**改进版**

```js
var searchRange = function(nums, target) {
    const len = nums.length;
    let left = 0, right = len - 1, start = -1, end = -1;
    let mid = parseInt(left + (right - left) / 2);
    while(left <= right){
        if(nums[mid] > target){
            right = mid - 1;
            mid = parseInt(left + (right - left) / 2);
        }
        else if(nums[mid] < target){
            left = mid + 1;
            mid = parseInt(left + (right - left) / 2);
        }
        else{
            break; /* 找到了 */
        }
    }
    /* 如果是因为left大于right而跳出的循环，则证明没有找到 */
    /* 如果left仍未大于right，则证明找到了，需要继续找 */
    if(left > right){
        return [start,end]; /* 直接返回[-1,-1] */
    } 
```

#### 官方题解

```js
const binarySearch = (nums, target, lower) => {
    let left = 0, right = nums.length - 1, ans = nums.length;
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (nums[mid] > target || (lower && nums[mid] >= target)) {
            right = mid - 1;
            ans = mid;
        } else {
            left = mid + 1;
        }
    }
    return ans;
}

var searchRange = function(nums, target) {
    let ans = [-1, -1];
    const leftIdx = binarySearch(nums, target, true);
    const rightIdx = binarySearch(nums, target, false) - 1;
    if (leftIdx <= rightIdx && rightIdx < nums.length && nums[leftIdx] === target && nums[rightIdx] === target) {
        ans = [leftIdx, rightIdx];
    } 
    return ans;
};
```

### [4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

![image-20210905170806814](/images/LeetCode/image-20210905170806814.png)

#### 思路：

- 归并排序，根据长度返回中位数

```js
var findMedianSortedArrays = function(nums1, nums2) {
    let len1 = nums1.length, len2 = nums2.length;
    let res = 1.0, i = 0, j = 0;
    let nums = [];
    while(i < len1 || j < len2) {
        if(i >= len1) {
            nums.push(nums2[j++]);
        }
        else if(j >= len2) {
            nums.push(nums1[i++]);
        }
        else {
            if(nums1[i] >= nums2[j]) {
                nums.push(nums2[j++]);
            }
            else {
                nums.push(nums1[i++]);
            }
        }
    }
    let len = nums.length;
    if(len % 2) {
        res = nums[Math.floor(len / 2)];
    }
    else {
        res = (nums[len / 2] + nums[len / 2 - 1]) / 2
    }
    return res;
};
```

时间复杂度：O(m+n)

空间复杂度：O(m+n)

执行用时：124ms，击败了65.19%的用户

内存消耗：43.2MB，击败了63.96%的用户

#### 官方题解：

题目要求：时间复杂度O(log(m+n))。

该题可以转化为，求两个有序数组中第k小的数，其中k为(m+n)/2或者(m+n)/2+1

要找到第k小的元素，我们可以比较nums1[k / 2 - 1]和nums2[k / 2 - 1]，所以对于nums1[k / 2 - 1]和nums2[k / 2 - 1]中的较小值，最多只会有(k-2)个元素比它小，那么它不可能是第k小的元素。

此处k / 2为整数除法( Math.floor() )

此时可以归纳三种情况：

1. nums1[k / 2 - 1] < nums2[k / 2 - 1]，则可以排除nums1[1] 到nums1[k / 2 - 1]
2. nums2[k / 2 - 1] < nums1[k / 2 - 1]，则可以排除nums2[1] 到nums2[k / 2 - 1]
3. nums1[k / 2 - 1] == nums2[k / 2 - 1]，可以并入第一种情况

所以在比较后，我们可以排除k / 2个不可能是第k小的数，意味着查找的范围已经缩小了一半

下一步就是在排除后的新数组上继续二分查找，并且根据我们排除的数的个数，来减小k的值

有三种特殊情况需要处理：

1. 越界。则取该数组最后一个数字，同时注意k不能直接减半
2. 若一个数组为空，直接返回另一个数组第k小的数字
3. 若k=1，直接返回两数组首元素较小值

优化后：

```

```



## 栈

### [剑指offer  06  从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

![image-20210905170903007](/images/LeetCode/image-20210905170903007.png)

#### 思路：

遍历链表，依此在头部添加val

```js
 var reversePrint = function(head) {
    let res = new Array();
    while(head!==null){
        res.unshift(head.val);
        head = head.next;
    }
     return res;
};
```

时间复杂度：O(n)

空间复杂度：O(n)

执行用时：80ms，击败了94.21%的用户

内存消耗：39.4MB，击败了94.03%的用户

#### 官方题解

**栈** 

​		栈的特点是后进先出，即最后压入栈的元素最先弹出。考虑到栈的这一特点，使用栈将链表元素顺序倒置。从链表的头节点开始，依次将每个节点压入栈内，然后依次弹出栈内的元素并存储到数组中。

## 树

### [671. 二叉树中第二小的节点]()(https://leetcode-cn.com/problems/second-minimum-node-in-a-binary-tree/)

![image-20210905171016582](/images/LeetCode/image-20210905171016582.png)

#### 思路：

- 在这个特殊的二叉树中，子节点的元素均大于等于父节点元素的值。
- 所以只要自顶往下，在某一层遇到了比根节点大，且是这一层最小的，那就是所要的节点。
- 如果一直没找到，直到所有节点遍历完了，也就是所有叶节点也和根节点的值一样，返回-1；

```

```

时间复杂度：O(n)

空间复杂度：O(1)

执行用时：76ms，击败了99.22%的用户

内存消耗：40.2MB，击败了47.62%的用户

#### 官方题解

```js
var findSecondMinimumValue = function(root){
    let ans = -1;
    const rootValue = root.val;

    const dfs = (node) => {
        if(node === null){
            return;
        }
        if(ans !== -1 && node.val >= ans){
            return;
        }
        if(node.val > rootValue){
            ans = node.val;
        }
        dfs(node.left);
        dfs(node.right);
    }

    dfs(root);
    return ans;
}
```

**const dfs = (node) => {} **

**=>  :  箭头函数，相当于匿名函数，且简化了函数定义**

## BFS和DFS

### [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

![image-20211012132241776](../../static/images/LeetCode/image-20211012132241776.png)

#### 思路

遍历grid，当遇到1，将其置0，将其上下左右的1置0，将其上下左右的1的上下左右的1置零……

```js
var numIslands = function(grid) {
    let res = 0
    const bfs = (i, j) => {
        if(i < 0 || j < 0 || i >= grid.length || j >= grid[0].length || grid[i][j] === '0') {
            return
        }
        grid[i][j] = '0'
        bfs(i + 1, j)
        bfs(i - 1, j)
        bfs(i, j + 1)
        bfs(i, j - 1)
    }
    for(let i = 0; i < grid.length; i++) {
        for(let j = 0; j < grid[0].length; j++) {
            if(grid[i][j] === '1') {
                res++
                bfs(i, j)
            }
        }
    }
    return res
};
```

时间复杂度：O(n)

空间复杂度：O(1)

执行用时：84ms，击败了52.76%的用户

内存消耗：40.4MB，击败了49.33%的用户

#### 官方题解

1.广度优先搜索BFS

```python
class Solution:
    def numIslands(self, grid: [[str]]) -> int:
        def bfs(grid, i, j):
            queue = [[i, j]]
            while queue:
                [i, j] = queue.pop(0)
                if 0 <= i < len(grid) and 0 <= j < len(grid[0]) and grid[i][j] == '1':
                    grid[i][j] = '0'
                    queue += [[i + 1, j], [i - 1, j], [i, j - 1], [i, j + 1]]
        count = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == '0': continue
                bfs(grid, i, j)
                count += 1
        return count
```

2.深度优先搜索DFS

```python
class Solution:
    def numIslands(self, grid: [[str]]) -> int:
        def dfs(grid, i, j):
            if not 0 <= i < len(grid) or not 0 <= j < len(grid[0]) or grid[i][j] == '0': return
            grid[i][j] = '0'
            dfs(grid, i + 1, j)
            dfs(grid, i, j + 1)
            dfs(grid, i - 1, j)
            dfs(grid, i, j - 1)
        count = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == '1':
                    dfs(grid, i, j)
                    count += 1
        return count
```



## 摩尔投票法

### [面试题17.10.主要元素](https://leetcode-cn.com/problems/find-majority-element-lcci/)

![image-20210905171110786](/images/LeetCode/image-20210905171110786.png)

#### 思路：

1. 若数组里只有一个数字，直接返回；
2. 对数组进行排序后，若两个相差一半长度的元素相等，则该元素就是主要元素；
3. 若遍历到一半还没出现，则证明没有主要元素。

```js
var majorityElement = function(nums) {
    l = parseInt(nums.length/2);
    if(!l){
        return nums[0];
    }
    nums.sort(); 
    for(let i = 0; i <= l; i++){
        if(nums[i] == nums[i+l]){
            return nums[i];
        }
    }
    return -1;
};
```

时间复杂度：O(nlogn)。排序复杂度超出了要求

空间复杂度：O(1)，没有用到多余的空间

执行用时：72ms，击败了94.35%的用户

内存消耗：42.2MB，击败了10.63%的用户

#### 官方题解：

Boyer-Moore投票算法

Boyer-Moore 投票算法的基本思想是：在每一轮投票过程中，从数组中删除两个不同的元素，直到投票过程无法继续，此时数组为空或者数组中剩下的元素都相等。

该题中表现为：遍历到x时，若count为0，则先将x的值赋给candidate；接下去若遇到不相同的，count--，遇到相同的，count++，若count为0，重新赋值。因此遍历一遍过后，如果数组nums 中存在主要元素，则candidate 即为主要元素，否则candidate 可能为数组中的任意一个元素。

因此需要二次遍历，看candidate出现的次数是否大于l/2。

【主要元素出现次数大于其他元素出现次数之和】

【== 和 === 的区别： == 会先将两边的值进行强制类型转换】 

```js
null == undefined //true
null === undefined //false
55 == '55' //true
55 === '55' //false  //因此推荐使用===
```

```js
var majorityElement = function(nums) {
    let candidate = -1; //候选主要元素，初始可以是任意值
    let count = 0; //候选主要元素出现的次数
    for (const num of nums) {
        if (count === 0) {
            candidate = num;
        }
        if (num === candidate) {
            count++;
        } else {
            count--;
        }
    }
    count = 0;
    const length = nums.length;
    for (const num of nums) {
        if (num === candidate) {
            count++;
        }
    }
    return count * 2 > length ? candidate : -1;
};
```



## 双指针 || 滑动窗口

### [3. 无重复字符的最长字串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

![image-20210905171211988](/images/LeetCode/image-20210905171211988.png)

#### 思路

- 最初的思路是，用两重循环，以数组每个元素为起点都做一次最大长度，保留最大
- 后面发现这样会超时(O(N^2))
- 改进成：遇到一个重复元素，则将pre中的该重复元素及其之前的元素截取掉

```js
 var lengthOfLongestSubstring = function(s) {
    let pre = [], counts = 0;
    let len = s.length;
    for(let i = 0; i < len; i++){
        let index = pre.indexOf(s[i]);
        if(index !== -1){
            pre.splice(0,(index + 1)); //截取数组，把前面的重复元素及其之前的元素删掉
        }
        pre.push(s[i]);
        counts = Math.max(counts,pre.length);
    }
    return counts;
};
```

时间复杂度：O(n)，遍历一次

空间复杂度：*O*(∣Σ∣)，最坏情况全不重复，则最长是所有字符的合集128

执行用时：76ms，击败了99.90%的用户

内存消耗：41.4MB，击败了71.54%的用户

#### 官方题解

```js
var lengthOfLongestSubstring = function(s) {
    // 哈希集合，记录每个字符是否出现过
    const occ = new Set();
    const n = s.length;
    // 右指针，初始值为 -1，相当于我们在字符串的左边界的左侧，还没有开始移动
    let rk = -1, ans = 0;
    for (let i = 0; i < n; ++i) {
        if (i != 0) {
            // 左指针向右移动一格，移除一个字符
            occ.delete(s.charAt(i - 1));
        }
        while (rk + 1 < n && !occ.has(s.charAt(rk + 1))) {
            // 不断地移动右指针
            occ.add(s.charAt(rk + 1));
            ++rk;
        }
        // 第 i 到 rk 个字符是一个极长的无重复字符子串
        ans = Math.max(ans, rk - i + 1);
    }
    return ans;
};
```

该做法称为“滑动窗口”

- 使用两个指针表示字符串的某个子串（窗口）的左右边界
- 每一步的操作中，将左指针向右移动一格，表示开始枚举下一个字符作为起始位置，然后不断地向右移动右指针，记下长度

### [26.删除有序数组中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

![image-20210905171301864](/images/LeetCode/image-20210905171301864.png)

#### 思路：

因为数组是有序的，所以遍历数组，当某元素和前一个元素不等，则代表它没出现过，把他放到第n个位置，n为出现过的元素个数

```js
var removeDuplicates = function(nums) {
    let len = nums.length;
    let res = 0;
    for(let i=0; i<len; i++){
        if(nums[i+1] != nums[i]){
            res++;
            nums[res] = nums[i+1];
        }
    }
    return res;
};
```

时间复杂度：O(n)

空间复杂度：O(1)

执行用时：76ms，击败了99.22%的用户

内存消耗：40.2MB，击败了47.62%的用户

#### 官方题解：

快慢指针，实际思想与自己无异

```js
var removeDuplicates = function(nums) {
    const n = nums.length;
    if (n === 0) {
        return 0;
    }
    let fast = 1, slow = 1;
    while (fast < n) {
        if (nums[fast] !== nums[fast - 1]) {
            nums[slow] = nums[fast];
            ++slow;
        }
        ++fast;
    }
    return slow;
};
```

### [27.移除指定元素](https://leetcode-cn.com/problems/remove-element/)

![image-20210905171351567](/images/LeetCode/image-20210905171351567.png)

#### 思路：

跟26题几乎一样，不一样的只是res的起点

```js
var removeElement = function(nums, val) {
    let res = 0;
    let len = nums.length;
    for(let i=0; i<len; i++){
        if(nums[i] !== val){
            nums[res++] = nums[i];
        }
    }
    return res;
};
```

时间复杂度：O(n)

空间复杂度：O(1)

执行用时：64ms，击败了99.51%的用户

内存消耗：38MB，击败了32.95%的用户

#### 官方题解：

除了快慢指针，还可以使用左右指针，在val元素数量小时很有效，**避免了需要保留的元素的重复赋值操作**。

```js
var removeElement = function(nums, val) {
    let left = 0, right = nums.length;
    while (left < right) {
        if (nums[left] === val) {
            nums[left] = nums[right - 1];
            right--;
        } else {
            left++;
        }
    }
    return left;
};
```

### [剑指offer52 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

![image-20210905171438009](/images/LeetCode/image-20210905171438009.png)

#### 官方题解

```js
 var getIntersectionNode = function(headA, headB) {
    if(headA===null || headB===null){
        return null;
    } /* 有一个为空，则没有交集 */
    let pA = headA;
    let pB = headB;
    while(pA!==pB){ /* 当他们没有相遇 */
        pA = (pA===null) ? headB : pA.next; 
        pB = (pB===null) ? headA : pB.next;
    }
    return pA;

};
```

有点没看懂问什么＋刚搬完寝室无心学习，直接看官方题解了

也属于双指针题目，但特殊的是，由于公共节点离两个头结点的位置是不同的，所以只遍历一遍自身是不够的，需要遍历完自身（若此时还没找到），再去从另一个的头结点处开始遍历，当两边都这么做，就能保证：若有公共节点，则同时停在公共节点处，也就意味着可以返回这个节点。若没有，则会同时处于null（两边都刚好遍历了lenA+lenB）

### 141. 环形链表

![image-20210905171523851](/images/LeetCode/image-20210905171523851.png)

#### 思路

- val值有限定范围    	小聪明，但值得注意的是，面试时应先问清楚面试官题目细节，如果刻意透露了限定条件，则利用反而会加分

```js
var hasCycle = function(head) {
    while(head){
        if(head.val==100001){
            return true;
        }
        else{
            head.val = 100001;
        }
        head = head.next; 
    }
    return false;
};
```

- 快慢指针，快的一次走两步，慢的一次走一步，快的能反过来追上慢的就有环

```js
 var hasCycle = function(head) {
    if(head==null || head.next==null){ //没环
        return false;
    }
    let fast = head.next, slow = head;
    while(fast){ //如果没环，fast会先到尾
        if(fast == slow){
            return true;
        }
        if(fast.next==null){
            return false;
        }
        fast = fast.next.next;
        slow = slow.next;
    }
    return false;
};
```

时间复杂度：O(n)

空间复杂度：O(1)

执行用时：76ms，击败了99.22%的用户

内存消耗：40.2MB，击败了47.62%的用户

#### 官方题解

1. 哈希表，用集合存储访问过的节点
2. 快慢指针

### [611. 有效三角形的个数](https://leetcode-cn.com/problems/valid-triangle-number/)

![image-20210905171641662](/images/LeetCode/image-20210905171641662.png)

#### 思路

- 暴力

```js
var triangleNumber = function(nums) {
    nums.sort((a, b) => a - b); /* sort方法仅适用于字符串，对数字需要这样操作 */
    let len = nums.length;
    let res = 0;
    for(let i = 0; i < len - 2; i++){
        if(nums[i] <= 0){
            continue; /* 三角形边长都是正数 */
        }
        for(let j = i + 1; j < len - 1; j++){
            for(let k = j + 1; k < len; k++){
                if(nums[i] + nums[j] > nums[k]){
                    res++;
                }
                else{
                    break;
                }
            }
        }
    }
    return res;
};
```

时间复杂度：O(N3)

执行用时：852ms，击败了23.08%的用户

内存消耗：39.4MB，击败了70.33%的用户

#### 官方题解



### [881. 救生艇](https://leetcode-cn.com/problems/boats-to-save-people/)

![image-20210905171743589](/images/LeetCode/image-20210905171743589.png)

#### 思路：

- 重点是每艘船只能载1或2人
- 排序，最轻的和最重的加，若不大于limit，左边指针后移一位
- 每次循环都将右指针前移一位，船数也加一

```js
var numRescueBoats = function(people, limit) {
    let res = 0;
    people.sort((a,b) => (a-b));
    let left = 0, right = people.length - 1;
    while(left <= right) {
        if(people[left] + people[right] <= limit) {
            left++;
        }
        right--;
        res++;
    }
    return res;
};
```

时间复杂度：O(nlogn) 排序

空间复杂度：O(1) 

执行用时：152ms，击败了93.16%的用户

内存消耗：45.5MB，击败了36.32%的用户

### 15. [三数之和](https://leetcode-cn.com/problems/3sum/)

![image-20210907152931008](/images/LeetCode/image-20210907152931008.png)

#### 思路：

- 此题的重点是，不能存在重复的三元组
- 要找到和为0的三元组，自然地想到需要先进行排序。排序后找第一个元素，这个元素的特点是不能和前一个元素相等，这也就保证了每个三元组的最小值不会相等，初步保证不会重复
- 接下去的操作类似于之前的两数之和题目，使用双指针来寻找，对于左指针指向的元素，拥有和对第一个元素一样的约束，这样也保证了第二个元素不会在第二个位置重复使用
- 当前两个确定后，由于和是0是固定的，第三个只需要正常寻找就可以

```js
var threeSum = function(nums) {
    let res = [];
    nums.sort((a,b)=>(a-b)); // 排序数组
    for(let i = 0; i < nums.length - 2; i++) { // 最后两个数不可能是三元组内第一个元素
        if(i > 0 && nums[i-1] === nums[i]) continue; // 确保第一个位置不重复
        let first = nums[i];
        let left = i + 1, right = nums.length - 1;
        while(left < right) {
            let second = nums[left], third = nums[right];
            if(first + second + third === 0) {
               res.push([first, second, third]);
               // 确保第二个位置不重复
               while(1) {
                   left++;
                   if(nums[left] !== nums[left-1] || left >= right) {
                       break;
                   }
               }
            }
            else if(first + second + third < 0) left++;
            else right--;
        }
    }
    return res;
};
```

时间复杂度：O(n^2)

空间复杂度：O(logn)

执行用时：136ms，击败了83.01%的用户

内存消耗：6MB，击败了34.90%的用户

## 基础线性表 || 矩阵 

### [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

![image-20210905171849815](/images/LeetCode/image-20210905171849815.png)

#### 思路

- 用carry表示进位
- head代表头结点，尾结点往后走到l1、l2都为空
- 如果用一个为空，另一个还未到结束，则空的那部分都是0

```js
 var addTwoNumbers = function(l1, l2) {
    let carry = 0; /* 第一次一定没有进位 */
    let head = null, tail = null;
    while(l1 || l2){
        const num1 = l1 ? l1.val : 0;
        const num2 = l2 ? l2.val : 0;
        const sum = num1 + num2 + carry;
        if(!head){
            head = tail = new ListNode(sum % 10);
        }
        else{
            tail.next = new ListNode(sum % 10);
            tail = tail.next;
        }
        carry = (sum >= 10) ? 1 : 0;
        if(l1){
            l1 = l1.next;
        }
        if(l2){
            l2=l2.next;
        }
    }
    if(carry == 1){
        tail.next = new ListNode(1);
        tail = tail.next;
    }
    return head;
};
```

时间复杂度：O(max(m,n))

空间复杂度：O(1)

执行用时：96ms，击败了99.90%的用户

内存消耗：43.2MB，击败了32.54%的用户

#### 官方题解

```
一样
```

### [21.合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

![image-20210905171920563](/images/LeetCode/image-20210905171920563.png)

#### 思路：

1. 创建结点head和tail
2. 先判断是否都不为空，若有一个为空则返回另一个，都为空则直接返回空结点
3. 都不为空的话，head指向小的那个list，然后进入while循环，根据val值决定tail延伸方向，有一个为空即跳出循环

```c
struct ListNode* mergeTwoLists(struct ListNode* l1, struct ListNode* l2){
    struct ListNode *head, *tail;
    if(l1 && l2){
        if(l1->val < l2->val){
            head = l1;
            l1 = l1->next;
        }
        else{
            head = l2;
            l2 = l2->next;
        }
        tail = head;
        while(l1 && l2){
            if(l1->val<l2->val){
                tail->next = l1;
                l1 = l1->next;
            }
            else{
                tail->next = l2;
                l2 = l2->next;
            }
            tail = tail->next;
        }
        tail->next = l1?l1:l2;
    }
    else if(l1){
        return l1;
    }
    else if(l2){
        return l2;
    }
    return head;
}
```

时间复杂度：O(n+m)

空间复杂度：O(n+m)

执行用时：4ms，击败了90.26%的用户

内存消耗：6MB，击败了59.92%的用户

#### 官方递归解法：

```C
struct ListNode* mergeTwoLists(struct ListNode* l1, struct ListNode* l2) {
    if(l1==NULL)
        return l2;
    if(l2==NULL)
        return l1;
    if(l1->val < l2->val){
        l1->next = mergeTwoLists(l1->next,l2);
        return l1;
    }else{
        l2->next = mergeTwoLists(l1,l2->next);
        return l2;
    }
}
```

#### 递归：

- 必须要有边界条件，否则递归无法停止将会出错
- 递归函数通过不断调用自身，直至遇到边界条件后进行回溯，返回最终答案。

### [448.找到消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

![image-20210905172035800](/images/LeetCode/image-20210905172035800.png)

#### 	思路：

1. 数字大小都在区间[1,n]内，项数范围是[0,n-1]
2. 则每个元素的绝对值减去1，就代表其中一个项的位置
3. 把该位置的元素变为负值
4. 则仍未正数的元素的下标，就是代表没有出现的数字 - 1

```javascript
var findDisappearedNumbers = function(nums) {
    len = nums.length; //获取数组长度
    var res = new Array(); //存放答案的新数组
    for(let i=0;i<len;i++){
        let num = Math.abs(nums[i]) - 1; //项数
        if(nums[num] > 0){
            nums[num] *= -1; //包含的项变负
        }
    }
    for(let i=0;i<len;i++){
        if(nums[i]>0){
            res.push(i+1); //添加正数
        }
    }
    return res;
};
```

时间复杂度：O(n)

空间复杂度：符合题目要求，除了返回的数组以外不占用其他空间

执行用时：112ms，击败了97.43%的用户

内存消耗：45.2MB，击败了99.32%的用户

#### 官方题解的做法：

每遇到一次x，就让nums[x-1]加上n，最后数组里仍处在[1,n]范围内的项，下标加一就是消失的数字。

```js
var findDisappearedNumbers = function(nums) {
    const n = nums.length;
    for (const num of nums) {
        const x = (num - 1) % n; // %n很重要
        nums[x] += n;
    }
    const ret = [];
    for (const [i, num] of nums.entries()) {
        if (num <= n) {
            ret.push(i + 1);
        }
    }
    return ret;
};
```

### [54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

![image-20210905172127345](/images/LeetCode/image-20210905172127345.png)

#### 思路：

一层层剥开矩阵，用矩阵的四个边界控制，每走一圈（每剥开一层），矩阵缩小一次

```js
 var spiralOrder = function(matrix) {
    const rows = matrix.length; /* 行数 */
    const columns = matrix[0].length; /* 列数 */
    let res = [];
    if(rows==0 || columns==0){
        return res;
    }
    let left = 0, right = columns - 1, top = 0, bottom = rows - 1;
    while(left<=right && top<=bottom){
        for(let j=left; j<=right; j++){
            res.push(matrix[top][j]);
        }
        for(let i=top+1; i<=bottom; i++){
            res.push(matrix[i][right]);
        }
        if(left<right && top<bottom){
            for(let j=right-1; j>left; j--){
                res.push(matrix[bottom][j]);
            }
            for(let i=bottom; i>top; i--){
                res.push(matrix[i][left]);
            }
        }
        [left,right,top,bottom] = [left + 1, right - 1, top + 1, bottom - 1];
    }
    return res;
};
```

时间复杂度：O(mn)，遍历矩阵

空间复杂度：O(1)，除了返回的数组，没有用到多余空间

执行用时：60ms，击败了99.00%的用户

内存消耗：37.7MB，击败了48.38%的用户

#### 官方题解：

一样

## 贪心

### [1736. 替换隐藏数字得到的最晚时间](https://leetcode-cn.com/problems/latest-time-by-replacing-hidden-digits/)

![image-20210905172213963](/images/LeetCode/image-20210905172213963.png)

#### 思路:

从第一位开始判断：因为第一位最重要，直接if else枚举

```js
 var maximumTime = function(time) {
    const arr = Array.from(time); //Array.from:从一个类似数组或可迭代对象创建一个新的、浅拷贝的数组实例
    if(arr[0]=='?'){
        if(arr[1]<=4 || arr[1]=='?'){
            arr[0] = 2;
        }
        else{
            arr[0] = 1;
        }
    }
    if(arr[1]=='?'){
        if(arr[0]=='?' || arr[1]==2){
            arr[1] = 4;
        }
        else{
            arr[1] = 9;
        }
    }
    if(arr[3]=='?'){
        arr[3] = 5;
    }
    if(arr[4]=='?'){
        arr[4] = 9;
    }
    return arr.join(''); //用join方法来将各元素连接
};
```

时间复杂度：O(1)

空间复杂度：O(1)

执行用时：72ms，击败了90.00%的用户

内存消耗：37.8MB，击败了84.00%的用户

#### 官方题解

```js
var maximumTime = function(time) {
    const arr = Array.from(time);
    if (arr[0] === '?') {
        arr[0] = ('4' <= arr[1] && arr[1] <= '9') ? '1' : '2';
    }
    if (arr[1] === '?') {
        arr[1] = (arr[0] == '2') ? '3' : '9';
    }
    if (arr[3] === '?') {
        arr[3] = '5';
    }
    if (arr[4] === '?') {
        arr[4] = '9';
    }
    return arr.join('');
};
```

思路一致，官方代码利用正则表达式，更加简洁

贪心算法：是一种在每一步选择中都采取在当前状态下最好或最优（即最有利）的选择，从而希望导致结果是最好或最优算法

## 哈希表

### [705. 设计哈希集合](https://leetcode-cn.com/problems/design-hashset/)

![image-20210905172258007](/images/LeetCode/image-20210905172258007.png)

```js
var MyHashSet = function() {
    this.BASE = 769; // 哈希函数用的取模方法，选取一个较大质数
    this.data = new Array(this.BASE).fill(0).map(() => new Array());
};
MyHashSet.prototype.add = function(key) {
    const h = key % this.BASE;
    const it = this.data[h];
    for(let i = 0; i < it.length; i++) {
        if(it[i] === key) {
            return; // 已经存在，不添加
        }
    }
    it.push(key);
};
MyHashSet.prototype.remove = function(key) {
    const h = key % this.BASE;
    const it = this.data[h];
    for(let i = 0; i < it.length; i++) {
        if(it[i] === key) {
            it.splice(i, 1); // 删除
            return;
        }
    }
};
MyHashSet.prototype.contains = function(key) {
    const h = key % this.BASE;
    const it = this.data[h];
    for(let i = 0; i < it.length; i++) {
        if(it[i] === key) {
            return true;
        }
    }
    return false;
}
```

### [706. 设计哈希映射](https://leetcode-cn.com/problems/design-hashmap/)

![image-20210905172332695](/images/LeetCode/image-20210905172332695.png)

```js
var MyHashMap = function() {
    this.BASE = 769;
    this.data = new Array(this.BASE).fill(0).map(() => new Array());
};
MyHashMap.prototype.put = function(key, value) {
    const h = this.hash(key);
    for(let it of this.data[h]) {
        if(it[0] === key) {
            it[1] = value;
            return;
        }
    }
    this.data[h].push([key, value]);
};
MyHashMap.prototype.get = function(key) {
    const h = this.hash(key);
    for(let it of this.data[h]) {
        if(it[0] === key) {
            return it[1];
        }
    }
    return -1
};
MyHashMap.prototype.remove = function(key) {
    const h = this.hash(key);
    for(let it of this.data[h]) {
        if(it[0] === key) {
            this.data[h].splice(this.data[h].indexOf(it), 1);
            return;
        }
    }
};
MyHashMap.prototype.hash = function(key) {
    return key % this.BASE;
};
```

### [1.两数之和](https://leetcode-cn.com/problems/two-sum/)

![image-20210905172415655](/images/LeetCode/image-20210905172415655.png)

#### 思路：

- 暴力，双重循环
- 哈希表

```js
var twoSum = function(nums, target) {
    let hashMap = {};
    for(let i = 0; i < nums.length; i++){
        if(hashMap[target - nums[i]] !== undefined){
            return [i, hashMap[target - nums[i]]];
        }
        hashMap[nums[i]] = i; //存放的是该元素的索引值
    }
    return [];
};
```



### [128. 最长连续子序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

![image-20211006145530245](../../static/images/LeetCode/image-20211006145530245.png)

#### 思路：

因为时间复杂度要求为O(n)，所以不能使用排序算法。而很自然就想到应该使用哈希表来解决这个问题`let hashNums = new Set(nums)`

得到这样一个集合后，对于在数组内的元素，我们查找它的左邻居(比它小1的数字)是否在集合里，若在则长度加一，并保存长度然后从它的右邻居开始找，直到它的右邻居不在集合里

```js
var longestConsecutive = function(nums) {
    res = 0
    let myHash = new Set(nums)
    for(let num of myHash) {
        if(!myHash.has(num - 1)) {
            let currentNum = num
            let currentLen = 1
            while(myHash.has(currentNum + 1)) {
                currentNum++
                currentLen++
            }
            res = Math.max(res, currentLen)
        }
    }
    return res
};

```




