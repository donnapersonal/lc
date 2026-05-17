# 2064.分配给商店的最多商品的最小值

题目链接：[2064.分配给商店的最多商品的最小值](https://leetcode.cn/problems/minimized-maximum-of-products-distributed-to-any-store/)

## 题目大意

给你一个整数 n ，表示有 n 间零售商店。总共有 m 种商品，每种商品的数目用一个下标从 0 开始的整数数组 quantities 表示，其中 quantities[i] 表示第 i 种商品的数目

你需要将 所有商品 分配到零售商店，并遵守这些规则：

- 一间商店 至多 只能有 一种商品 ，但一间商店拥有的商品数目可以为 任意 件
- 分配后，每间商店都会被分配一定数目的商品（可能为 0 件）。用 x 表示所有商店中分配商品数目的最大值，你希望 x 越小越好。也就是说，你想 最小化 分配给任意商店商品数目的 最大值 
  
请你返回最小的可能的 x 

```js
Example 1:
Input: n = 6, quantities = [11,6]
Output: 3
Explanation: One optimal way is:
- The 11 products of type 0 are distributed to the first four stores in these amounts: 2, 3, 3, 3
- The 6 products of type 1 are distributed to the other two stores in these amounts: 3, 3
The maximum number of products given to any store is max(2, 3, 3, 3, 3, 3) = 3.

Example 2:
Input: n = 7, quantities = [15,10,10]
Output: 5
Explanation: One optimal way is:
- The 15 products of type 0 are distributed to the first three stores in these amounts: 5, 5, 5
- The 10 products of type 1 are distributed to the next two stores in these amounts: 5, 5
- The 10 products of type 2 are distributed to the last two stores in these amounts: 5, 5
The maximum number of products given to any store is max(5, 5, 5, 5, 5, 5, 5) = 5.

Example 3:
Input: n = 1, quantities = [100000]
Output: 100000
Explanation: The only optimal way is:
- The 100000 products of type 0 are distributed to the only store.
The maximum number of products given to any store is max(100000) = 100000.
```

限制：
- m == quantities.length
- 1 <= m <= n <= 10^5
- 1 <= quantities[i] <= 10^5

## 解题

把 quantities[i] 视作木棍的长度，你有 m 根木棍，要切分成 n 段（长度可以是 0），让最长的那根木棍尽量短

看示例 1，有两根长度分别为 11 和 6 的木棍，要切分成 n=6 段（长度可以是 0）

最长的木棍越短，要求就越苛刻。如果切分后，最长的木棍长度不能是 2（因为切分出的段数超过 n），那么也不能是 1 这样更短的长度（切分出的段数更多）

最长的木棍越长，要求就越宽松。如果切分后，最长的木棍长度可以是 3，那么也可以是 4，是 5 等更大的长度

据此，可以`二分猜答案`

现在问题变成一个判定性问题：`如果要求切分后每段木棍的长度都 ≤mx，切出的段数能否 ≤n？`

```js
/**
 * @param {number} n
 * @param {number[]} quantities
 * @return {number}
 */
var minimizedMaximum = function(n, quantities) {
    function check(x) {
        let count = 0;

        for (const q of quantities) {
            count += Math.floor((q - 1) / x) + 1;
        }

        return count <= n;
    }

    let l = 1;
    let r = Math.max(...quantities) + 1;

    while (l < r) {
        const mid = l + Math.floor((r - l) / 2);

        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }

    return l;
};
```
```python
class Solution:
    def minimizedMaximum(self, n: int, quantities: List[int]) -> int:
        # 判定问题
        def check(x: int) -> bool:
            # 计算所需商店数量的最小值，并与商店数量进行比较
            count = 0
            for q in quantities:
                count += (q - 1) // x + 1
            return count <= n
        
        l, r = 1, max(quantities) + 1
        # 二分查找寻找最小的使得判定问题为真的 x
        while l < r:
            mid = l + (r - l) // 2
            if check(mid):
                r = mid
            else:
                l = mid + 1
        return l
```
```java
class Solution {
    public int minimizedMaximum(int n, int[] quantities) {
        int left = 1;
        int right = 0;

        for (int q : quantities) {
            right = Math.max(right, q);
        }

        right = right + 1;

        while (left < right) {
            int mid = left + (right - left) / 2;

            if (check(mid, n, quantities)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }

        return left;
    }

    private boolean check(int x, int n, int[] quantities) {
        int count = 0;

        for (int q : quantities) {
            count += (q - 1) / x + 1;
        }

        return count <= n;
    }
}
```

- 时间复杂度：`O(mlogU)`，其中 `m` 是 `quantities` 的长度，`U=max(quantities)`
- 空间复杂度：`O(1)`