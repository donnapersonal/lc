# 规律

核心规律：`当一个大问题可以拆成很多重复的小问题，并且当前答案依赖之前/子问题的答案，就很可能用 DP`

通俗说：`DP = 先算小问题，再用小问题拼出大问题`

## 什么时候想到 DP？

看到这些关键词，优先想 DP：

| 题目特征       | 为什么像 DP               |
| ---------- | --------------------- |
| 求最大 / 最小值  | 当前最优依赖之前最优            |
| 求方案数       | 当前方案数由前面状态累加          |
| 可以选 / 不选   | 状态转移                  |
| 字符串匹配      | `s[:i]` 和 `t[:j]` 的关系 |
| 背包 / 子集和   | 容量、目标和、选择物品           |
| 路径问题       | 当前格子依赖上方/左方           |
| 股票买卖       | 当前持有/不持有状态            |
| 区间合并 / 戳气球 | 枚举最后一步                |

## 线性 DP：当前依赖前面

典型题：

| 题目                            | 为什么用 DP               |
| ----------------------------- | --------------------- |
| 198. House Robber             | 偷当前房子就不能偷前一个          |
| 213. House Robber II          | 环形房子，拆成两次线性 DP        |
| 91. Decode Ways               | 当前字符能单独解码，也可能和前一个一起解码 |
| 983. Minimum Cost For Tickets | 当前天的最低花费依赖之前天         |
| 413. Arithmetic Slices        | 当前等差子数组依赖前一个长度        |
| 53. Maximum Subarray          | 当前最大子数组和依赖前一个状态       |
| 152. Maximum Product Subarray | 需要维护当前最大积和最小积         |

`198. House Robber`

每个房子有两个选择：
- 偷当前房子：dp[i-2] + nums[i]
- 不偷当前房子：dp[i-1]

所以：`dp[i] = max(dp[i-1], dp[i-2] + nums[i])`

本质：当前最优来自“偷”或“不偷”的选择

`152. Maximum Product Subarray`

为什么比最大子数组和复杂？

因为负数会反转：
- 最大值 * 负数 可能变最小
- 最小值 * 负数 可能变最大

所以要同时维护：
- 当前最大乘积
- 当前最小乘积

## 路径 DP：当前格子依赖邻居

典型题:
| 题目                                 | 为什么用 DP            |
| ---------------------------------- | ------------------ |
| 64. Minimum Path Sum               | 当前格子只能从上或左来        |
| 63. Unique Paths II                | 有障碍物，路径数依赖上方和左方    |
| 120. Triangle                      | 当前点依赖上一层相邻位置       |
| 931. Minimum Falling Path Sum      | 当前点依赖上一行三个方向       |
| 174. Dungeon Game                  | 反向 DP，当前位置需要保证后面能活 |
| 1594. Maximum Non Negative Product | 正负数影响，需要最大/最小乘积    |

`64. Minimum Path Sum`

当前位置 (i, j) 的最小路径和来自：

上方：dp[i-1][j]
左方：dp[i][j-1]

所以：dp[i][j] = grid[i][j] + min(上方, 左方)

本质：到当前格子的最优路径，只可能来自已经算过的邻居

## 字符串 DP：比较两个前缀

典型题：

| 题目                               | 状态含义                                |
| -------------------------------- | ----------------------------------- |
| 72. Edit Distance                | `word1[:i]` 到 `word2[:j]` 的最少操作     |
| 1143. Longest Common Subsequence | 两个前缀的最长公共子序列                        |
| 97. Interleaving String          | `s1[:i]` 和 `s2[:j]` 能否组成 `s3[:i+j]` |
| 10. Regular Expression Matching  | `s[:i]` 是否匹配 `p[:j]`                |
| 139. Word Break                  | `s[:i]` 能否被拆分                       |
| 140. Word Break II               | 返回所有拆分结果，DFS + memo                 |

`72. Edit Distance`

每次看最后一个字符。

如果相等：dp[i][j] = dp[i-1][j-1]

如果不等，有三种操作：

删除：dp[i-1][j]
插入：dp[i][j-1]
替换：dp[i-1][j-1]

所以：dp[i][j] = 1 + min(删除, 插入, 替换)

本质：两个字符串问题，常常拆成两个前缀之间的问题

`1143. Longest Common Subsequence`

如果两个字符相等：dp[i][j] = dp[i-1][j-1] + 1

如果不等：dp[i][j] = max(dp[i-1][j], dp[i][j-1])

本质：当前字符要么匹配，要么跳过其中一个

## 背包 DP：选或不选

典型题：

| 题目                                    | 为什么是背包              |
| ------------------------------------- | ------------------- |
| 416. Partition Equal Subset Sum       | 是否能选出和为 total/2 的子集 |
| 698. Partition to K Equal Sum Subsets | 分成 k 组，每组和相等        |
| 322. Coin Change                      | 凑出金额的最少硬币数          |
| 518. Coin Change II                   | 凑出金额的方案数            |
| 279. Perfect Squares                  | 用平方数凑 n 的最少数量       |
| 1262. Greatest Sum Divisible by Three | 维护余数状态              |

`416. Partition Equal Subset Sum`

目标是：能不能选一些数，使它们的和 = total / 2

这就是典型 0/1 背包：

每个数只能选一次
容量是 target

状态可以是：dp[s] = 是否能凑出和 s

`322 vs 518 Coin Change`

这两个很容易混

| 题目                  | 问什么   | DP 目标   |
| ------------------- | ----- | ------- |
| 322. Coin Change    | 最少硬币数 | minimum |
| 518. Coin Change II | 方案数   | count   |

`322 -> dp[amount] = 凑出 amount 的最少硬币数`

`518 -> dp[amount] = 凑出 amount 的组合数量
`

本质区别： 一个求最优值，一个求方案数

## 股票 DP：持有 / 不持有状态

典型题：

| 题目                                      | 为什么用 DP          |
| --------------------------------------- | ---------------- |
| 122. Best Time to Buy and Sell Stock II | 每天有持有/不持有状态，也可贪心 |
| 309 / 714 / 123 / 188 类似题               | 加冷冻期、手续费、次数限制    |

虽然截图里主要是 122，但股票类通用思路是：
- hold = 当前持有股票的最大收益
- cash = 当前不持有股票的最大收益

每天更新：
- hold = max(继续持有, 今天买入)
- cash = max(继续不持有, 今天卖出)

## LIS 类 DP

典型题：

| 题目                                  | 为什么              |
| ----------------------------------- | ---------------- |
| 300. Longest Increasing Subsequence | 当前数可以接在前面比它小的数后面 |
| 354. Russian Doll Envelopes         | 排序后转成 LIS        |

`300. LIS`

朴素 DP：dp[i] = 以 nums[i] 结尾的最长递增子序列长度

转移：

如果 nums[j] < nums[i]:
dp[i] = max(dp[i], dp[j] + 1)

优化版用二分：

tails[len] = 长度为 len 的递增子序列的最小结尾

`354. Russian Doll Envelopes`

先排序：宽度升序, 宽度相同时，高度降序, 然后对高度做 LIS。

为什么高度要降序？

防止宽度相同的信封被错误地套进去

## 区间 DP：枚举最后一步 / 分割点

典型题：

| 题目                             | 为什么是区间 DP    |
| ------------------------------ | ------------ |
| 312. Burst Balloons            | 枚举区间里最后戳哪个气球 |
| 96. Unique Binary Search Trees | 枚举哪个数字作为根    |
| 1039 / 矩阵链类                    | 枚举中间分割点      |

`312. Burst Balloons`

难点是：

如果先戳某个气球，左右邻居会变化。

所以反过来想：

枚举最后戳哪个气球

如果 k 是区间 (l, r) 里最后戳的气球，那么它的左右邻居固定是 l 和 r。

转移：

dp[l][r] = max(dp[l][k] + dp[k][r] + nums[l]*nums[k]*nums[r])

本质：

区间 DP 常常通过“最后一步”让问题变简单

## 树形 DP

典型题：

| 题目                    | 为什么用树形 DP     |
| --------------------- | ------------- |
| 337. House Robber III | 每个节点有偷/不偷两种状态 |

`337. House Robber III`

对每个节点返回两个值：

rob = 偷当前节点时的最大收益
not_rob = 不偷当前节点时的最大收益

如果偷当前节点：

rob = node.val + left.not_rob + right.not_rob

如果不偷当前节点：

not_rob = max(left.rob, left.not_rob) + max(right.rob, right.not_rob)

本质：当前节点的最优选择依赖左右子树状态

## 状态机 DP

典型题：

| 题目                                           | 状态           |
| -------------------------------------------- | ------------ |
| 790. Domino and Tromino Tiling               | 不同覆盖形状状态     |
| 1458. Max Dot Product of Two Subsequences    | 选/不选两个元素     |
| 3418. Maximum Amount of Money Robot Can Earn | 位置 + 剩余能力/次数 |
| 1043. Partition Array for Maximum Sum        | 枚举最后一段长度     |

状态机 DP 的特点是：
- 当前状态不只是 i
- 还要记录额外条件

比如：
- 是否持有股票
- 用了几次操作
- 当前余数是多少
- 剩余几次机会
- 当前形状是否缺一块

## 面试快速判断口诀

看到这些，优先想 DP：
- 当前答案依赖之前答案
- 有重复子问题
- 要求最大/最小/方案数
- 每一步有选或不选
- 字符串前缀匹配
- 路径只能从某几个方向来
- 子集和 / 背包 / 凑金额
- 区间里枚举最后一步
- 树上每个节点有多个状态

**最核心记忆**
- DP = 状态定义 + 状态转移 + 初始化 + 遍历顺序

面试里可以这样说：

I use dynamic programming because the problem has overlapping subproblems, and the optimal answer for the current state can be built from smaller previously solved states.