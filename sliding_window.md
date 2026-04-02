# 概念

`滑动窗口`是一种常用于解决`数组/字符串`相关问题的算法思想，可以用来`减少不必要的重复计算`，从而提高效率，特别是在`处理连续数据`的场景下

`滑动窗口`通常涉及到一个窗口（可以是数组或字符串的一部分），这个窗口可以在数据结构上向前滑动
- 不断的调节子序列的起始位置和终止位置，从而得出要想的结果
- 窗口的大小可以是`固定`的也可以是`动态变化`的

优点：
- `高效`：通过避免重复计算，可以减少不必要的计算，提高效率
- `直观`：这种方法很直观，易于理解和实现

# 滑动窗口的动机从何而来？

| 触发因素 | 对应判断 |
| --- | --- |
| 目标包含连续区间 | 窗口思想可行 |
| 区间大小固定 | 固定长度窗口 |
| 需高效判断区间中是否所有元素满足某条件 | 滑窗 + 条件检查 |
| 可提前剪枝跳过无效区间 | 滑窗结构天然适合 |

> 注意：滑动窗口仅在所有数字都`非负`时才有效，因为`窗口扩展/收缩依赖于总和单调性`

# 使用场景

`滑动窗口`算法适用于许多类型的问题，如：
- `固定大小的问题`：如求长度为 `K` 的`子数组/子字符串`的`最大或最小值`
- `可变大小的问题`：如找到`数组/字符串`中满足`特定条件`的`最小或最大长度的子数组/子字符串`

> - 滑动窗口算法的时间复杂度通常是 `O(n)`，其中 `n` 是数组或字符串的长度
> - 空间复杂度则取决于窗口的大小和维护窗口状态所需的额外空间
  
## 定长滑动窗口

总结成三步：`入 - 更新 - 出`
- `入`：下标为 `i` 的元素进入窗口，更新相关统计量
- `更新`：更新答案，`一般是更新最大值/最小值`
- `出`：下标为 `i−k+1` 的元素离开窗口，更新相关统计量

以上三步适用于所有`定长滑窗题目`

```python
class Solution:
    def maxVowels(self, s: str, k: int) -> int:
        res = count = 0
        for i, c in enumerate(s):
            # 1. 进入窗口
            if c in "aeiou":
                count += 1
            if i < k - 1:  # 窗口大小不足 k
                continue
            # 2. 更新答案
            res = max(res, count)
            # 3. 离开窗口
            if s[i - k + 1] in "aeiou":
                count -= 1
        return res
```

## 基于“滑动窗口 + 字符频率 + valid 计数”的模板

适用场景：
- 判断是否包含某些字符/异位词
- 找到所有异位词
- 最小窗口包含所有字符（如 LeetCode 76）
- 子串满足特定频率要求

| 场景 | 如何修改模板 |
| --- | --- |
| 找到所有异位词（如：LeetCode 438）| 保持 `right - left >= len(t)`，每次 `valid == len(need)` 就 `res.append(left)` |
| 判断是否包含异位词（如：LeetCode 567）| 一旦满足 `valid == len(need)` 就 `return True` |
| 最小覆盖子串（如：LeetCode 76）| 条件是 `valid == len(need)`，然后维护 `min_len` 和 `start`，不断缩小窗口 |
| 包含所有字符的最短子串 | 同上，但需在窗口满足后尽量缩小 `left` |

通用模板代码：
```python
from collections import defaultdict

def sliding_window_template(s: str, t: str) -> str:
    need = defaultdict(int)
    window = defaultdict(int)
    for c in t:
        need[c] += 1

    left = right = 0
    valid = 0  # 记录当前窗口满足 need 条件的字符种类数

    # 根据题意定义结果变量，例如最小长度、起始位置、答案列表等
    res = []

    while right < len(s):
        # c 是将要加入窗口的字符
        c = s[right]
        right += 1

        # 更新窗口内数据
        if c in need:
            window[c] += 1
            if window[c] == need[c]:
                valid += 1

        # 判断左侧窗口是否需要收缩
        while right - left >= len(t):  # 对于固定长度匹配，如异位词
        # while valid == len(need):     # 对于所有 need 被满足，如最小覆盖子串
            # 如果满足条件，更新结果
            if valid == len(need):
                res.append(left)  # 对于找异位词，记录当前窗口起点
                # res = s[left:right]    # 对于最小覆盖子串，更新最小结果

            # d 是将要从窗口移除的字符
            d = s[left]
            left += 1

            # 更新窗口内数据
            if d in need:
                if window[d] == need[d]:
                    valid -= 1
                window[d] -= 1

    return res  # 或返回符合条件的子串、长度等
```

## 从两端拿固定长度

这可以使用逆向思维，从两端变成从中间

# 真题示例

