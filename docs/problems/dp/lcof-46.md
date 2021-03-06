---
title: 面试题46. 把数字翻译成字符串
sidebarDepth: 0
---
## 题目链接
- [面试题46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

## 解法一 - 动态规划
由题意可以看出，要求我们给出所有可能不同的翻译方法，其实就是给出符合要求的切割方法数量。题目告诉我们，$[0,...,25]$ 是合法的最小切割单位。很显然，我们最后一次切割依赖于上一次切割的位置和可能数量，很显然可以通过动态规划来解决。

假设 $dp[i]$ 表示从 $[0, ...,i-1]$ 范围的合法切割数量。则：
$$
dp[i] = dp[i-1] + [i-2, i-1] ? dp[i-2] : 0
$$

那么边界情况是什么？
$dp[0] = 1, dp[1] = 1$ ，$dp[0]$ 表示为空时的切割数量，$dp[1]$ 表示只有一个数字时的切割数量。

```javascript
var translateNum = function(num) {
    let str = '' + num;
    let dp = new Array(str.length+1).fill(0);

    dp[0] = 1;
    dp[1] = 1;

    for(let i = 2; i <= str.length; i++) {
        dp[i] += dp[i-1];

        if(str[i-2] === '0') continue;

        let tmp = parseInt(str[i-2] + str[i-1]);
        if(tmp < 26) dp[i] += dp[i-2];
    }

    return dp[str.length];
};
```
**复杂度分析**:
- 时间复杂度：$O(log(num))$
- 空间复杂度：$O(log(num))$


**通过滚动数组的技巧优化空间**
```javascript
/**
 * @param {number} num
 * @return {number}
 */
var translateNum = function(num) {
    let str = '' + num;

    dp0 = 1;
    dp1 = 1;

    for(let i = 2; i <= str.length; i++) {
        let tmp = dp1;
        if(str[i-2] !== '0' && parseInt(str[i-2] + str[i-1]) < 26) dp1 += dp0;
        dp0 = tmp; 
    }

    return dp1;
};
```
**复杂度分析**:
- 时间复杂度：$O(log(num))$
- 空间复杂度：$O(log(num))$, 优化了常系数