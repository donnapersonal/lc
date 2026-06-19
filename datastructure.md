# 快速自查表

核心就是三件事：`它解决什么问题？复杂度？看到什么关键词想到它？`

| Data Structure | 解决什么问题 | 核心操作复杂度 | 看到什么关键词想到它 |
| --- | --- | --- | --- |
| Array | 按 index 快速随机访问；连续区间；原地修改 | access `O(1)`, search `O(n)`, insert/delete middle `O(n)` | index, subarray, in-place, prefix, range |
| String | 字符序列处理；substring/subsequence；频率统计 | access `O(1)`, scan `O(n)` | substring, palindrome, anagram, character count |
| HashMap | 快速建立 key → value 映射；计数；查找 | get/put average `O(1)` | count, frequency, mapping, seen before, pair sum |
| HashSet | 快速判断是否存在；去重 | add/remove/contains average `O(1)` | duplicate, unique, visited, exists |
| Stack | 后进先出；处理最近的未匹配元素 | push/pop/top `O(1)` | parentheses, undo, nested, previous, valid | 
| Monotonic Stack | 找下一个更大/更小；维护单调性 | each element push/pop once, total `O(n)` | next greater, previous smaller, temperature, histogram |
| Queue | 先进先出；按顺序处理 | enqueue/dequeue `O(1)` | level order, process in order, waiting line |
| Deque | 两端进出；滑动窗口最大/最小 | push/pop both ends `O(1)` | sliding window maximum, maintain candidates |
| Linked List | 高效插入/删除节点；指针操作 | access/search `O(n)`, insert/delete with node `O(1)` | reverse list, cycle, merge lists, remove node |
| Heap / Priority Queue | 动态取最小/最大；Top K；资源分配 | peek `O(1)`, push/pop `O(log n)` | top k, kth, closest, smallest/largest, earliest end | 
| Tree | 层级结构；递归分治；父子关系 | traversal `O(n)`, balanced BST search `O(log n)` | root, leaf, height, path, ancestor, subtree | 
| Binary Search Tree | 有序树；快速查找/插入/删除 | average `O(log n)`, worst `O(n)` | sorted tree, search value, range query | 
| Graph | 表示任意关系网络 | traversal `O(V+E)` | nodes, edges, connected, path, dependency, cycle |
| Trie | 前缀树；字符串前缀查询 | insert/search `O(L)` | prefix, dictionary, autocomplete, word search |
| Union Find | 动态连通性；合并集合；判断是否同组 | nearly `O(1)` with path compression | connected components, union, groups, redundant edge |
| Prefix Sum | 快速算区间和 | build `O(n)`, range query `O(1)` | range sum, subarray sum, cumulative | 
| Difference Array | 区间批量更新 | range update `O(1)`, rebuild `O(n)` | many range increments, booking, intervals update | 
| Ordered Set / TreeMap | 需要有序查找、floor/ceiling | `O(log n)` | nearest smaller/larger, sorted dynamic data |

# 直觉

## Array / String

它们解决的是：`“我能不能用 index 扫一遍，把答案维护出来？”`

常见模式：
- two pointers
- sliding window
- prefix sum
- in-place swap
- sort + scan

看到这些词先想 `Array/String`：
- subarray
- substring
- continuous
- remove duplicates
- rotate
- merge
- range sum
- palindrome
- anagram

## HashMap / HashSet

它们解决的是：`“我不想每次都扫一遍，我想 O(1) 查以前见过的信息。”`

`HashMap` 存：
- value -> index
- char -> count
- node -> clone node
- prefixSum -> frequency

`HashSet` 存：
- seen values
- visited nodes
- unique states

看到这些词先想 `Hash`：
- duplicate
- frequency
- count
- seen before
- pair
- anagram
- visited
- cache
- mapping

## Stack / Queue

`Stack` 解决的是：`“我要处理最近的、还没被匹配/解决的东西”`

`Queue` 解决的是：`“我要按层、按顺序扩散处理”`

看到这些词先想 `Stack`：
- parentheses
- nested
- valid
- remove
- previous greater/smaller
- next greater/smaller

看到这些词先想 `Queue/BFS`：
- level
- minimum steps
- shortest path in unweighted graph
- spread
- nearest

## Linked List

它解决的是：`“我不能随机访问，只能靠指针移动”`

核心技巧：
- dummy node
- fast / slow pointers
- prev / curr / next
- reverse pointers
- detect cycle
- merge two lists

看到这些词先想 `Linked List`：
- reverse
- cycle
- middle
- merge
- remove n-th
- reorder

## Heap / Priority Queue

它解决的是：`“我需要动态地拿当前最小/最大”`

核心直觉：
- 如果每次都要 `scan` 找 `min/max`，考虑 `heap`

看到这些词先想 `Heap`：
- top k
- kth largest
- closest
- merge k
- minimum cost
- earliest ending
- schedule resources

## Tree

它解决的是：`“一个节点的问题，依赖左子树和右子树的结果”`

核心递归模板：

```python
def dfs(node):
    if not node:
        return base

    left = dfs(node.left)
    right = dfs(node.right)

    return combine(left, right, node)
```

看到这些词先想 `Tree DFS`：
- height
- depth
- path
- ancestor
- subtree
- diameter
- balanced
- serialize

## Graph

它解决的是：`“节点之间有任意连接关系，不再是简单左右子树”`

核心表示：
- adjacency list: node -> neighbors
- visited set
- queue for BFS
- stack/recursion for DFS

看到这些词先想 `Graph`：
- connected
- component
- path
- cycle
- dependency
- shortest path
- island
- relationship

## Union Find

它解决的是：`“我要不断合并集合，并快速判断两个东西是否连通”`

核心操作：
- `find(x)`: 找 x 的根
- `union(a, b)`: 合并两个集合

看到这些词先想 `Union Find`：
- connected components
- number of groups
- merge accounts
- redundant connection
- same set
- dynamic connectivity

做题时可以背这个快速判断表：

| 如果题目说 | 优先想 |
| --- | --- |
| 有没有出现过、计数、映射 | HashMap / HashSet |
| 连续子数组/子串 | Sliding Window / Prefix Sum |
| 最近未匹配、括号、下一个更大 | Stack |
| 最短步数、层层扩散 | Queue / BFS |
| 链表反转、环、中点 | Linked List pointers |
| 动态取最大/最小、Top K | Heap |
| 父子结构、路径、高度 | Tree DFS |
| 任意节点关系、连通、依赖 | Graph |
| 合并集合、判断同组 | Union Find |
| 前缀查询、字典搜索 | Trie |
| 有序动态查找 | TreeMap / Ordered Set |

# 关键词 -> 模板

| Pattern | 看到什么想到它 | 例题 |
| --- | --- | --- |
| HashMap / HashSet | 查重、频率、映射、配对、计数、是否出现 | Two Sum、Group Anagrams |
| Two Pointers | 有序数组、两端、原地操作、配对 | 3Sum、Container With Most Water |
| Sliding Window | 连续子数组/子串、最长/最短、满足条件 | Longest Substring Without Repeating Characters |
| Prefix Sum + HashMap | 连续区间和、区间计数、存在多少个 | Subarray Sum Equals K |
| Sort + Scan / Sort + Heap / Sweep Line | 区间、重叠、相邻关系、调度、资源复用 | Merge Intervals |
| Binary Search / Binary Search on Answer | 有序、边界、最小可行值、答案具有最小可行性、单调性、min max 、 max min 、 first true | Search in Rotated Sorted Array |
| DFS / BFS | 树、网格、连通区域、最短步数、层层扩散 | Number of Islands |
| Tree Recursion | 高度、路径、子树结果、祖先 | Lowest Common Ancestor |
| Heap / PriorityQueue | Top K、动态最大最小、调度、合并有序流 | Kth Largest Element、Meeting Rooms II |
| Heap / Quickselect | top k / kth / closest |  |
| Stack | 最近未匹配、括号、撤销、嵌套、表达式 | Valid Parentheses |
| Monotonic Stack | 下一个更大/更小、左右第一个、柱状图 | Daily Temperatures |
| Linked List Pointers | 快慢指针、反转、合并、环 | Reverse Linked List、Linked List Cycle |
| Backtracking | 所有组合、排列、切分、选择路径 | Combination Sum |
| 1D DP | 最优值、方案数、当前位置依赖过去 | House Robber |
| 2D DP | 两个序列、网格状态、字符串匹配 | Longest Common Subsequence |
| Topological Sort | 依赖关系、课程顺序、构建顺序 | Course Schedule |
| Union Find | 动态连通、分组、冗余连接 | Redundant Connection |
| Greedy | 局部选择、区间调度、最少/最多操作  | Jump Game |
| Matrix Traversal | 网格边界、方向模拟、状态扩散 | Rotting Oranges |
| Trie | 前缀搜索、字典匹配、自动补全 |  |
| Dijkstra | 非负权重最短路径 |  |
| Bit Manipulation | 异或、状态压缩、位计数 |  |
| Advanced Graph | MST、Bellman-Ford、Floyd |  |
| Advanced DP | 区间 DP、树形 DP、状态压缩 DP |  |
| Segment Tree / Fenwick Tree | 通常不是通用面试第一优先级 |  |