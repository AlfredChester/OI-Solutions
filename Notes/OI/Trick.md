动态维护直径——考虑并查集，带权为当前联通块的直径的两端点 $u_i, v_i$。合并时直径为 $\max \{\mathrm{dis}(u_x, v_x), \mathrm{dis}(u_x, v_y), \mathrm{dis}(u_y, v_x), \mathrm{dis}(u_y, v_y)\}$。

两个东西不能一起选的时候可以连二分图跑最大匹配。

Dilworth 定理：最小链覆盖数等于最大反链大小。

查询区间出现元素种类相关：扫描线。然后 lst - 1, cur + 1。

区间做一个东西比较困难的时候，不妨看看能否差分转化为单点做一个事情。

处理树上有交的路径的时候，可以考虑 LCA。

有二元对要处理线性基，可以先 a * 2^k | b 跑线性基，消成 log 对之后再试试暴力。

对于一维结构（或准一维结构）上的类哈密顿问题，可以考虑拆边的贡献。例题：https://www.luogu.com.cn/problem/P10712

线段树二分见 jiangly 模板。你可能还得看看图论相关。

Slope Trick: 见 [APIO2016] 烟花表演的题解以及 https://www.luogu.com.cn/article/xvbc3rk8。通常用于处理一类调整问题。

图上路径异或和：使用生成树的前缀和 $f_u$。$A = \{ f_u \oplus w \oplus f_v \}$ 的基，则 $1 -> n$ 的路径的线性基为 $B = \{ f_n \oplus x \in A \}$。

对于 $a, b, c \in \mathbb{Z}$，$\frac{1}{a}+ \frac{1}{b} = \frac{1}{c} \implies ac + bc - ab = 0 \implies c ^ 2=(a-c)(b-c)$，可以做一些数论的东西。例题：https://www.luogu.com.cn/problem/P4844

一个置换能组成的排列个数为 $\mathrm{lcm} \space c_i$，其中 $c_i$ 是每个环的长度。置换有显然的性质是 https://www.luogu.com.cn/problem/P12553

树上两个相邻的点一定有一个点是另一个点的父亲，可以用来保证均摊复杂度是对的。https://www.luogu.com.cn/problem/P12698

分治结构每层不同的长度个数不超过 $2$。

卡空间的时候（经常是多了一个 $\log$）可以考虑能不能通过跑 $\log$ 次解决。也可能用在图连通性上。

区间加区间除可以嗯暴力：区间 max, min 除这个值相同直接变成加减操作。

正着跑 dp 可能和倒着跑复杂度不一样。https://newoj.cyez.cc:18160/d/problemset/p/1499