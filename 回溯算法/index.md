# 

# 回溯法

## 什么是回溯法

回溯法是一种搜索的方式，也称回溯搜索法

**回溯是递归的副产品，只要有递归，就会有回溯**

## 回溯法的效率

**回溯的本质是穷举**，回溯法本身效率并不高，想使其变得更高效，可以加上一些剪枝的操作，但这也无法改变穷举本质

## 回溯法解决的问题

- 组合问题：N 个数字按照给定规则找出 k 个数的集合
- 切割问题：一个字符串按给定规则有几种切割方式
- 子集问题：N 个数的集合里有多少个符合条件的子集
- 排列问题：N 个数按照给定规则排列，有几种方式
- 棋盘问题：N 皇后，数独

## 理解回溯法

**回溯法能解决的问题都可以抽象为树形结构**

回溯法其实就是解决在集合中递归查找子集，所以**树的宽度就是集合的大小，树的深度就是递归的深度**

递归有终止条件，所以保证了树的深度有限

## 回溯法模板

### 返回值以及参数

```javascript
const backtracking (params) => {
    return
}
```

回溯法所需的参数很难一次性确定，可以先写逻辑，然后把用到的参数补充进来就可以了

### 终止条件

```javascript
if (end) {
    // save result
    return
}
```

什么时候打打了终止条件，就把答案存起来，结束本层递归

### 遍历过程

```javascript
for (const element of elementsOfThisLevel) {
    // 处理元素
    // backtracking
    // 回溯，撤销处理结果
}
```

for 循环是横向遍历，backtracking 是纵向遍历，就能把树遍历完，一般来说，叶子节点就是其中一个结果

### 模板框架组合

```javascript
const backtracking = (params) => {
    if (end) {
        // 存放结果
        return
    }
    for (const element of elementsOfThisLevel) {
    	// 处理元素
    	// backtracking
    	// 回溯，撤销处理结果
	}   
}
```



## 实战

### [组合](https://leetcode.cn/problems/combinations/)

```
给定两个整数 n 和 k，返回范围 [1, n] 中所有可能的 k 个数的组合。

你可以按 任何顺序 返回答案。
```

#### 思路

分析一下，n 就是树的宽度，k 就是树的深度，每次到了叶子节点，就是一个结果

**每次从集合中选一个元素，可选择的范围随着选择的进行而收缩**

#### 题解

```javascript
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
var combine = function (n, k) {
    const res = [], path = []
    const backtracking = (n, k, start) => {
        const len = path.length
        if (len === k) {
            res.push(path)
            return
        }
        for (let i = start; i <= n - (k - len) + 1; i++) {
            path.push(i)
            backtracking(n, k, i + 1)
            path.pop()
        }
    }
    backtracking(n, k, 1)
    return res
};
```






















