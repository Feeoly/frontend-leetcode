---
title: 面试题 10.01. 合并排序的数组
sidebarDepth: 0
---
 
## 简介
- [面试题 10.01. 合并排序的数组](https://leetcode-cn.com/problems/sorted-merge-lcci/)

## 思路
### 解法一 - 二路归并
本身 A 和 B 都是有序的，返回结果是合并后的 A。
1. 申请新的数组空间复制 A'
2. 二路归并 A' 和 B 到 A 中

```javascript
var merge = function(A, m, B, n) {
    let copy = new Array(m);
    for(let i = 0; i < m; i++) {
        copy[i] = A[i];
    }

    let i = 0;
    let j = 0;
    let count = 0;
    while(i < m && j < n) {
        if(copy[i] < B[j]) {
            A[count] = copy[i++]; 
        } else {
            A[count] = B[j++];
        }
        count++;
    }

    if(j < n) {
        i = j;
        copy = B;
    }
    for(let k = count; k < m+n; k++) {
        A[k] = copy[i++];
    }

};
```
**复杂度分析**:
- 时间复杂度： $O(N+M)$
- 空间复杂度： $O(M)$

### 解法二 - 二路归并的变体
解法一的思路是从小的位置进行归并，这样带来的后果是 A 的值可能会被覆盖。
因为 A 后面是空的，因此我们反过来从大的位置开始归并。这样可以节省空间。

```javascript
var merge = function(A, m, B, n) {

    let i = m-1;
    let j = n-1;
    let count = m+n-1;
    while(i >= 0 && j >= 0) {
        if(A[i] < B[j]) {
            A[count] = B[j--]; 
        } else {
            A[count] = A[i--];
        }
        count--;
    }

    while(j >= 0) {
        A[count--] = B[j--];
    }
};
```

**复杂度分析**:
- 时间复杂度： $O(N+M)$
- 空间复杂度： $O(1)$