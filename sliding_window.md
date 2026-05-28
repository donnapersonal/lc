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

# 规律

题目`在处理“连续子数组 / 连续子字符串”，且窗口可以随着左右指针移动来维护答案`

也就是：
- right 负责扩大窗口
- left 负责缩小窗口
- 窗口中维护当前状态
- 每次判断当前窗口是否满足条件

## 什么时候想到 Sliding Window？

看到这些关键词，优先想滑动窗口：

| 关键词                              | 含义                                 |
| -------------------------------- | ---------------------------------- |
| substring / subarray             | 连续子串 / 连续子数组                       |
| longest                          | 求最长合法窗口                            |
| shortest / minimum               | 求最短合法窗口                            |
| at most K                        | 最多 K 个                             |
| exactly K                        | 恰好 K 个，常转成 atMost(K) - atMost(K-1) |
| contains / permutation / anagram | 维护字符频率                             |
| product / sum less than          | 维护窗口乘积或和                           |
| replace / flip                   | 允许修改 K 次，本质是窗口内违规数量 ≤ K            |

## 一、最长窗口类

目标通常是：找满足条件的最长连续区间

常见写法：

for right in range(n):
    加入 nums[right]

    while 窗口不合法:
        移出 nums[left]
        left += 1

    更新最长答案

对应题：
| 题目                                                | 为什么用滑动窗口             |
| ------------------------------------------------- | -------------------- |
| 3. Longest Substring Without Repeating Characters | 窗口内不能有重复字符           |
| 424. Longest Repeating Character Replacement      | 窗口内最多替换 k 个字符        |
| 1004. Max Consecutive Ones III                    | 窗口内最多有 k 个 0         |
| 904. Fruit Into Baskets                           | 窗口内最多 2 种水果          |
| 1838. Frequency of the Most Frequent Element      | 排序后窗口内元素可被增加成同一个数    |
| 1456. Maximum Number of Vowels                    | 固定长度窗口统计元音数量         |
| 1438. Longest Continuous Subarray                 | 窗口最大值和最小值差不能超过 limit |

例子：`3. Longest Substring Without Repeating Characters`

要求：最长无重复字符子串

窗口维护：当前窗口里有哪些字符

如果右指针加入字符后重复了，就移动左指针，直到没有重复。

本质：右边扩展，发现重复就左边收缩

例子：`1004. Max Consecutive Ones III`

要求：最多把 k 个 0 翻成 1，最长连续 1

窗口维护：窗口里 0 的数量

只要：`zero_count <= k` 窗口就是合法的

如果 `zero_count > k`，左指针右移

## 二、最短窗口类

目标通常是：找满足条件的最短连续区间

常见写法：

for right in range(n):
    加入 s[right]

    while 窗口已经满足条件:
        更新最短答案
        移出 s[left]
        left += 1

对应题：

| 题目                                            | 为什么用滑动窗口            |
| --------------------------------------------- | ------------------- |
| 76. Minimum Window Substring                  | 找包含 t 所有字符的最短子串     |
| 209. Minimum Size Subarray Sum                | 找和至少为 target 的最短子数组 |
| 30. Substring with Concatenation of All Words | 找包含所有 words 的连续拼接窗口 |

例子：`76. Minimum Window Substring`

要求：s 中包含 t 所有字符的最短子串

窗口维护：
- 每个字符出现次数
- 当前满足了多少个需要的字符
- 当窗口已经覆盖 t，就尽量缩小左边

本质：先扩大到合法，再尽量缩小

## 三、固定长度窗口类

这类最简单

特点是：窗口长度固定为 k，右指针每走一步，左边也同步移出

对应题：

| 题目                                 | 为什么用固定窗口          |
| ---------------------------------- | ----------------- |
| 1456. Maximum Number of Vowels     | 长度为 k 的子串中最多元音    |
| 187. Repeated DNA Sequences        | 长度固定为 10 的 DNA 子串 |
| 438. Find All Anagrams in a String | 窗口长度固定为 len(p)    |
| 567. Permutation in String         | 窗口长度固定为 len(s1)   |

例子：`438. Find All Anagrams in a String`

要求：找 s 中所有 p 的异位词

异位词长度一定等于 len(p)，所以窗口长度固定

窗口维护：当前窗口字符频率

如果频率和 p 一样，就找到答案

## 四、计数类窗口：At Most / Exactly K

这类题经常问：有多少个子数组满足某条件

对应题：
| 题目                                                          | 核心                     |
| ----------------------------------------------------------- | ---------------------- |
| 992. Subarrays with K Different Integers                    | 恰好 K 种不同整数             |
| 930. Binary Subarrays With Sum                              | 和恰好等于 goal             |
| 713. Subarray Product Less Than K                           | 乘积小于 k 的子数组个数          |
| 395. Longest Substring with At Least K Repeating Characters | 特殊，可以用分治，也可以枚举种类数 + 滑窗 |

关键技巧：`Exactly K 转 At Most K`

很多“恰好 K 个”的题不好直接做

可以转成：`恰好 K 个 = 最多 K 个 - 最多 K - 1 个`

比如：`992. Subarrays with K Different Integers`

要求：恰好有 K 个不同整数的子数组数量

可以变成：`atMost(K) - atMost(K - 1)`

因为：
- 最多 K 个里面包含：1 种、2 种、...、K 种
- 最多 K-1 个里面包含：1 种、2 种、...、K-1 种

两者相减，剩下的就是恰好 K 种

`713. Subarray Product Less Than K`

窗口维护：当前窗口乘积 product

如果：`product >= k` 就移动左指针缩小窗口。

每次右指针固定时，以 right 结尾的合法子数组数量是：right - left + 1

因为窗口内任意起点到 right 都合法

## 五、频率统计类窗口

这类题通常处理字符串

关键词：
- permutation
- anagram
- contains
- frequency
- character count

对应题：

| 题目                                            | 窗口维护什么   |
| --------------------------------------------- | -------- |
| 438. Find All Anagrams in a String            | 字符频率     |
| 567. Permutation in String                    | 字符频率     |
| 76. Minimum Window Substring                  | 目标字符覆盖情况 |
| 424. Longest Repeating Character Replacement  | 窗口内最高频字符 |
| 30. Substring with Concatenation of All Words | 单词频率     |

## 六、需要额外数据结构的滑窗

有些窗口不仅要维护 sum / count，还要维护最大值、最小值

对应题：

| 题目                                            | 额外结构            |
| --------------------------------------------- | --------------- |
| 1438. Longest Continuous Subarray             | 单调队列维护最大值和最小值   |
| 1838. Frequency of the Most Frequent Element  | 排序 + 前缀思想 / 窗口和 |
| 30. Substring with Concatenation of All Words | HashMap 统计单词频率  |

`1438 为什么是 Monotonic Queue + Sliding Window？`

要求：窗口内 max - min <= limit

所以每次要快速知道窗口最大值和最小值

普通数组每次找 max/min 太慢

所以用两个单调队列：
- maxDeque 维护窗口最大值
- minDeque 维护窗口最小值

一旦：max - min > limit

就移动左指针缩小窗口

## 面试时怎么快速判断？

可以按这个顺序想：
- Step 1：是不是连续？
  - 如果题目说：subarray / substring / consecutive -> 优先考虑滑动窗口
- Step 2：窗口是否合法能不能快速判断？
  - 比如：
    - 窗口内 0 的数量 <= k
    - 窗口内不同元素数量 <= k
    - 窗口内字符频率覆盖 target
    - 窗口内 product < k
    - 窗口内 max - min <= limit
  - 如果能维护状态，就适合滑窗
- Step 3：求最长还是最短？
  - 最长: 不合法时缩小，合法时更新答案
  - 最短: 合法时更新答案，然后继续缩小
  - 个数: 每次加 right - left + 1
  - 固定长度: 长度超过 k 就移动 left

## 最核心记忆

`Sliding Window = 连续区间 + 动态维护状态`

- 最长窗口：不合法就缩小
- 最短窗口：合法就缩小
- 固定窗口：长度固定移动
- 计数窗口：right 固定后统计 left 到 right 的合法数量
- Exactly K：atMost(K) - atMost(K-1)

面试里可以这样说：

I use sliding window because we are dealing with a contiguous subarray or substring, and the validity of the current range can be updated incrementally when we move the left and right pointers.
