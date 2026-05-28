# 规律

核心规律：`每一步都做当前看起来最优的选择，且这个局部最优不会破坏全局最优`

通俗说：`贪心题不是枚举所有可能，而是找到一个“永远不会吃亏”的选择规则`

## 什么时候想到 Greedy？

看到这些关键词，可以优先想贪心：

| 题目特征        | 常见贪心思路         |
| ----------- | -------------- |
| 最少操作次数      | 每一步尽量修复最多问题    |
| 最大收益 / 最大数量 | 优先选当前收益最高或代价最低 |
| 区间不重叠 / 合并  | 按结束时间、起点排序     |
| 跳跃 / 覆盖范围   | 维护当前能到的最远位置    |
| 字典序最小 / 最大  | 尽量让前面的字符更优     |
| 分配资源        | 优先满足最紧急 / 最小需求 |
| 排序后做选择      | 排序 + 贪心通常一起出现  |

## 区间类贪心

对应题：
| 题目                             | 为什么用贪心            |
| ------------------------------ | ----------------- |
| 56. Merge Intervals            | 按 start 排序，能合并就合并 |
| 435. Non-overlapping Intervals | 优先保留结束早的区间        |
| 763. Partition Labels          | 每段必须覆盖所有字符最后出现位置  |
| 1029. Two City Scheduling      | 按去 A 和去 B 的成本差排序  |

`435. Non-overlapping Intervals`

目标：删除最少区间，使剩下区间不重叠

贪心规则：`每次保留结束时间最早的区间`

为什么？
- 结束越早，后面留给其他区间的空间越大

所以这类题常见套路是：
- 按 end 排序
- 能选就选
- 冲突就跳过

## 跳跃 / 覆盖范围类

对应题：

| 题目               | 贪心点                 |
| ---------------- | ------------------- |
| 55. Jump Game    | 不断维护当前能到的最远位置       |
| 45. Jump Game II | 当前这一跳范围内，选择能跳得最远的位置 |
| 1326 / 覆盖类问题     | 用最少区间覆盖目标范围         |

`55. Jump Game`

目标：判断能不能到最后

贪心维护：farthest = 当前能到的最远位置

遍历时，如果当前位置超过 farthest，说明到不了

否则更新：farthest = max(farthest, i + nums[i])

本质：只关心目前能覆盖到多远

`45. Jump Game II`

目标：最少跳几步到最后

贪心规则：在当前这一跳能到的范围内，找下一跳能到最远的位置

类似坐公交：当前这班车能到一段范围，在这段范围里选下一班能开最远的车

## 字符串 / 字典序类贪心

对应题：

| 题目                                                       | 为什么用贪心                        |
| -------------------------------------------------------- | ----------------------------- |
| 12. Integer to Roman                                     | 从大到小尽量匹配最大符号                  |
| 767. Reorganize String                                   | 每次优先放剩余次数最多的字符                |
| 2384. Largest Palindromic Number                         | 大数字尽量放两边，小心中心位                |
| 3752. Lexicographically Smallest String After Operations | 尽量让前面字符更小                     |
| 556. Next Greater Element III                            | 找下一个更大的排列，类似 next permutation |

`556. Next Greater Element III`

本质是：找比当前数大一点点的下一个排列

贪心规则：
- 从右往左找第一个下降点
- 在右边找一个刚好比它大的数交换
- 交换后右边变成最小排列

为什么？
- 要让结果变大，但又尽量小，所以变化越靠右越好

## 括号类贪心

对应题：

| 题目                                         | 贪心点                 |
| ------------------------------------------ | ------------------- |
| 921. Minimum Add to Make Parentheses Valid | 遇到无法匹配的右括号就补左括号     |
| 678. Valid Parenthesis String              | 用范围维护可能的左括号数量       |
| 32. Longest Valid Parentheses              | 可以用栈 / DP，也可以左右扫描贪心 |

`921. Minimum Add to Make Parentheses Valid`

规则：

遇到 '('：需要一个右括号匹配
遇到 ')'：
    如果有未匹配 '('，就抵消
    否则必须补一个 '('

本质：每次遇到非法右括号，立刻补一个左括号，这是不会吃亏的

`678. Valid Parenthesis String`

* 可以当：左括号 / 右括号 / 空

所以不固定某一种，而是维护范围：
- 可能的未匹配左括号最小值
- 可能的未匹配左括号最大值

本质是贪心地保留所有可能性

## 股票 / 子数组类贪心

对应题：

| 题目                                      | 为什么用贪心         |
| --------------------------------------- | -------------- |
| 122. Best Time to Buy and Sell Stock II | 所有上涨段都可以赚      |
| 53. Maximum Subarray                    | 当前子数组变差就重新开始   |
| 2483. Minimum Penalty for a Shop        | 枚举关门时间，用前缀统计代价 |

`122. Best Time to Buy and Sell Stock II`

只要明天比今天贵：

price[i+1] > price[i]

就把这个差价赚了。

因为多次交易允许，所以：

总收益 = 所有上涨差价之和

本质：

每一段上涨都不会亏，全部收下。

`53. Maximum Subarray`

贪心思想：

如果当前和已经小于 0，就不要带着它继续走

因为负数前缀只会拖累后面的子数组。

所以：current = max(nums[i], current + nums[i])

## 排序 + 贪心类

很多贪心题都要先排序

对应题：

| 题目                                            | 排序目的                  |
| --------------------------------------------- | --------------------- |
| 1665. Minimum Initial Energy to Finish Tasks  | 按 actual - minimum 排序 |
| 945. Minimum Increment to Make Array Unique   | 排序后让每个数至少比前一个大 1      |
| 3075. Maximize Happiness of Selected Children | 先选幸福值高的               |
| 2593. Find Score of an Array After Marking    | 按值从小到大选               |
| 853. Car Fleet                                | 按位置排序，看是否追上前车         |
| 1727. Largest Submatrix With Rearrangements   | 每行高度排序后计算面积           |
| 1648. Sell Diminishing-Valued Colored Balls   | 优先卖价值高的球              |

`为什么排序常配合贪心？`

因为排序后可以让你明确：
- 先处理最小的
- 先处理最大的
- 先处理最早结束的
- 先处理代价差最大的

排序的作用是：给贪心选择一个稳定顺序

## 任务调度 / 分配类

对应题：

| 题目                                 | 贪心点              |
| ---------------------------------- | ---------------- |
| 621. Task Scheduler                | 高频任务决定最少空闲时间     |
| 135. Candy                         | 左右两遍，保证局部相邻关系    |
| 1953. Maximum Number of Weeks      | 最大项目不能超过其他项目总和太多 |
| 1665. Minimum Initial Energy       | 先做更“苛刻”的任务       |
| 1564. Put Boxes Into the Warehouse | 先放小箱子 / 预处理仓库高度  |

`135. Candy`

要求：评分更高的孩子糖更多

只看一边不够，所以做两次贪心：
- 从左到右：保证右边比左边高时糖更多
- 从右到左：保证左边比右边高时糖更多

本质：局部相邻约束，用两次扫描分别满足

## “最少操作”类贪心

对应题：

| 题目                                              | 贪心点            |
| ----------------------------------------------- | -------------- |
| 945. Minimum Increment to Make Array Unique     | 每个数尽量变成当前最小可用值 |
| 1536. Minimum Swaps to Arrange a Binary Grid    | 每次找最近可用行换上来    |
| 2033. Minimum Operations to Make Uni-Value Grid | 统一到中位数最优       |
| 2333. Minimum Sum of Squared Difference         | 优先减少最大的差值      |
| 3785. Minimum Swaps to Avoid Forbidden Values   | 优先解决冲突最多的值     |

`945. Minimum Increment to Make Array Unique`

排序后：当前数必须 > 前一个数

如果当前数太小，就提升到：prev + 1

这是最小增量，所以最优

## 面试快速判断口诀

看到这些，优先想 `Greedy`：
- 每一步都可以做一个明显最优选择
- 题目问最少操作 / 最大数量 / 最大收益
- 区间要选最多不重叠
- 跳跃要覆盖最远
- 字符串要字典序最小/最大
- 任务要最早结束 / 最高频 / 最小代价
- 排序后选择会变清楚

**最核心记忆**
- Greedy = 找一个局部最优规则，并证明它不会影响全局最优

面试里可以这样说：

I use greedy because at each step, there is a choice that is always safe: it either leaves more room for future choices, reduces the largest current problem, or gives the best immediate gain without hurting later decisions.