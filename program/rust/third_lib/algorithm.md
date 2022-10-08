- [Algorithm](#algorithm)
  - [1. 搜索：search.rs](#1-搜索searchrs)
  - [2. 排序：sort.rs](#2-排序sortrs)
  - [3. 图论：graph.rs](#3-图论graphrs)
  - [4. 数据结构：data_structures.rs](#4-数据结构data_structuresrs)
  - [5. 机器学习：ml.rs](#5-机器学习mlrs)
  - [6. 数学：math.rs](#6-数学mathrs)
  - [7. 字符串：string.rs](#7-字符串stringrs)
  - [8. 策略：problem.rs](#8-策略problemrs)
  - [9. 杂项：misc.rs](#9-杂项miscrs)

# [Algorithm](https://github.com/TianyiShi2001/Algorithms)

## 1. 搜索：search.rs

+ 二分查找
+ 插值搜索
+ 三元搜索

## 2. 排序：sort.rs

+ 归并排序
+ 选择排序
+ 冒泡排序
+ 插入排序
+ 计数排序
+ 堆排序
+ 基数排序
+ 桶排序
+ 快速排序
    - 霍尔隔断

## 3. 图论：graph.rs

+ 图表示
    - 邻接矩阵（加权和未加权）
    - 邻接表（加权和未加权）
    - 压缩邻接矩阵（加权）
+ 基本图算法
    - 深度优先搜索（迭代和递归）
    - 广度优先搜索（迭代）
+ 树算法
    - 树表示：有或没有指向父级的指针
    - 基础知识（DFS、树高、树总和等）
    - 树木中心
    - 树根
    - 树同构
    - 最低共同祖先（LCA）
+ 最小生成树/森林
    - Prim 算法
    - 克鲁斯卡尔算法
+ 网络流量
    - 福特-福克森 + DFS
    - 具有容量扩展功能的 DFS
    - Edmonds-Karp 算法 (BFS)
    - Dinic 算法 (BFS + DFS)
+ 最短的路径
    - BFS（未加权）
    - 拓扑排序的 DAG 最短路径
    - Dijkstra 算法（非负权重，SSSP）
    - 贝尔曼福特算法 (SSSP)
    - Floyd-Warshall 算法 (APSP)
+ 其他
    - 双向检查
    - DAG 图的拓扑排序和 DAG 最短路径
    - 欧拉路径/电路
    - 强连通分量（Tarjan 算法）

## 4. 数据结构：data_structures.rs

+ 并查集，不连接集
    - disjoint-sets = "0.4.2"
+ 位操作
+ 优先队列（二进制堆）
+ 平衡树
    - AVL树
    - 红黑树
+ 分离集（联合查找）
+ 稀疏表（范围查询）（通用）
+ 四叉树
+ k维树（kd tree）

## 5. 机器学习：ml.rs

+ 聚类
    - 分层聚类（WIP，几乎完成）
+ K 最近邻 (KNN)
    - 在四叉树中（文档/注释 WIP）
    - 在 kd 树中（文档/注释 WIP）

## 6. 数学：math.rs

+ 大中华区
    - 欧几里得算法
    - 互质
    - 扩展欧几里得算法
+ 液晶模组
+ 日志2
+ 质数
    - 主要检查
    - 埃拉托色尼筛
    - 素数分解（Pollard 的 rho 算法）
+ 线性代数（文档/注释 WIP）
    - 基础知识（矩阵表示、矩阵/向量算术）
    - 方阵的行列式（拉普拉斯展开式）
    - 高斯-乔丹消除法
    - LU分解
    - 乔列斯基分解

## 7. 字符串：string.rs

* 后缀数组：suffix_array;
* 后缀树：suffix_tree;
* 后缀trie树：suffix_trie;

## 8. 策略：problem.rs

+ 动态规划
    - 编辑距离
    - 背包 0/1
+ 回溯
    - 数独
    - N-皇后
+ 图形
    - 旅行商问题（暴力和DP）
+ 网络流量
    - 老鼠和猫头鹰

## 9. 杂项：misc.rs

* 数组，组合迭代器：combinations;
* 数组，排列迭代器：permutations;