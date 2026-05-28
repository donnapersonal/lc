# 规律

核心规律：当题目需要反复拿到`“当前最大 / 当前最小 / Top K / 最优候选”`时，就很可能用 `Heap`

`Heap` 的作用不是排序全部数据，而是：`随时快速拿到当前最重要的那个元素`

## 1. Top K / 第 K 大类

典型题：

| 题目                                   | 为什么用 Heap         |
| ------------------------------------ | ----------------- |
| 215. Kth Largest Element in an Array | 维护前 k 大元素         |
| 692. Top K Frequent Words            | 先统计频率，再取频率最高的 k 个 |

核心规律

如果题目问：
- 第 K 大
- 第 K 小
- 前 K 个
- Top K frequent
- K pairs smallest / largest

优先想到：`Heap / Priority Queue`

`215. Kth Largest Element`

目标是找第 `k` 大

可以用小根堆维护 `k` 个最大值：
- 堆里始终放当前最大的 k 个数
- 堆顶就是这 k 个数里最小的
- 最后堆顶就是第 k 大

为什么不用排序？
- 排序是 `O(n log n)`
- `Heap` 可以做到 `O(n log k)`

当 `k` 比 `n` 小很多时更划算

## 2. 数据流类

典型题：
| 题目                                | 为什么用 Heap       |
| --------------------------------- | --------------- |
| 295. Find Median from Data Stream | 数据不断进来，要随时返回中位数 |

核心规律

如果题目说：stream/online/dynamic/不断加入数据/随时查询最大/最小/中位数

就很适合 `Heap`

`295. Find Median from Data Stream`

用两个堆：
- 左边：大根堆，保存较小的一半
- 右边：小根堆，保存较大的一半

中位数就是：
- 两个堆大小相等：取两个堆顶平均
- 一个堆更大：取那个堆顶

本质是：`Heap` 帮我们动态维护中间位置

## 3. 区间 / 会议室 / 事件调度类

典型题：

| 题目                                                  | 为什么用 Heap         |
| --------------------------------------------------- | ----------------- |
| 2402. Meeting Rooms III                             | 每次要找最早空出来的会议室     |
| 1353. Maximum Number of Events That Can Be Attended | 每天参加截止时间最早的 event |

`2402. Meeting Rooms III`

会议室问题通常要维护：
- 当前哪些会议室正在使用
- 它们什么时候结束

所以用小根堆：
- 按结束时间排序
- 每次拿最早结束的会议室

如果还要选编号最小的空房间，也可以再用一个堆维护空房间编号

`1353. Maximum Number of Events`

每天可以参加一个 `event`

贪心策略：每天选择结束时间最早的 event

为什么？
- 越早结束的 event 越容易过期，应该优先参加

所以用小根堆存 `event` 的 `end day`

## 4. 多路合并 / 多候选扩展类

典型题：
| 题目                                   | 为什么用 Heap       |
| ------------------------------------ | --------------- |
| 373. Find K Pairs with Smallest Sums | 每次取当前和最小的一对     |
| 355. Design Twitter                  | 合并多个用户的 tweet 流 |
| 692. Top K Frequent Words            | 多个候选中按优先级取前 k   |

`373. Find K Pairs with Smallest Sums`

两个有序数组，要找 k 个最小 pair sum

不能暴力枚举所有 pair

Heap 维护：
- 当前可能最小的 pair
- 每次弹出最小 pair
- 再加入下一个候选 pair

本质是：不生成全部组合，只扩展当前最有希望的候选

`355. Design Twitter`

News Feed 要取关注用户最近的 tweet

每个用户自己的 tweet 列表是按时间排好的

问题变成：
- 从多个有序列表中取最新的 10 条

这就是多路合并

所以用 `heap` 按时间戳取最新 `tweet`

## 5. 贪心 + Heap 类

典型题：

| 题目                                       | 为什么用 Heap          |
| ---------------------------------------- | ------------------ |
| 1642. Furthest Building You Can Reach    | 梯子应该留给最大高度差        |
| 3510. Minimum Pair Removal to Sort Array | 每次选择当前最小 pair 进行操作 |
| 855. Exam Room                           | 每次选择距离最近人的最远位置     |
| 3408. Design Task Manager                | 每次执行优先级最高的任务       |

`1642. Furthest Building You Can Reach`

爬楼需要消耗：bricks 或 ladders

贪心思想：
- 梯子应该用在最大的高度差上
- 砖头用在较小高度差上

所以用小根堆记录目前用梯子的高度差

当梯子数量超过限制，就把最小的高度差拿出来改用砖头

本质：Heap 帮我们动态决定哪些跳跃最值得用梯子

`3408. Design Task Manager`

任务有优先级

每次执行要拿：优先级最高的任务

所以用最大堆

如果任务会被修改 / 删除，通常还要用 lazy deletion：
- 堆里旧数据不立刻删
- 弹出时发现过期再跳过
  
`855. Exam Room`

每次学生入座，要选：离最近的人最远的位置

这其实是在所有空区间中选“最优区间”

所以用 `heap` 存区间优先级：
- 距离越大越优先
- 距离相同，座位编号越小越优先

本质：每次从所有候选区间里取最优一个`

## 面试快速判断口诀

看到这些，优先想 `Heap`：
- top k
- kth largest / kth smallest
- median from stream
- merge k sorted lists
- minimum cost
- maximum profit
- schedule events / meeting rooms
- 每次取当前最小/最大
- 动态加入 + 动态查询最值

## Heap 和 Sorting 怎么选？

| 方法            | 适合场景                   |
| ------------- | ---------------------- |
| Sorting       | 数据一次性处理，全部排序也可以        |
| Heap          | 只关心 Top K / 动态最值 / 数据流 |
| Quickselect   | 只找第 K 大，不需要动态维护        |
| Balanced Tree | 需要删除任意元素 + 排序查询        |

简单判断：
- 只查一次第 K 大 → `Quickselect / Heap`
- 要不断插入并查询最大最小 → `Heap`
- 要完整有序结果 → `Sorting`

## 最核心记忆

- Heap = 动态维护当前最大/最小候选
- Top K → Heap
- 数据流 → Heap
- 调度最早结束 / 最高优先级 → Heap
- 多路合并 → Heap
- 贪心里需要反复选当前最优 → Heap

面试里可以这样说：

I use a heap because at each step, I only need the current best candidate, not a fully sorted list. The heap lets me retrieve and update that candidate efficiently.