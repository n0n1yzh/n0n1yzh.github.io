---
title: Dijkstra
date: 2020-12-01 01:01:15
categories:
- AlgorithmsThinking
- Dijkstra
mathjax: true
cover: false
top_img: false
aside: false
tags:
---

### Dijkstra

Dijkstra 是解决带权重的有向图上的单源最短路径问题。该算法要求**对所有的边的权重均为非负值**。

集合 $V:$ 所以节点构成的集合

集合 $S:$ 已经维护好了的节点构成的集合

算法重复进行从 $ V-S$ 中选择**最短路径估计**[^1]最小的节点 $u$ ，将其加入 $S$ 中，然后对所有从 $u$ 出发的边进行**松弛**[^2]。

在伪码中，使用一个最小优先队列 $Q$ 来维护还未加入 $S$ 中的节点，每次循环从 $Q$ 中拿到最短路径估计最小的节点 $u$ ，此时将这个节点永久标记成 $S$ 中的节点，并对从这个节点 $u$ 发出的边进行**松弛**。

```
Dijkstra.(G,w,s)
# 初始化求从s出发的单源最短路径
# 初始化结束后，对于V中所有节点，我们有v.Π = NIL && s.d = 0 && others.d = INF 
Initialize-Single-Source(G,s)
S = {}
Q = G.V
while Q != {}
     u = EXTRACT-MIN(Q)
     S = S + {u}
     for each vertex v 属于 G.Adj[u]
         RELAX(u,v,w)
```



[^1]: 对于每个节点 $v$ 来说，我们维持一个属性 $v.d$ ，用来记录从源节点 $s$ 到节点 $v$ 的最短路路径权重的上界。我们称 $v.d$ 为 $s$ 到 $v$ 的**最短路径估计**
[^2]:**松弛的对象是边**。对一条边 $(u,v)$ 的松弛操作：  $if\ v.d>u.d+w(u,v) \quad Then \ v.d = u.d+w(u,v)\ and \ v.\pi=u$  其中 $v.\pi$ 为节点v的前驱。