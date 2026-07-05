## 概念

`Kadane’s Algorithm` 就是专门用来解决：`连续子数组的最大和` 的经典算法

核心思想只有一句：
- 对每个数字，判断：**是接着前面的 `subarray` 加下去，还是从当前数字重新开始？**

核心公式

```python
current_sum = max(nums[i], current_sum + nums[i])
max_sum = max(max_sum, current_sum)
```

意思是：
- current_sum = 以当前 nums[i] 结尾的最大子数组和
- max_sum = 到目前为止见过的最大子数组和
  
**为什么可以这样？**

如果前面的 current_sum 是负数，它只会拖累当前数字

比如：

current_sum = -5
nums[i] = 4

那：

继续加：-5 + 4 = -1
重新开始：4

明显重新开始更好

所以： `current_sum = max(nums[i], current_sum + nums[i])`

例子: nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]

当走到 4 的时候，前面的和已经不好了，所以从 4 重新开始：

4 -> 4
4 + (-1) = 3
3 + 2 = 5
5 + 1 = 6

最大就是：

[4, -1, 2, 1] = 6

**最简记忆**
- current_sum：当前这段要不要继续
- max_sum：历史最大答案

代码：

```python
current_sum = nums[0]
max_sum = nums[0]

for i in range(1, len(nums)):
    current_sum = max(nums[i], current_sum + nums[i])
    max_sum = max(max_sum, current_sum)

return max_sum
```

**一句话：`前面是负担就丢掉，前面有帮助就继续加`**

## 跟 prefix sum 的区别

不一样，但有关系

**Prefix Sum 是什么？**

Prefix Sum 适合问：`某个固定区间 [i, j] 的和是多少？`

公式：`sum(i, j) = prefix[j + 1] - prefix[i]`

它主要解决：`快速求任意子数组和`

**Kadane 是什么？**

Kadane 适合问：`所有连续子数组里，哪个子数组的和最大？`

它不是单纯求某个区间和，而是边扫边决定：`前面的 subarray 要不要继续带着？`

核心：
- `current_sum = max(nums[i], current_sum + nums[i])`
- `max_sum = max(max_sum, current_sum)`
  
**它们的关系**

`Maximum Subarray` 也可以用 `Prefix Sum` 理解

子数组和：`nums[j...i] = prefix[i] - prefix[j - 1]`

要让这个值最大，就是：`当前 prefix - 前面最小的 prefix`

所以也可以写成 Prefix Sum 版本：

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        prefix_sum = 0
        min_prefix = 0
        max_sum = nums[0]

        for num in nums:
            prefix_sum += num
            max_sum = max(max_sum, prefix_sum - min_prefix)
            min_prefix = min(min_prefix, prefix_sum)

        return max_sum
```


> - Kadane is like an optimized DP/greedy version for maximum subarray. 
> - Prefix sum can also solve it by tracking the minimum prefix sum so far, but Kadane is usually the standard interview answer.

中文记忆：
- `Prefix Sum`：快速算某一段的和
- `Kadane`：动态决定最大连续子数组和
