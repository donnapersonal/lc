Kruskal 算法就是：

把边按权重排序，优先选最优的边；如果这条边不会形成环，就选它。

它常用来求：

最小生成树：边权从小到大选
最大生成树：边权从大到小选
1. 什么是生成树

给你一个无向连通图，有 n 个点。

如果选出一些边，满足：

所有点都连通
没有环
一共正好有 n - 1 条边

这就是一棵生成树。

2. Kruskal 的核心思想

Kruskal 是一种 贪心算法。

求最小生成树时

始终优先考虑当前最小的边。

但不是最小边都能选，因为如果一条边的两个端点已经连通，再选它就会形成环。

所以规则就是：

按边权从小到大排序
从前往后看每条边
如果这条边连接的是两个不同连通块，就选它
如果已经在同一个连通块里，就跳过
直到选了 n - 1 条边
3. 为什么要用并查集

Kruskal 里最重要的问题就是：

这条边加进去会不会成环？

本质上就是问：

这条边的两个端点现在是不是已经连通了？

这个问题最适合用 并查集 Union Find 来维护。

4. 并查集在 Kruskal 里的作用

对于一条边 (u, v, w)：

如果 find(u) != find(v)
说明 u 和 v 还不连通，选这条边是安全的
然后 union(u, v)
如果 find(u) == find(v)
说明已经连通了，再选就成环，所以跳过
5. 最小生成树模板
Python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False
        self.parent[px] = py
        return True


def kruskal(n, edges):
    """
    n: 节点个数
    edges: [u, v, w]
    返回最小生成树总权重；如果图不连通，返回 -1
    """
    edges.sort(key=lambda e: e[2])  # 按权重从小到大排序
    uf = UnionFind(n)
    total = 0
    used = 0

    for u, v, w in edges:
        if uf.union(u, v):   # 只有不成环才选
            total += w
            used += 1
            if used == n - 1:
                return total

    return -1
6. 最大生成树怎么改

只要把排序改一下：

edges.sort(key=lambda e: -e[2])

这样就是优先选最大的边。

7. 手动例子
n = 4
edges = [
  [0,1,1],
  [1,2,2],
  [0,2,3],
  [2,3,4],
  [1,3,5]
]
第一步：排序

从小到大：

[0,1,1]
[1,2,2]
[0,2,3]
[2,3,4]
[1,3,5]
第二步：依次选边
边 [0,1,1]

0 和 1 不连通，选

边 [1,2,2]

1 和 2 不连通，选

现在已有边：

0 - 1 - 2
边 [0,2,3]

0 和 2 已经连通了，再选会成环，跳过

边 [2,3,4]

2 和 3 不连通，选

此时已经选了 n - 1 = 3 条边，结束。

答案

总权重：

1 + 2 + 4 = 7
8. 为什么 Kruskal 是对的

直觉上：

为了让总权重最小，我们应该尽量优先拿小边
但不能破坏“树”的性质，所以不能成环
并查集保证我们每次只加安全边

更正式地说，Kruskal 利用了 贪心选择性质：
当前能安全加入的最小边，一定可以出现在某棵最小生成树中。

9. 时间复杂度

设边数是 m，点数是 n。

排序
𝑂
(
𝑚
log
⁡
𝑚
)
O(mlogm)
并查集遍历所有边

几乎是线性的：

𝑂
(
𝑚
𝛼
(
𝑛
)
)
O(mα(n))

其中 α(n) 非常小，可以近似看成常数。

总复杂度
𝑂
(
𝑚
log
⁡
𝑚
)
O(mlogm)

因为排序是主导项。

空间复杂度
𝑂
(
𝑛
)
O(n)
10. Kruskal 什么时候特别适合

Kruskal 很适合：

边的列表直接给出
图比较稀疏
需要判断是否成环
需要求最小/最大生成树
11. 常见易错点
1）图不连通

如果最后选不到 n - 1 条边，就说明图不连通，没有生成树。

2）忘记并查集判断

如果你只按权重选边，不判断是否成环，就错了。

3）最小生成树和最短路径混淆

Kruskal 求的是：

让整个图连通，且总边权最小

不是求某两个点之间的最短路。

12. 一句话记忆版

Kruskal = 排序 + 并查集 + 贪心选边 + 避免成环

13. 面试口语版

你可以直接这么说：

Kruskal is a greedy algorithm for building a minimum spanning tree.
I sort all edges by weight, then scan them from smallest to largest.
For each edge, if its two endpoints belong to different connected components, I include it and union them.
If they are already connected, I skip it because adding that edge would create a cycle.
Once I have selected n - 1 edges, I have the MST.

如果你要，我下一条可以直接给你：
“Kruskal 和 Prim 的区别” 或者 “Kruskal 题型总结”。


Prim算法，该算法以顶点为单元，与图中边数无关，比较适合于稠密图

Kruskal算法，该算法以边为单元，时间主要取决于边数，比较适合于稀疏图