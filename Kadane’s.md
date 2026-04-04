Kadane’s Algorithm 是一种用来解决 Maximum Subarray / 最大子数组和 问题的经典线性算法。

它的核心思想非常简单：

遍历数组时，维护“以当前位置结尾的最大子数组和”。

也就是说，走到 nums[i] 时，你只问自己一个问题：

以 nums[i] 结尾的最好结果，是
1）从当前元素重新开始
还是
2）把前面的子数组接上当前元素？

所以状态转移就是：

𝑑
𝑝
[
𝑖
]
=
max
⁡
(
𝑛
𝑢
𝑚
𝑠
[
𝑖
]
,
𝑑
𝑝
[
𝑖
−
1
]
+
𝑛
𝑢
𝑚
𝑠
[
𝑖
]
)
dp[i]=max(nums[i],dp[i−1]+nums[i])

其中：

dp[i] 表示 以 i 结尾的最大子数组和
最终答案是所有 dp[i] 里的最大值

更直观地说：

如果前面的累积和是负数，那它只会拖后腿。
那就不要它，直接从当前元素重新开始。

所以 Kadane 的本质就是这句：

负贡献前缀直接丢掉。

最常见写法：

def maxSubArray(nums):
    cur_sum = nums[0]
    res = nums[0]

    for i in range(1, len(nums)):
        cur_sum = max(nums[i], cur_sum + nums[i])
        res = max(res, cur_sum)

    return res

比如数组：

[-2, 1, -3, 4, -1, 2, 1, -5, 4]

走到 4 时，前面的和是负的，所以直接从 4 重新开始更好。
后面继续累加：

4
4 + (-1) = 3
3 + 2 = 5
5 + 1 = 6

所以最大子数组和是 6，对应子数组是：

[4, -1, 2, 1]

它的复杂度很优秀：

Time: O(n)
Space: O(1)

如果你想用一句英文在面试里解释，可以直接说：

Kadane’s Algorithm keeps track of the maximum subarray sum ending at each position.
At each step, we either start a new subarray from the current element or extend the previous one.
If the previous running sum is negative, we discard it because it can only make the future result worse.