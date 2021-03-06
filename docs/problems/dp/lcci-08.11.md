---
title: 面试题 08.11. 硬币
sidebarDepth: 0
---
## 简介
- [面试题 08.11. 硬币](https://leetcode-cn.com/problems/coin-lcci/)

## 解法一 - 动态规划
本题属于完全背包问题。具体总结看《背包九讲》

题目要求的是：以 $4$ 种面值的货币（每种个数无限），求有多少种方式可以组合成 $n$。

我们假设 $f(i, v)$ 表示前 $i$ 种面值可以组合成 $v$ 的方案个数，那么最终答案就是 $f(4, n)$

我们可以直接得出递推公式： $f(i, v) = \sum_{j=0}^{k}f(i-1, v-j \times c_i), k = \lfloor \frac{v}{c_i} \rfloor$

这样，由于 $i$ 有 $4$ 种取值， $v$ 的取值有 $n+1$ 种，而 $k$ 的取值有 $\lfloor \frac{v}{c_i} \rfloor$，那么状态总数有 $4(n+1) \times \lfloor \frac{v}{c_i} \rfloor$。这样我们可以通过三重循环来实现这个方程。

首先对于第三个变量 $k$，只能够取整数，因此我们尝试着优化：

$f(i, v) = f(i-1, v) + f(i-1, v- c_i) + ... + f(i-1, v-Kc_i)$

$f(i, v-c_i) = f(i-1, v- c_i) + ... + f(i-1, v-Kc_i)$

因此 $f(i, v) = f(i-1, v) + f(i, v-c_i)$。此时我们可以通过二维数组来解决整个问题，然后使用动态规划常见的空间优化策略（滚动数组），我们可以将空间复杂度降低到一位数组，证明忽略。

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var waysToChange = function(n) {
    let MOD = 1000000007;
    let memo = new Array(n+1).fill(0);

    memo[0] = 1;

    let coins = [1, 5, 10, 25];

    for(let i = 0; i < coins.length; i++) {
        for(let j = coins[i]; j <= n; j++) {
            memo[j] = (memo[j] + memo[j-coins[i]]) % MOD;
        }
    }
    return memo[n];
}
```

**复杂度分析**:
- 时间复杂度： $O(N)$
- 空间复杂度： $O(N)$

## 解法二 - 数学
$n = 25a + 10b + 5c + d$，求表示法的个数就是求 $a,b,c$ 有几种组合。

$a \in [0, \lfloor \frac{n}{25} \rfloor]$， $b \in [0, \lfloor \frac{n-25a}{10} \rfloor]$，$c \in [0, \lfloor \frac{n-25a-10b}{5} \rfloor]$

我们通过枚举所有可能的 $a$ 来约去公式中的 $a$。

那么对于不同的 $a$，假设 $rest = {n-25a}, m_1 = \lfloor rest/10\rfloor, m_2 = \lfloor rest/5\rfloor$，则 $\sum_{b=0}^{m_1}m_2-2b+1 =(m_1+1)(m_2+1) -m_1(1+m_1)$。

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var waysToChange = function(n) {
    let MOD = 1000000007;

    let a = Math.floor(n/25);
    let ans = 0;
    for(let i = 0; i <= a; i++) {
        let res = n - i * 25;
        let m1 = Math.floor(res/10);
        let m2 = Math.floor(res/5);

        ans =  (ans + (m1+1)*(m2+1) - m1*(m1+1))%MOD;
    }

    return ans;
}
```

**复杂度分析**:
- 时间复杂度： $O(\lfloor \frac{n}{25}\rfloor)$
- 空间复杂度： $O(1)$