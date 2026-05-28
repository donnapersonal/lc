# 规律

对于 `Graph / Tree / DFS / BFS / Union-Find / Topological Sort / Shortest Path`

核心判断：
- 有“点和点之间的关系” → `Graph`
- 要一直往深处探索 → `DFS`
- 要按层一圈圈扩散 → `BFS`
- 要判断是否属于同一组 → `Union-Find`
- 有先后依赖关系 → `Topological Sort`
- 有最短路径 / 最小代价 → `BFS / Dijkstra / Bellman-Ford / Floyd`

## 1. DFS：适合“走到底 / 找所有 / 递归结构”

`DFS` 的核心是：从一个点出发，一条路走到底，再回头试其他路

常见关键词：

| 关键词                     | 想到 DFS    |
| ----------------------- | --------- |
| island / connected area | 岛屿、连通块    |
| path / all paths        | 找路径、找所有路径 |
| tree recursion          | 树的递归处理    |
| backtracking            | 回溯搜索      |
| clone / copy            | 复制图       |
| detect cycle            | 检测环       |

**为什么这些题用 DFS？**

| 题目                                   | 为什么用 DFS             |
| ------------------------------------ | -------------------- |
| 200. Number of Islands               | 从一个陆地出发，把整座岛全部染色     |
| 695. Max Area of Island              | 从一个陆地出发，统计整块岛面积      |
| 130. Surrounded Regions              | 从边界出发，标记不会被包围的区域     |
| 417. Pacific Atlantic Water Flow     | 从海洋边界反向 DFS，找能流到海洋的点 |
| 133. Clone Graph                     | 从一个节点开始，递归复制所有邻居     |
| 797. All Paths From Source to Target | 要找所有路径，天然回溯          |
| 2101. Detonate the Maximum Bombs     | 炸弹能引爆其他炸弹，本质是图的可达性   |
| 1192. Critical Connections           | 找桥，需要 DFS + low-link |

通俗理解:

`DFS` 像走迷宫：
- 先沿着一条路一直走
- 走不通再退回来
- 换下一条路

所以适合：
- 找连通区域
- 找所有路径
- 递归处理树/图

## 2. BFS：适合“最少步数 / 按层扩散”

`BFS` 的核心是：一层一层扩散，第一次到达就是最短步数

常见关键词：
| 关键词                      | 想到 BFS   |
| ------------------------ | -------- |
| shortest path            | 最短路径     |
| minimum steps            | 最少步数     |
| level order              | 层序遍历     |
| spread / rot / infection | 腐烂、传播、扩散 |
| word ladder / lock       | 每一步变化一次  |

**为什么这些题用 BFS？**

| 题目                                      | 为什么用 BFS              |
| --------------------------------------- | --------------------- |
| 994. Rotting Oranges                    | 腐烂按分钟一层层扩散            |
| 127. Word Ladder                        | 每次改一个字母，求最短转换次数       |
| 126. Word Ladder II                     | 先 BFS 找最短层，再 DFS 回溯路径 |
| 752. Open the Lock                      | 每次转一位，求最少步数           |
| 1091. Shortest Path in Binary Matrix    | 网格里求最短路径              |
| 815. Bus Routes                         | 每坐一辆公交是一层，求最少换乘       |
| 1162. As Far from Land as Possible      | 多源 BFS，从所有陆地同时扩散      |
| 102 / 107 / 103 Binary Tree Level Order | 树的层序遍历                |
| 116. Populating Next Right Pointers     | 按层连接 next 指针          |
| 863. All Nodes Distance K               | 从 target 开始扩散 k 层     |

通俗理解:

BFS 像水波纹：
- 第 0 层：起点
- 第 1 层：一步能到的点
- 第 2 层：两步能到的点
...

所以只要问：
- 最少几步？
- 第几层？
- 多久传播完？

优先 `BFS`

## 3. Binary Tree：大多数是 DFS / BFS

树题本质上也是`图`，只是`没有环`

**DFS 适合什么树题？**
- 需要从根到叶、子树信息、递归返回值

| 题目                                            | 为什么 DFS             |
| --------------------------------------------- | ------------------- |
| 98. Validate BST                              | 每个节点要满足上下界          |
| 236. LCA                                      | 需要从左右子树返回信息         |
| 113. Path Sum II                              | 找所有 root-to-leaf 路径 |
| 129. Sum Root to Leaf Numbers                 | 路径上累积数字             |
| 979. Distribute Coins                         | 每个子树向父节点返回盈亏        |
| 1339. Maximum Product of Splitted Binary Tree | 先求每个子树和             |
| 1448. Count Good Nodes                        | DFS 时维护路径最大值        |
| 1110. Delete Nodes and Return Forest          | 递归处理删除后森林           |
| 1382. Balance BST                             | 中序 DFS 得到有序数组，再重建   |

树 DFS 的一句话: `当前节点的答案依赖左右子树 → DFS`

**BFS 适合什么树题？**
- 需要按层处理

| 题目                                     | 为什么 BFS  |
| -------------------------------------- | -------- |
| 102. Level Order Traversal             | 层序遍历     |
| 107. Level Order II                    | 层序结果反转   |
| 103. Zigzag Level Order                | 每层方向不同   |
| 1161. Maximum Level Sum                | 每层求和     |
| 199. Right Side View                   | 每层最后一个节点 |
| 662. Maximum Width of Binary Tree      | 每层计算宽度   |
| 2583. Kth Largest Sum in a Binary Tree | 每层求和后排序  |

树 BFS 的一句话: `题目问“每一层” → BFS`

## 4. Union-Find：适合“分组 / 连通性 / 合并”

`Union-Find` 的核心是：`快速判断两个元素是不是在同一个集合里`

常见关键词：

| 关键词                  | 想到 Union-Find |
| -------------------- | ------------- |
| connected components | 连通分量          |
| merge accounts       | 合并账号          |
| same group           | 是否同组          |
| redundant connection | 多余边           |
| provinces            | 省份数量          |
| equations equality   | 等式关系          |

为什么这些题用 Union-Find？

| 题目                                                   | 为什么用 Union-Find           |
| ---------------------------------------------------- | ------------------------- |
| 547. Number of Provinces                             | 城市之间有连接，要数连通块             |
| 721. Accounts Merge                                  | 邮箱相同的账号要合并                |
| 947. Most Stones Removed                             | 同行同列的石头属于同一组              |
| 684. Redundant Connection                            | 加边时如果两点已连通，就形成环           |
| 990. Satisfiability of Equality Equations            | `a==b` 合并，`a!=b` 检查冲突     |
| 1319. Number of Operations to Make Network Connected | 电脑连通分量数量                  |
| 1584. Min Cost to Connect All Points                 | Kruskal 最小生成树用 Union-Find |

通俗理解:

Union-Find 就像班级分组：
- union(a, b)：把 a 和 b 放到同一组
- find(a)：查 a 属于哪个组

如果题目重点是：
- 谁和谁连在一起？
- 一共有几个组？
- 合并后有没有冲突？

就优先想 `Union-Find`

## 5. Topological Sort：适合“依赖关系 / 先后顺序”

拓扑排序的核心是：`先做没有依赖的，再做依赖它的`

常见关键词：

| 关键词              | 想到 Topological Sort |
| ---------------- | ------------------- |
| prerequisite     | 先修课                 |
| dependency       | 依赖                  |
| ordering         | 顺序                  |
| alien dictionary | 字母顺序                |
| can finish       | 能否完成                |
| eventual safe    | 最终安全状态              |

为什么这些题用拓扑排序？

| 题目                             | 为什么用拓扑排序               |
| ------------------------------ | ---------------------- |
| 207. Course Schedule           | 判断课程依赖图有没有环            |
| 210. Course Schedule II        | 输出一种合法课程顺序             |
| 269. Alien Dictionary          | 根据单词顺序推导字母先后关系         |
| 802. Find Eventual Safe States | 反向图 + 出度为 0 的点逐层删除     |
| 63. Unique Paths II            | 网格 DP 也可以看成有向无环图上的状态转移 |

通俗理解

如果题目说：
- A 必须在 B 之前
- B 必须在 C 之前

那就是依赖图

如果依赖图有环：
- A 依赖 B
- B 依赖 A

就无法完成

## 6. Shortest Path：最短路径 / 最小代价

这类题的核心问题是：`从一个点到另一个点，代价最小是多少？`

不同算法看边权

**什么时候用 BFS？**
- 所有边权都一样
- 每一步代价相同

例子：
| 题目                                   | 为什么 BFS      |
| ------------------------------------ | ------------ |
| 127. Word Ladder                     | 每改一个字母代价都是 1 |
| 752. Open the Lock                   | 每转一次代价都是 1   |
| 1091. Shortest Path in Binary Matrix | 每走一步代价都是 1   |
| 815. Bus Routes                      | 每坐一辆 bus 算一层 |

**什么时候用 Dijkstra？**

边权是`非负数`，而且不同边代价不同

例子：
| 题目                                          | 为什么 Dijkstra |
| ------------------------------------------- | ------------ |
| 743. Network Delay Time                     | 信号传播时间不同     |
| 3650. Minimum Cost Path with Edge Reversals | 不同方向代价不同     |
| 部分加权图最短路                                    | 需要最小总代价      |

**什么时候用 Bellman-Ford？**

`最多经过 K 条边 / K 次中转`

例子：
| 题目                                   | 为什么 Bellman-Ford           |
| ------------------------------------ | -------------------------- |
| 787. Cheapest Flights Within K Stops | 限制最多 K 次中转，不能只用普通 Dijkstra |

`Bellman-Ford `的重点是：
- 做 K+1 轮松弛
- 每轮代表多用一条边

## 什么时候用 Floyd？

`任意两点之间的最短路径，点数不大`

例子：
| 题目                                     | 为什么 Floyd                 |
| -------------------------------------- | ------------------------- |
| 2976. Minimum Cost to Convert String I | 字符到字符之间可能多次转换，要求任意字符对最小成本 |

`Floyd` 的核心：`dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`

## 7. Backtracking / Memoization：适合“枚举所有可能”

这类题通常不是单纯图遍历，而是：`每一步有多个选择，要尝试所有组合`

| 题目                                | 为什么                   |
| --------------------------------- | --------------------- |
| 140. Word Break II                | 要返回所有拆分方案             |
| 212. Word Search II               | Trie + DFS 搜索所有单词     |
| 756. Pyramid Transition Matrix    | 每一层有多种可能，需要 DFS + 记忆化 |
| 385. Mini Parser                  | 递归解析嵌套结构              |
| 341. Flatten Nested List Iterator | 递归/栈展开嵌套列表            |

通俗理解:

`Backtracking` 就是：
- 选择一个
- 继续走
- 不行就撤销
- 换下一个

如果重复状态很多，加 `memo`

## 面试快速判断口诀

`DFS`
- 要走完整块区域
- 要找所有路径
- 要处理树的子树信息
- 要递归返回结果
- 用 `DFS`

`BFS`
- 问最短步数
- 问第几层
- 问多久扩散完
- 问最近距离
- 用 `BFS`

`Union-Find`
- 问几个组
- 问是否连通
- 问合并账号/集合
- 问加边是否成环
- 用 `Union-Find`

`Topological Sort`
- 问依赖顺序
- 问能不能完成
- 问是否有环
- 用 `Topological Sort`

`Dijkstra / Bellman-Ford / Floyd`
- 边权不同且非负 → Dijkstra
- 限制最多 K 条边 → Bellman-Ford
- 任意两点最短路 → Floyd
- 边权都是 1 → BFS
  
## 最核心记忆

- DFS：一条路走到底，适合连通块 / 树递归 / 所有路径
- BFS：一层层扩散，适合最短步数 / 层序遍历
- Union-Find：快速合并集合，适合连通性
- Topological Sort：处理依赖顺序
- Dijkstra：非负加权最短路
- Bellman-Ford：限制边数或允许逐轮松弛
- Floyd：所有点到所有点的最短路

面试里可以这样说：

I choose this method based on the structure of the problem: if it asks for reachability or connected components, I think of DFS/BFS; if it asks for shortest steps, BFS; if it asks for grouping or connectivity, Union-Find; and if it has dependency ordering, Topological Sort.