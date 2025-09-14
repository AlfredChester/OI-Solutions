# OI TRICKS


<h2 id="oi-技巧套路思想">OI 技巧/套路/思想</h2>

Start from 2022.3.16

以前写过一个 <a href="https://www.cnblogs.com/wyb-sen/p/15576545.html" target="_blank">总结复习</a> ，大致是以算法为纲目，然后每个算法尽量找一个经典题目出来，随着 CSP 的过去，上面那个文章渐渐咕咕了

随着 14 天停课集训的结束，逐渐意识到自己整理记录一些 trick 之类的重要性，然后发现好像上面那个文章未免略显臃肿，于是决定这篇文章以 trick/套路 为纲目，<s>上一篇直接不管了</s>

随着时间的流逝，越发发现本文的不完善，有很多东西，第一次看到的时候以为是 ad-hoc 就没有记录。后面感觉某一个技巧自己见过却找不到原，这是一个遗憾。

<h3 id="二分答案">二分答案</h3>

<blockquote>

圣经：那这怎么优化呢？你看到他数据范围出 log 了，那还有什么好说的，对不对？Stop learning useless algorithms, go and learn Binary Search. 我们就无脑二分

</blockquote>

本质是把一个最值问题转成判定问题

对于一些题目，而判定一个解是否存在，也可以理解成于计数有多少个解。

[Gym102354B] Yet Another Convolution 通过二分答案，把 max 变成 \sum。

<h3 id="交换枚举顺序">交换枚举顺序</h3>

非常经典的思想了，非常多的题目都有应用

<h3 id="削减问题规模递归处理">削减问题规模递归处理</h3>

很基础的思想，但是一直没有明确写在这里。

Ptz Summer 2022 Day2E Ternary Search

<a href="https://atcoder.jp/contests/arc172/tasks/arc172_e" target="_blank" rel="noopener nofollow">ARC172E</a>

[Ptz Winter 2020 Day 6 D] Split in Sets

<h3 id="01-bfs">01-BFS</h3>

如果边权只涉及 0/1 ，请考虑 01-BFS <s>（每次都忘了/ll</s>

<a href="https://www.fzoi.top/problem/5522" target="_blank" rel="noopener nofollow">https://www.fzoi.top/problem/5522</a>

WOJ 4701 Walk

<h3 id="构造题考虑下界构造解考虑无解没事就连边不会就调整">构造题考虑下界，构造解考虑无解，没事就连边，不会就调整！</h3>

AMPPZ2014 The Cave

<h3 id="构造性质图的思路">构造性质图的思路</h3>

两种思路：1. 所有有性质的图，强行构造 2. 构造不出来，说明性质很强，考虑什么样的图才能有构造。

始终秉持从易到难考虑的原则，才能下手问题。即秉持一个规约的思想。

CF1656I Neighbour Ordering

<h3 id="上下界之间的所有数字都可以通过调整取到">上下界之间的所有数字都可以通过调整取到</h3>

ARC125F Tree Degree Subset Sum：<a href="https://www.fzoi.top/post/1107" target="_blank" rel="noopener nofollow">20220303pb</a>

<blockquote>

树的具体形态显然用处不大：只要 ∑di = 2n − 2 都可以构造出一棵合法的树。我们的主要问题出在多了一维 x 。因为什么性质也找不出来，尝试把 x 这一维消掉。先把所有 di 都减掉 1 ，那么 ∑di = n − 2 。我们尝试证明：对于总和 y ，设 m(y), M(y) 分别是最小和最大的 x ，那么x ∈ [m(y), M(y)] 都有合法方案。我们的证明主要和 0 的个数 z 有关。因为 m(y) 必然一个 0 都没选，而 M(y) 选了 z 个 0 ，所以只要 M(y) − m(y) ≤ 2z ，就可以通过调整 0 的个数构造出每个 x 。发现对于任一个集合 S ，都有 −z ≤ y(S) − x(S) ≤ z − 2 ，所以 −z ≤ y − M(y) ≤ y − m(y) ≤ z − 2 ，所以M(y) − m(y) ≤ 2x − 2 ，然后就证完了。

</blockquote>

[HDU7300]Magma Cave 判定是否存在生成树使得第 k 大边权值是 w，使用 LCT 维护最大/最小生成树并同时辅助一个主席树求第 k 大。

TopCoder14651 CanidsSeesaw

<blockquote>

给定长度为 $n$ 的正整数序列 $x_i,a_i$，以及一个整数 $k$,求出一个排列 $p_i$，使得：

记 $sumx_i=\sum x_j,sumy_i=\sum a_{p[i]}$，那么有 $\sum [sumy_i&gt;sumx_i]=k$

</blockquote>

求出最大的，求出最小的，判定可不可以。

然后发现交换相邻的 $p_i,p_{i+1}$ 答案至多变化 $1$，那么赢麻了。

<a href="https://www.fzoi.top/problem/7868" target="_blank" rel="noopener nofollow">20231108T2 金币 coins</a>

<hr>

衍生：正反两个方案拼接 ：Gym 102268J Jealous Split

<h3 id="交互题">交互题</h3>

一个常见思路：倒推题目，即假装我知道了 xxx，那么这道题就可以做了。为了知道 xxx，我需要 $\dots$

<h3 id="单调栈退栈时做转移">单调栈退栈时做转移</h3>

[CF1407D]Discrete Centrifugal Jumps 探究 DP 转移的条件，发现就是单调栈

<h3 id="正难则反时间倒流">正难则反/时间倒流</h3>

RT，反着想很多时候有奇效，可能很多时候出题者都是先有了反着想的思路，为了增加难度才魔改出题目

为啥会忘记反着思考啊！！！

反正想有时候就是一种容斥的思考

<a href="https://www.fzoi.top/problem/5400" target="_blank" rel="noopener nofollow">[JOI2016Final]断层</a> 发现风化条件恶心，考虑倒着做，每次向下移动，维护 0 层原来位置即可，可以使用线段树。

<a href="https://www.luogu.com.cn/problem/P4172" target="_blank" rel="noopener nofollow">[WC2006]水管局长 </a> 删边不好维护，不妨将其离线下来倒着做，这样就变成了维护加边，用 LCT 等数据结构维护一下即可

<a href="https://www.fzoi.top/problem/5611" target="_blank" rel="noopener nofollow">[CCO2021D2T1]Travelling Merchant</a> 倒着想，在有解的情况下，选择当前最大的 $r_i$ 肯定是可以的。所以可以使用当前的 $r_i$ 去贡献，然后把这条边删掉。删掉过程中，一些点变成无解点（没有出度），于是可以确定这些点的 ans，然后把这个点删掉，注意在删点的时候记得更新连到它的点的答案

<a href="https://www.fzoi.top/problem/6805" target="_blank" rel="noopener nofollow">CF1750F Majority</a> 有趣的正难则反。首先可以知道，将 0 视作 -1，可以把一个连续段缩成一个数；如若考虑一个序列合法的条件，发现是难蚌且难以直接 DP 的，但是不合法的条件是非常清楚的，即对于任意相邻的三个数 $x, y &lt;0, z, |y| &gt; x + z$。如此可以设出一个 dpij 表示长度为 i 的串，最后一段 1 长度为 j；钦定首尾必定为 1， 发现 dp{i, 1....i-1} 是可算的，用总方案数 ( $2^{i - 2}$) 减去即可得到 dp{i, i}。dp 的过程可以两次前缀和优化。

一边选一边有删除的问题一定要时间倒流，变成一边出现一边选，这个远远贪心 friendly 过原来的问题。

<a href="https://www.fzoi.top/problem/5597" target="_blank" rel="noopener nofollow">[CCO2017]Professional Network</a> 倒着想，从白嫖的思路想，排序后倒着做。假装最开始全都买了，然后能嫖则嫖，不能嫖就从已经嫖了的一个里面选择最小的一个不嫖了，有一点反悔贪心的味道 双倍经验：<a href="https://codeforces.com/problemset/problem/1251/E2" target="_blank" rel="noopener nofollow">Voting</a>

<a href="https://www.luogu.com.cn/problem/P6642" target="_blank" rel="noopener nofollow">[CCO2020] Exercise Deadlines</a> （好像 CCO 很多这种反着想的题，最开始正着想了一个线段树二分暴力维护的做法哈哈）我不考虑我小的数怎么办，我考虑我大的数贪心地仅可能地往后放。倒着做，用一个大根堆维护当前可以放的数的位置集合，每次从其中取出最大的放在当前位置，然后把新的可以放的位置加入堆。

<a href="https://www.luogu.com.cn/problem/P3826" target="_blank" rel="noopener nofollow">[NOI2017]蔬菜</a>

<a href="https://www.luogu.com.cn/problem/CF1886D" target="_blank" rel="noopener nofollow">CF1886D Monocarp and the Set</a>

<h3 id="邻项交换">邻项交换</h3>

国王饮水记

<a href="https://www.fzoi.top/problem/6969" target="_blank" rel="noopener nofollow">Kenkoooo</a> 首先可以用邻项交换法证明知道对于同一列，应该是先尝试 p 比较小的，感性的理解是，先做不容易做的，后面的做起来轻松。现在的问题是考虑列和列之间的顺序。

一个看起来很直觉的贪心是设 $prob_j = \prod p_i$，即全部通过的概率，先尝试 prob 比较大的。

但是这样是错误的，是无证明的，一个简单的例子是，当两者 prob 相等的时候，先后仍然存在影响。求出 $e_j = 1 + p_1 + p_1p_2 + p_1p_2p_3+\dots$ 表示单独考虑第 j 的期望尝试次数：

<ul>
<li>先 i 后 j，期望次数为 ans1 = ei + (1 − probi)ej；</li>
<li>先 j 后 i，期望次数为 ans2 = ej + (1 − probj )ei。</li>
</ul>

令 ans1 &lt; ans2，导出 ei/probi &lt; ej/probj 是全序关系，那么按这个升序排序即可。

<a href="https://www.luogu.com.cn/problem/AT_agc023_f" target="_blank" rel="noopener nofollow">AGC023F 01 on Tree</a>

<h3 id="从出题人角度考虑问题">从出题人角度考虑问题</h3>

<blockquote>

为什么题目不出成任意两个数交换？出题人不会对不对<br>

—— lxl

</blockquote>

或者说，对于一些构造类的 SPJ 的题目，可以考虑 SPJ 的复杂度，从而得到一些限制条件

也就是说，考虑某道题目的“可做点”到底在哪里，考虑如果删去了题目的某一个条件，问题还可以不可以做，问题是否变得复杂。

<a href="https://www.fzoi.top/problem/5707" target="_blank" rel="noopener nofollow">CF1620F Bipartite Array</a> <a href="https://www.luogu.com.cn/blog/_post/409408" target="_blank" rel="noopener nofollow">Sol1</a>

<h3 id="构造维护差分序列">构造/维护差分序列</h3>

看到区间操作，一定要想到转化成差分序列上的单点修改，注意转化成差分序列后，对应的充要条件可能会变化

<a href="https://www.fzoi.top/problem/5607" target="_blank" rel="noopener nofollow">[ARC136C]Circular Addition</a> 转化成差分序列后，发现操作变成了每次可以任意选择两个数一者+1，一者-1。所以答案为 $\max\{mx,sum1,-sum2\}$ ,$mx$ 是原序列最大值，$sum1/2$ 分别表示差分序列中正负数之和。之所以取 $mx$ 是考虑到差分数组变成 $0$ 不代表原序列为 $0$ ，而且当且仅当这个环呈现单峰状态的时候，$mx&gt;\max\{sum1,-sum2\}$

<a href="https://www.luogu.com.cn/problem/P3348" target="_blank" rel="noopener nofollow">[ZJOI2016]大森林</a> <a href="https://www.cnblogs.com/wyb-sen/p/15924032.html" target="_blank">题解</a> 其中所谓考虑 $l-1,l$  的差异，其实把本来的区间修改差分一下，然后一个做前缀和的过程，然后这个差异的修改用到了数据结构。

<a href="https://www.luogu.com.cn/problem/P7560" target="_blank" rel="noopener nofollow">[JOISC 2021 Day1] フードコート</a> 同样是借用了上一道题大森林的思想。后续也借用了一个经典思想就是，因为是删除一段前缀，所以不需要真正删除，平移就可以了。为了计算查询平移之后的位置，可以使用一个数据结构维护只增加的人数，一个数据结构维护实际人数，那么查询位置就是 第一个数据结构中的人数-第二个数据结构中的人数+B （如果第二个数据结构人数&gt;B）。第一个数据结构可以选用树状数组；第二个数据结构需要支持 区间修改/区间对 $0$ 取 $\max$ ，一种方式是吉司机线段树，另外一种方案是线段树每一个点维护二元组 $(ad,mx)$ 表示这个区间先 $+ad$ 然后对 $mx$ 取 $\max$ ，把标记 pushdown 就可以了。具体查询可以使用平衡树查询。

<blockquote>

CODE FESTIVAL 2017 Final E Combination Lock<br>

给定一个长为 $n$ 的序列 $a$ 和 $m$ 个 $(l, r)$，保证 $a_i\le 26$，可以做无数次操作，每次可以对一个 $(l, r)$ 执行区间 $\bmod 26$ 意义下 $+1$，问是否可以变成回文串。

</blockquote>

模意义 + 1，由于可以进行任意次操作，那么做 mod - 1 次等价于 -1，所以实际上加减任意。区间操作转为维护差分序列，补一个 $a_{n + 1} = 0$，设 $d_i = a_i - a_{i + 1}$，那么实际上是要求 xxxx，然后将每次对 d_l + 1, d_r - 1，视作是二者在交易货币之类，一个连通块内的人可以任意交易货币，之类之类然后就可以做了。/cy

<hr>

或者说是形如 $\max-\min$ 的限制，也可以转成差分序列的条件来构造

<a href="https://www.fzoi.top/problem/5408" target="_blank" rel="noopener nofollow">[CF1500F]Cupboards Jumps</a> 限制转化成 $\max\{|d_i|,|d_{i+1}|,|d_i+d_{i+1}|\}=w_i$ ，瞎 jb 设一个 $dp_{i,j}$ 表示 $d_i=j$ 行不行，然后发现每次转移值比较特殊，所以使用了这样的 <a href="#Cupboards_Jumps2" rel="noopener nofollow">处理方式</a> <a name="Cupboards_Jumps1"></a>

另外的，对于一类 区间修改 + 前缀最大值 的 DS 问题，可以维护原序列的差分序列

假设这两种操作是交替的，考虑每次把一段区间 $[l,r]$ 整体抬高 $h$，此时由于前缀 max, 需要二分出第一个位置和 $a[r]$ 的差值大于 $h$ ；把一段区间整体降低 $h$ ，那需要二分出来的是那个位置不需要向前面取 $\max$

由此，发现我们非常关注差值的变化，所以我们考虑维护差分数组。<br>
由此，引出了线段树上二分来维护的思路。

JOISC 2019 day2 两道料理 <a href="https://www.cnblogs.com/wyb-sen/p/16537916.html" target="_blank">题解</a>

差分的形式和裂项相消是很接近的，这样转化的好处是可以使得在线维护某一些贡献

CF626F Group Projects 首先对 a 排序，可以将一个组的最小/大值看成左右括号，容易得到 f_{i, j, k} 表示前 i 个，有 j 个左括号没有匹配，不和谐度总和 k；转移考虑如果加入左括号，k -= a；右括号 k += a。但是这样的复杂度是 n^2值域，因为可能先有很多左括号之类。<strong>考虑差分将贡献拆成单点的形式/裂项相消</strong> 即可。

对于单调序列计数，通过差分，可以去掉单调的限制，即本来是计数 ai，变成计数 ai 的差分数组。这样做可能会有一些意想不到的好处。

<a href="https://atcoder.jp/contests/abc221/tasks/abc221_h" target="_blank" rel="noopener nofollow">ABC221H Count Multiset</a>

<h3 id="对-1100-操作--对-10-操作">对 11/00 操作 $\to$ 对 10 操作</h3>

CF1615F LEGOndary Grandmaster：操作是可以把 11-&gt;00，00-&gt;11，性质是操作前后异或和不变，可以想到如果对 1,0 进行操作，那么异或和也不变。考虑令 $ai = ai \oplus (i\&amp;1)$，此时每次操作就是交换。

[AGC040C] Neither AB nor BA：不能操作 AB/BA，将偶数位的 AB 翻转，等价于不能操作 AA/BB，剩下的分析是容易的。

<hr>

同时，3 是一个很神奇的数字，一些操作记得放在 $\bmod 3$ 的意义下考虑。

<h3 id="遇到各种和操作加法异或就考虑前缀和和差分">遇到各种和操作（加法，异或）就考虑前缀和和差分</h3>

<a href="https://www.luogu.com.cn/problem/P8864" target="_blank" rel="noopener nofollow">P8864 「KDOI-03」序列变换</a>

<h3 id="虚点">虚点</h3>

虚点再各个方面都有所应用，比如优化建图。

当我们需要对某一个集合执行某个操作的时候，建立一个虚点，那么只需要对虚点操作

<a href="https://www.luogu.com.cn/problem/P3348" target="_blank" rel="noopener nofollow">[ZJOI2016]大森林</a> <a href="https://www.cnblogs.com/wyb-sen/p/15924032.html" target="_blank">题解</a>

<a href="https://atcoder.jp/contests/abc250/tasks/abc250_h" target="_blank" rel="noopener nofollow">ABC250Ex</a> 由于起终点都是关键点，所以先用 dijkstra 求出<strong>相邻</strong>关键点之间的最短路（把松弛视作染 col，相邻 col 建图），然后 Kruskal 重构树。

<h3 id="求第-k-大">求第 k 大</h3>

仿照主席树求第 $k$ 大的方式，我们可以把排名问题转化为求对应值有多少数量的问题，在某种程度上有平衡复杂度的作用。

<a href="https://www.luogu.com.cn/problem/CF601C" target="_blank" rel="noopener nofollow">CF601C Kleofáš and the n-thlon</a> 发现如若直接把 DP 状态设置为排名非常难做，于是设 $dp[i][j]$ 表示前 $i$ 轮在不使用 $c$ 的情况下总得分为 $j$ 的人数的期望，然后就很好做了。

或者说根据 $\le x$ 的个数就是 $x$ 的排名。那么对于求解排名为 $k$ 的数是什么的问题，就可以通过二分答案 $mid$ ，然后求 $mid$ 的排名，即 $\le mid$ 的数量来做。<br>
这样算适用于计算 $\le x$ 的数量的时候需要用到 $x$ 的情况。

<a href="https://www.fzoi.top/problem/7003" target="_blank" rel="noopener nofollow">20230308 T1 分数</a> 二分答案，套用 01 分数规划，使用双指针求出 mid 的排名

AT_wtf19_d Distinct Boxes

<h3 id="前-k-大的方案">前 k 大的方案</h3>

即具体给出前 k 大的值，此时如果想要套用上面二分的想法，无疑是鸡肋且没有用好题目性质的。

大致思想是分成 m 组，每组有 log 求组内第 k 大的手段，第一次将所有组的最大值放入 优先队列，每次从优先队列弹一个，那么在对应组找到 k + 1 大代替（以前似乎在某到淀粉质里面也见过这种套路，似乎是对于分治重心的每一个字数开一个小根堆，外面开一个大根堆之类）。

发现实际上不是求第 k 大，是求的某一个状态的后继，实际上只需要掌握一个 trans(S) 方法，使得所有 val(S) $\le$ val(trans(S))，且利用 trans 操作可以到达所有状态，且 trans 复杂度低，且 S 利用 trans(S) 转移到的状态数量不会太多（pop 掉一个元素，不会新增太多后继）；

如若此时 trans 转移到了太多后继，那么可以把这些后继排序从而使得度数降低，例如后继 k1,k2,k3，变成 trans(s) = k1, trans(k1) = k2，trans(k2) = k3。

典题：十二省联考异或粽子 <a href="https://www.luogu.com.cn/problem/P2048" target="_blank" rel="noopener nofollow">NOI2010 超级钢琴</a>

[CCO2020] Shopping Plans：<strong>非常震撼</strong>。<a href="https://www.luogu.com.cn/problem/solution/P6646" target="_blank" rel="noopener nofollow">command_block</a> 的题解写得很好。

<h3 id="摩尔投票">摩尔投票</h3>

摩尔投票 + 线段树可以求出区间出现次数前 k 大。

摩尔投票：<a href="https://zhuanlan.zhihu.com/p/387744743" target="_blank" rel="noopener nofollow">https://zhuanlan.zhihu.com/p/387744743</a>

例题：<a href="https://codeforces.com/contest/674/problem/G%EF%BC%8C" target="_blank" rel="noopener nofollow">https://codeforces.com/contest/674/problem/G，</a> 注意 p\le 20

<h3 id="子树信息">子树信息</h3>

记得考虑使用 dfn 序 + 二维偏序/ 分治 DFN / 线段树合并 / 有根树淀粉质 / 淀粉树 / DSU on Tree / <strong>BFN 序</strong> 来获取子树信息

或者，转换一下角度，使用树上差分，考虑每一个点对于其祖先的贡献之类。

<hr>

BFN 序维护子树信息适用于点的贡献和深度信息强相关，缺陷在于要求操作可差分。

[GDKOI2023DAY2T3] 这道题，首先考虑序列上怎么做，可以设 $f_{i, j}$ 表示 u = i，$[i, i + 2^j)$ 的答案，$f_{i, 0} = v_i$ 有

（倍增和二进制操作的良好相性）


$$
f_{i, j} = f_{i, j - 1} + f_{i + 2^{j - 1},j-1} + \sum_{k = i + 2^{j - 1}}^{2^j - 1} (2^{j - 1} - 2^j[v_k \&amp; 2^{j - 1}])

$$

后续的 \sum 的意义是考虑这些点的 dis 的二进制位第 j - 1 位多了一个 1，讨论 v_k 这一维是 0/1。

序列上树，考虑每一个点 u 向最右儿子连边权 1，右儿子向左边一个儿子连边权 0，最左边一个儿子向 u 的左边一个兄弟的右儿子连边权 0；<strong>如果 u 没有儿子但是在 u 的同层左侧的点 v 有儿子，将 u 的右儿子设为最近的 v 的右儿子</strong>，这样形成一张 DAG。

设 f_{i, j} 表示 u = i, 从 i 开始走 DAG，走 $&lt; 2^j$ 边权可达的点的答案，实际上也就是直到 i 的 $2^j$ 级右儿子之前（不包含），每一个右儿子以及其向左衍生出来的一条链。

这样维护的是一个前缀信息，维护方法和序列上类似，处理好后缀和。

求答案的时候只要 u 子树内的，减去 u 的左边一个兄弟的答案即可，由此可以看出为什么要连 黑体 字提到的边。当然另外一个解决方案是先对于 u，把 u 的儿子按照 maxdep 排序？不知道，试了试发现不行，画一画的确存在一些神笔情况难以避免。

<hr>

DSU on Tree：当然题目中不一定明确的问 子树i的答案，可能是把问题转化后需要算子树i的答案。DSU 不仅可以做子树内的贡献统计，还可以做子树外面的统计，只需要维护补集即可。

<blockquote>

给定一棵n个点的树树。定义 f[L,R] 表示为了使得L~R号点两两连通，最少需要选择的边的数量。求 $\sum_{L,R} f[L, R]$<br>

———— <a href="https://www.cnblogs.com/zwfymqz/p/9683124.html" target="_blank">attack</a>

</blockquote>

对于每一条边统计有多少个区间跨过这条边即可；统计这一问题的对偶问题,有多少个区间没跨过会更方便；使用启发式合并+并查集统计子树内的，使用启发式合并+set统计子树外的。

具体的，考虑每次把子树内点的信息全部加入的过程，用 Set 维护当前已经出现过的所有点。每次 s.insert(x)，找出它的前驱 pre 和后继 nxt，删除 C(nxt - pre, 2), 加入 (x - pre, 2) + (nxt - x, 2) 即可。

这道题看起来还有点换根 DP 的感觉，有点套用 “正反做两遍” 中的技巧的想法，但是仔细想一想是做不到的。

--

子树 = dsu on tree 操作序列某一前缀

<a href="https://codeforces.com/problemset/problem/1767/F" target="_blank" rel="noopener nofollow">CF1767F Two Subtrees</a>

划分出轻重儿子之后，可以暴力 nlogn 扫描轻子树而获得对于一条重链的<strong>所有“旁支”的信息</strong>。

2023集训队互测 R1T3 树

yr 的方法：先 dfn 序处理出来，然后这样就可以以另外的，利于计算贡献的方式来遍历所有点，然后处理一个插入一个之类。例子还是 treelan tour，解法是先淀粉质，然后通过上述思路求解每一个点到子树内的最长 LIS/LDS，求解顺序按照权值大小。由于是淀粉质，所以要维护次大之类，相对麻烦。

--

考虑用 DS 维护重儿子信息，然后暴力枚举轻儿子来拼路径。同时，在 dfs 过程中开 log 个线段树的方法，即一条重链可以不断继承自己重儿子的线段树并暴力枚举合并轻子树。注意撤回。

Treeland tour

<a href="https://www.luogu.com.cn/problem/P5391" target="_blank" rel="noopener nofollow">[Cnoi2019] 青染之心</a> 每一个点上放了一个物品，求根到每一个点的完全背包，卡空间。那么我们这里考虑一个类似倒着 dsu on tree 的过程，在 dfs 的过程中维护 log 个背包，对于过程中一个点 $u$，先扫所有轻儿子，并用当期的背包数组 $f[dep]$ 更新 $f[dep+1]$；最后扫描重儿子，更新 $f[dep]$。

<hr>

分治DFN ：<a href="https://www.cnblogs.com/wyb-sen/p/15888449.html" target="_blank">CF600E</a>

分治 DFN 的独特优势在于可以处理一类只能撤销，不能删除的贡献！

<a href="https://www.luogu.com.cn/problem/P7124" target="_blank" rel="noopener nofollow">[Ynoi2008] stcm</a> 分治 DFN 序大放光芒！

<hr>

子树信息 = 重儿子 + 轻儿子。所以对于一类 DDP 问题，可以类似树套树地，每一个节点开一个 ds 维护轻儿子，重链上开一个 DS 维护重儿子。（实际上没有套起来）如若觉得给轻儿子开一个 DS 令人烦躁，可以考虑多叉树转二叉树，这样就只会有一个轻儿子了/hanx

CF1740H MEX Tree Manipulation

<h3 id="点到根的信息">点到根的信息</h3>

出栈序直接拍平/<strong>树剖之后维护前缀信息</strong>/有根树点分治/动态开点线段树

<a href="https://www.luogu.com.cn/blog/_post/275907" target="_blank" rel="noopener nofollow">[NOI2014] 购票</a> <a href="https://www.luogu.com.cn/blog/_post/275907" target="_blank" rel="noopener nofollow">出栈序直接拍平</a>

<a href="https://www.luogu.com.cn/problem/P4482" target="_blank" rel="noopener nofollow">[BJWC2018]Border 的四种求法</a>  <a href="https://www.cnblogs.com/wyb-sen/p/16087987.html" target="_blank">维护前缀信息</a>

<a href="https://www.luogu.com.cn/problem/CF932F" target="_blank" rel="noopener nofollow">CF932F Escape Through Leaf</a> <a href="https://www.luogu.com.cn/blog/_post/241413" target="_blank" rel="noopener nofollow">有根树点分治</a>

由于点到根走的是多条重链的前缀，所以这个性质有时候可以前缀和优化建图，比线段树少一个 log。

<h3 id="反悔贪心">反悔贪心</h3>

常见的思维过程是先贪心地naive进行选择，后面碰到更好的选择的时候把前面的选择替换掉。

<a href="https://www.luogu.com.cn/problem/P1792" target="_blank" rel="noopener nofollow">经典：种树</a> 找到权值最大的位置i，不难发现i-1和i+1不可能恰好只选择一个，那么我们选择i，并用xi-1+xi+1-xi替换xi-1,xi和xi+1。用双向链表+堆维护即可。

<a href="https://www.fzoi.top/problem/5529" target="_blank" rel="noopener nofollow">Warehouse Store</a> 正着做，能选就选，不能选就找一个之前最大的换掉(如果更小的话)

<a href="https://www.fzoi.top/problem/5597" target="_blank" rel="noopener nofollow">[CCO2017]Professional Network</a> 倒着想，从白嫖的思路想，排序后倒着做。假装最开始全都买了，然后能嫖则嫖，不能嫖就从已经嫖了的一个里面选择最小的一个不嫖了，有一点反悔贪心的味道 双倍经验：<a href="https://codeforces.com/problemset/problem/1251/E2" target="_blank" rel="noopener nofollow">Voting</a>

<strong>费用流的本质就是一个可悔贪心</strong>。费用流既然会了，那么就有凸性质，那么就可以 wqs 二分。

<a href="https://www.luogu.com.cn/problem/CF802O" target="_blank" rel="noopener nofollow">CF802O April Fools' Problem (hard)</a> 详见 2022 10 月总结。

<h3 id="翻折引理">翻折引理</h3>

可以理解为卡特兰数的扩展

在不越过 $y=x+1$ 的限制下从 $(0,0)$ 到 $(n,n)$ ，每次向右向上走，合法方案数是 $\binom n{2n}-\binom {n-1}{2n}$，理解为所有不合法路径其实是走到了 $(n-1,n+1)$ ，即 $(n,n)$ 对称点

同理可以知道走到 $(i,n)$ 的方案数等

简单的应用：<a href="https://www.fzoi.top/problem/7320" target="_blank" rel="noopener nofollow">CF1204E Natasha, Sasha and the Prefix Sums</a>

厉害的应用：<a href="https://www.fzoi.top/problem/7323" target="_blank" rel="noopener nofollow">[JLOI2015] 骗我呢 - 题目 - FZUOJ (fzoi.top)</a>

<a href="https://www.fzoi.top/problem/5914" target="_blank" rel="noopener nofollow">#5914. a 酱的 dq</a> 蛇皮推 DP 之后发现折线法

<blockquote>

翻折引理 plus：若现在的限制不是某条直线的一侧而是在某两条直线中间，可以考虑其跨直线的情况：假设其跨过第一条直 线叫事件 A，跨过另一条直线为事件 B，考虑如何把事件 ABAAABABAABABABB... 容斥掉。我们首先先把 A/B 连续段变成单 个的 A/B，再考虑事件 ABABABAB... 怎么做：假设一条线是 $y=-a$，一条线是 $y=b$，则事件 A 相当于以 $y=-a$ 为对称轴翻折，AB 相当于以 $y=b$ 对 $y=-a$ 的对称直线，也就是 $y=-2a-b$ 为对称轴翻折，以此类推，一直到超出坐标轴限制（组合数值 = 0）最后答案就是 偶数次翻折方案数之和（包括不折）- 奇数次方案数之和。这样的话，时间复杂度是 $O(\frac n{a+b})$，其中 $n$ 是总步数。也不适用于所有直线。

--gtr

</blockquote>

<a href="https://www.fzoi.top/contest/109/problem/5903" target="_blank" rel="noopener nofollow">游走 - 题目 - FZUOJ (fzoi.top)</a>

<h3 id="楼房重建线段树兔兔线段树">楼房重建线段树/兔兔线段树</h3>

<a href="https://www.luogu.com.cn/problem/P4198" target="_blank" rel="noopener nofollow">楼房重建</a> 每次 pushup log 递归看左区间对右区间影响

<a href="https://www.fzoi.top/problem/5916" target="_blank" rel="noopener nofollow">#5916. c 酱的二分图</a> 注意这道题要求的不是斜率，只要高度大于就行

<a href="https://www.luogu.com.cn/problem/P4425" target="_blank" rel="noopener nofollow">[HNOI/AHOI2018]转盘 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)</a>

CF1340F Nastya and CBS：考虑一个区间如若被消成存在 <code>{)</code> 那么无论外面怎么搞都不合法。那么一个子区间应该可以被处理成 <code>)))]]}})(((({[[[[</code> 这类形式。于是一个做法是分块维护 左边的右括号 和 右边的左括号 的 hash 值，每次匹配一下。更优秀的做法是利用 楼房重建线段树 的思想，在递归过程中，计算出左区间对右区间的影响，然后进一步递归右区间即可。

<strong>楼房重建线段树 = 线段树上二分 + 区间修改</strong>，有些时候，可以直接用楼房重建线段树代替后面二者的操作，可以大大节省码量。当然也可以二者互换，等式左边的方法思维难度更低。

具体的，楼房重建线段树的关键在于处理左区间对右区间的影响，为了处理这个影响，可能会记录一个 out[rt] 表示 rt 溢出的贡献个数，兔兔的方法是暴力往右子树递归算答案而等式右边的方法则是把具体的影响范围每次都直接算出来，然后配合区间修改做。

[USACO23FEB] Hungry Cow P：使用楼房重建，线段树每一个节点维护 cnt, out, ans 表示区间内 1 的个数，溢出了多少个 1，溢出的这些 1 加入 rs，rs 的答案；或者使用线段树分治+线段树上二分+区间覆盖。前者显然全方位暴打后者，后者小心可持久化不要 pushdown，小心空间问题。

<h3 id="黑白染色">黑白染色</h3>

每次跳 (r, c) 也是二分图。转成二分图之后，更容易考虑性质。

<a href="http://qoj.ac/problem/2213" target="_blank" rel="noopener nofollow">QOJ 2213 Knight</a>

<h3 id="树高-log">树高 log</h3>

提供了暴力跳 fa 的条件

<a href="https://www.fzoi.top/problem/2958" target="_blank" rel="noopener nofollow">「BZOJ3730」震波</a> 点分树模板，每次修改直接暴力跳虚父亲

<a href="https://www.luogu.com.cn/problem/P4618" target="_blank" rel="noopener nofollow">[SDOI2018]原题识别 - 洛谷 </a> <a href="https://www.cnblogs.com/wyb-sen/p/15534369.html" target="_blank">题解</a>

[BALTICOI 2016] 交换 2022 10 月总结。

<a href="https://codeforces.com/gym/102220/problem/A" target="_blank" rel="noopener nofollow">GYM Apple Business</a> 点进去看官方题解把。。。

<h3 id="叶子节点稀少">叶子节点稀少</h3>

<a href="https://www.luogu.com.cn/problem/P3346" target="_blank" rel="noopener nofollow">[ZJOI2015]诸神眷顾的幻想乡 </a> 发现如果### 条链的话直接 SAM 即可，多条直链考虑广义 SAM 即可。考虑 <strong>化曲为直</strong> ，枚一个叶子作为根，对每个点到根的串建立广义 SAM（SAM 维护 Trie 树）

<h3 id="交换答案状态">交换答案状态</h3>

在 DP 值域小而状态数量大的情况下，我们可以考虑交换答案和状态然后转成判定性问题

AGC033D Complexity

<a href="https://www.luogu.com.cn/problem/P7562" target="_blank" rel="noopener nofollow">[JOISC 2021 Day4] イベント巡り 2 (Event Hopping 2) </a> 容易想到策略就是尽量从 1 到 n 选，然后在过程中判断是否需要按照最优方案选而不是按字典序选才能满足条件。考虑在判断的时候实际上就是在计算区间 $l,r$ 中最多能选多少个区间，容易想到区间 DP $f[l][r]$ ，但是会 TMLE 双飞。考虑答案和状态交换，设 $f[l][i]$ 表示从 $l$ 开始选 $2^i$ 个区间到达的最近位置，那么有 $f[i][0]\leftarrow f[i+1][0]$ ，$f[i] [j]\leftarrow f[i+1][j],f[f[i][j-1]][j-1]$ 。然后具体计算 $l,r$ 中能选多少个区间的时候倍增跳一下即可。

<a href="https://www.fzoi.top/contest/132/problem/5665" target="_blank" rel="noopener nofollow">[Atcoder DP E]Knapsack 2</a> 超大背包

<a href="https://www.fzoi.top/problem/5704" target="_blank" rel="noopener nofollow">搭建双塔</a>

<blockquote>

给你𝑁个分别为ℎ𝑖的木块，看是否能堆出两个高度相同的塔，并让其高度尽可能大<br>

𝑓 𝑖 𝑗 表示前i个木块，两塔高度差为j的两塔的高度和的最大值（知道差与和，即可求出两个塔的值

</blockquote>

<a href="https://www.fzoi.top/problem/5659" target="_blank" rel="noopener nofollow">[USACO16OPEN]262144 P</a> 经典高级状态优化，dp[l][r] = i, 变成 dp[l][i] = r，本题中体现为最近的右端点。

<h3 id="n个1-1的序列随机后前缀和极值范围-">n个+1-1的序列随机后前缀和极值范围 $O（\sqrt n）$</h3>

这个居然还真有用...太邪恶了

<a href="https://www.luogu.com.cn/problem/P9047" target="_blank" rel="noopener nofollow">[PA2021] Poborcy podatkowi</a> 优化 DP 状态个数，随机后只有 $\sqrt n$ 个

<h3 id="全局平衡二叉树">全局平衡二叉树</h3>

比如，对于点到根路径加法，点到根查询，传统的树链剖分需要 log^2，利用全局平衡二叉树，可以做到 log。

<a href="https://www.luogu.com.cn/problem/P4211" target="_blank" rel="noopener nofollow">LNOI 2014 LCA</a>

由此，还可以优化 DDP 问题。

<a href="https://www.luogu.com.cn/problem/P4719" target="_blank" rel="noopener nofollow">模板 动态 DP</a>

<h3 id="可行性-dp-状态压缩">可行性 DP 状态压缩</h3>

形如 f[i][s] = 0/1 ，直接压成 f[i] = x，x 的二进制第 s 位表示之前的 dp[i][s]。显然这对 s 有要求，对转移方式有要求。

<a href="https://www.fzoi.top/problem/6729" target="_blank" rel="noopener nofollow">佛怒火莲</a> 找题解去吧/hsh

<a href="https://atcoder.jp/contests/abc278/tasks/abc278_f" target="_blank" rel="noopener nofollow">ABC278F</a>

CF1886E I Wanna be the Team Leader

<h3 id="连续段-dp">连续段 DP</h3>

每次插入元素时，对三类转移方式进行分类讨论：将插入的元素作为一个新连续段插入，将元素插入至一个已有连续段的两端，将元素用于合并两个连续段。

<a href="https://www.cnblogs.com/chroneZ/p/17299874.html" target="_blank">blog</a>

<a href="https://www.luogu.com.cn/problem/P5825" target="_blank" rel="noopener nofollow">排列计数</a>

<h3 id="扩展-lucas-定理推论">扩展 Lucas 定理推论</h3>

$\binom nm \bmod 2 = [m\subseteq n]$

常常用于推式子，注意不仅可以正用，而且可以<strong>反着来</strong>

CF1770F Koxia and Sequence

第一个观察是 n 是偶数 ans = 0，证明考虑对称性，swap(a[i],a[n - i + 1]) 可以得到另一组解；对于 n 是奇数的情况，可以认为贡献均发生在 a[1]（将 2...n 做对称），自然想到枚举 a[1] 的二进制第 b 位，计算合法的方案数的 <strong>奇偶性</strong>，以下假装 a1 被抠掉了 2^b。

考虑把 y 的限制容斥掉，即假设 f(y) 表示 $\sum_{\sum_a = x - 2^b} [a_1 \in (y\oplus 2^b)][\forall {i&gt;1}, a_i\in y]$，有 $ans = \sum_{y^{\prime}\in y} (-1)^{|y| - |y^{\prime}|}f(y^{\prime})$，只关心异或，所以 $ans = \oplus f(y^{\prime})$。只需求出 f(y)，考虑 <strong>逆用 Lucas 定理的推论</strong>，由于只关心奇偶性，将 $[a\in y]$ 变成 $\binom {y}{a} \bmod 2$，可以发现 f(y) 的式子变成了 <strong>多元范德蒙德卷积</strong> 的形式，结束。

<h3 id="莫队递推-">莫队递推 $f_{n, m}$</h3>

即对于一些行与行之间，列与列之间存在递推关系的 $f$，例如 $\binom nm = \binom {n - 1}m + \binom {n - 1}{m - 1}, \binom nm = \binom n{m - 1} \times \frac{n - m + 1}m$，那么实际上，如果我维护出 $n, m, (\binom nm, \binom n{m + 1})$，<s>大概</s>可以 $O(1)$ 得到 $n\to n \pm 1$ 或 $m\to m\pm 1$ 之后的 tuple，此后可以转成莫队维护

莫队维护存在一个细节，就是时刻要保证 右端点&gt;左端点，这个细节在某些诸如维护出现次数的情形下，是不重要的，但是在这里务必关系，因为可能不合法。

[GDKOI2023DAY1 T2] 换一种计算 $f_{n, m}$ 的方式，上述做法相当于是枚举了这 m 个自由位，有 j 位的 p_i = 某一个自由位，m - j 位的 p_i = 非自由位；实际上这样是相当愚蠢的，因为没有很好地体现自由位的“自由”，即枚举的 j 个当中，有一部分实际上也是错排；故而，更好的枚举的方式是枚举有 j 个自由位 pj = j，然后让剩下的 n - j 个数字做错排。

设 $g_i = (i - 1)(g_{i - 1} + g_{i - 2})$ 表示错排方案数。


$$
f_{n, m} = \sum_{i = 0}^m\binom mi g_{n - i}

$$

把组合数拆开，可以得到：$f_{n, m} = f_{n - 1, m - 1} + f_{n, m - 1}$

考虑使用莫队，把 m 看成行，n 看成列，想着如果知道的每行相邻数字如何递推就好了；即考虑推导出 $f_{n, m} = \sum \lambda f_{n-\star, m}$ 的形式

结合 $g_i$ 的递推式，中途利用吸收恒等式 $\binom mi i = m\binom{m-1}{i-1}$，可以得到


$$
f_{n, m}=(n-1)f_{n - 1, m} + (n - m - 1)f_{n - 2, m}

$$

<h3 id="组合数转二维平面走路径">组合数转二维平面走路径</h3>

众所周知 $\binom {n + m}n$ 的组合意义是从 $(0, 0)$ 走到 $(n, m)$ ，每次向右向上的方案数。

<a href="https://www.fzoi.top/problem/7317" target="_blank" rel="noopener nofollow">arc110d</a>

<a href="https://www.fzoi.top/problem/7319" target="_blank" rel="noopener nofollow">agc001e BBQ Hard</a>

<hr>

二项式反演其实也可以被看成是二维平面走路径的问题。

<a href="https://www.luogu.com.cn/problem/solution/CF1750G" target="_blank" rel="noopener nofollow">Doping</a> <a href="https://www.luogu.com.cn/blog/Troverld/solution-cf1750g" target="_blank" rel="noopener nofollow">xtx</a>

<h3 id="拆组合数">拆组合数</h3>

利用 $\binom nm = \binom {n - 1}m + \binom{n - 1}{m - 1}$，进行 DP/推柿子

推柿子：CF932E	Team Work

DP：Crash 的文明世界

<h3 id="数颜色">数颜色</h3>

对于一棵树，求一棵子树内是否有某种颜色，可以将所有有此颜色的点按 dfs 序排序，每个点打标记 +1，每相邻两个点的 lca 打标记 -1，答案即为子树内标记和

当然还可以线段树合并/dsu on tree/转成 dfn 区间之后上莫队/主席树之类之类。

<a href="https://www.luogu.com.cn/problem/P1972" target="_blank" rel="noopener nofollow">[SDOI2009]HH的项链 </a>  莫队/主席树维护 pre /<a href="https://www.luogu.com.cn/blog/_post/10524" target="_blank" rel="noopener nofollow">树状数组</a>

<a href="https://www.luogu.org/problem/P1903" target="_blank" rel="noopener nofollow">数颜色 / 维护队列</a> 带修莫队/树状数组套主席树

<a href="https://www.luogu.com.cn/problem/P2486" target="_blank" rel="noopener nofollow"> P2486 [SDOI2011]染色 </a> 区修，树上数颜色段：线段树区间合并颜色段个数

<a href="https://www.luogu.com.cn/problem/U41492" target="_blank" rel="noopener nofollow">U41492 树上数颜色</a> DSU on Tree

<h3 id="虚树">虚树</h3>

上面数颜色的方法仅仅用来数颜色未免有杀鸡牛刀之嫌。

<blockquote>

对于一棵树，求一棵子树内是否有某种颜色，可以将所有有此颜色的点按 dfs 序排序，每个点打标记 +1，每相邻两个点的 lca 打标记 -1，答案即为子树内标记和

</blockquote>

实际上可以发现这个其实衍生于常见的建立虚树的方法：即把每一个点按照 dfn 排序，加入 lca，再排序。然后求每一个点虚树上父亲。

上述计算贡献的方法就是树上差分。

但是注意到这样一个事情，这个方法实际上提供了一种不需要显式的建立虚树，就能够维护贡献的方式，具体的，设有 $k$ 个关键点已经按照 dfn 排序，那么 $k$ 条树上路径 $(a_i,a_{i+1\bmod (k+1)+1})$ 刚好把虚树上每一个点覆盖 2 次。

证明考虑每一个点和父亲的边被经过的次数，对于子树内的关键点，显然只需要关心最开始和最末尾这两个（当然也可能是同一个），因为中间的点的树上路径都在子树内。所以刚好就是两次。

那么这个贡献大概是可以用数据结构维护的，比如维护一个 set 存所有的关键点，就可以支持每次插入/删除一个关键点之后的贡献而并不需要把虚树显式重建。

[QOJ7610] Bus Line 。

<h3 id="决策单调性">决策单调性</h3>

<a href="https://www.cnblogs.com/birchtree/p/12937975.html#%E7%94%A8smawk%E7%9A%84reduce%E5%87%8F%E5%B0%91%E6%97%A0%E7%94%A8%E5%86%B3%E7%AD%96" target="_blank">决策单调性</a> 讲的很全面

一般的，使用单调队列+二分可以解决问题

<a href="https://www.fzoi.top/problem/1985" target="_blank" rel="noopener nofollow">NOI2009 诗人小G</a> 经典例题

有时候，决策单调性的使用需要对题目有更深入的分析

<a href="https://www.luogu.com.cn/problem/P5504" target="_blank" rel="noopener nofollow">JSOI2011 柠檬</a> <a href="https://www.cnblogs.com/wyb-sen/p/16289933.html" target="_blank">题解</a>

在转移仅存在于层于层之间的情况下，我们可以使用整体二分来找决策点

<a href="https://www.luogu.com.cn/problem/CF321E" target="_blank" rel="noopener nofollow">CF321E Ciel and Gondolas</a> RT，直接上整体二分找决策

<a href="https://www.luogu.com.cn/problem/CF1527E" target="_blank" rel="noopener nofollow">CF1527E Partition Game</a> 详见 2022 10 月总结。

在转移都在当前层，且贡献 f(l, r) 的计算方式只适合用类似莫队的方法求解时，即此时需要套用转移存在于层和层之间的方法，那么可以通过在外面套一个 cdq 分治的方法来手动实现“分层”

（莫队方法的时间复杂度的保证在于指针移动次数被递归过程的区间长度限制，区间长度总和是 nlogn 的，假设指针一次移动复杂度是 O(1)，那么复杂度就是 O(nlogn)）

具体的：

CDQ(l, r):<br>
CDQ(l, mid);<br>
solve(mid + 1, r, l, mid);<br>
CDQ(mid + 1, r);

solve(l, r, ol, or) 表示计算 f[l...r]，决策点在 [ol...or]。solve 的复杂度是区间长度乘上log，那么总复杂度就是 nlog^2n

<a href="https://www.fzoi.top/problem/6895" target="_blank" rel="noopener nofollow">20230203 T3</a> 外面使用 wqs 二分，里面套上述方法，有 log^3 的做法。

在贡献不能删除只能撤回（回滚）时，有一种双指针配合决策单调性分治的方法（整体二分）

「C.E.L.U-03」布尔 详见 2022HL集训总结

<h3 id="线段树分治">线段树分治</h3>

线段树分治通常结合可撤销并查集使用，而且感觉一般可以线段树分治的那么就可以整体二分。

线段树分治其实感觉更像是一种思想而不是 DS。

<a href="https://www.luogu.com.cn/problem/P5787" target="_blank" rel="noopener nofollow">板子题</a>

一类奇怪的线段树分治：<a href="https://www.luogu.com.cn/blog/command-block/ds-ji-lu-p7843-celu-03-bu-er" target="_blank" rel="noopener nofollow">「C.E.L.U-03」布尔</a>

线段树分治的关键在于发现某一个信息的影响时间是一个区间，体现这个思想的是<a href="https://www.luogu.com.cn/blog/_post/320229" target="_blank" rel="noopener nofollow">CF603E Pastoral Oddities</a>

<h3 id="矩阵快速幂">矩阵快速幂</h3>

常见的，在优化 DP 的时候，对于常系数的情况，可以使用矩阵快速幂

矩阵乘法有这样的优化技巧：

<ul>
<li><s>常系数线性递推</s></li>
<li>矩阵值 01 $\to $ bitset</li>
<li>向量乘矩阵平方复杂度，预处理 $T^{\lambda base^x}$  其中 $\lambda\in[1,base)$。</li>
<li>矩阵 A $n\times k$ ， B $k\times n$ 。$AB$ 得到 $n\times n$ ，$BA$ 得到 $k\times k$</li>
<li>int128 规避取模（卡常）</li>
</ul>

<a href="https://www.luogu.com.cn/problem/P3758" target="_blank" rel="noopener nofollow">TJOI2017 可乐</a> 模板

当然，我们还可以借鉴动态 DP 的思想，重定义一下矩阵乘法

<blockquote>

有类问题是这样的：边有边权，求从 $i$ 出发经过恰好 $k$ 条边走到 $j$ 的最长路，对这种题可以想到 DP。

设 $dp_{k,i,j}$表示从 $i$ 出发经过恰好 $k$ 条边走到 $j$ 的最长路，$G$ 为邻接矩阵，则有转移

$$

dp_{k,i,j}=\max_p\left\{dp_{k-1,i,p}+G_{p,j}\right\}



$$定义一个广义矩阵乘法

$$

C_{i,j}=\max_{k}\left\{A_{i,k}+B_{k,j}\right\}



$$把 $dp_k$ 看成矩阵，则上式可以改写为

$$

dp_k=dp_{k-1}\times G



$$因为这个广义矩阵乘法满足结合律，所以有

$$

dp_k=dp_0\times G^k



$$这样子就可以做到 $\mathcal{O}(n^3\log k)$

——<a href="https://www.luogu.com.cn/blog/_post/261968" target="_blank" rel="noopener nofollow">M_sea</a>

</blockquote>

<a href="https://www.luogu.com.cn/problem/solution/P6772" target="_blank" rel="noopener nofollow">NOI2020 美食家</a> <a href="https://www.cnblogs.com/wyb-sen/p/16288217.html" target="_blank">题解</a>

同时注意一件事情，对于这种 min/max + 卷积，可能可以使用决策单调性优化，从而使得矩阵乘法的复杂度变成 $O(n^2)$

<a href="https://www.luogu.com.cn/problem/P8864" target="_blank" rel="noopener nofollow">P8864 「KDOI-03」序列变换</a> 这道题同时还提示了这样一件事情：对于形如 $f_{i,j,h-1}\times coef_{j+1,k}\to f_{i,k,h}$ 的转移，经过略微调整之后可以使用矩块

<h3 id="-矩阵乘向量">$O(n)$ 矩阵乘向量</h3>

众所周知，矩阵乘向量 $A\bar{v}$ 是 $n^2$ 的。联系 <a href="https://www.cnblogs.com/wyb-sen/p/17987321" target="_blank">CH定理</a> 的知识，考虑特征值和特征向量。

发现，如果把 $\bar v$ 叙述成 $f$ 的线性组合，不妨设 $\bar{v}=\sum w_if_i$，那么 $A\bar {v}=A\sum w_if_i=\sum w_i\lambda_if_i$，这样就是 $O(n)$ 了。

当然要求特征值是好求的。

[CF1540E] Tasty Dishes 题解在总结。

<h3 id="循环矩阵">循环矩阵</h3>

容易证明对于循环矩阵而言，加/减/乘之后都还是循环矩阵

而循环矩阵在知道了第一行之后可以知道之后所有行<br>
所以在转移的时候可以只做第一行的转移，这样的话就是 $O(n^2)$ 的

<a href="https://blog.csdn.net/jaihk662/article/details/76571649" target="_blank" rel="noopener nofollow">https://blog.csdn.net/jaihk662/article/details/76571649</a><br>
对于其中为什么是 $(i+j-2)\%n+1$ ，因为 $a[i][j]$ 其实对应的是 $a[1][j-i+1]$

<a href="https://www.fzoi.top/problem/5718" target="_blank" rel="noopener nofollow">BZOJ2510 弱题</a> RT

<h3 id="字符串问题">字符串问题</h3>

我们考虑 SA/SAM/ACAM/KMP 解决这类问题

<a href="https://www.luogu.com.cn/problem/SP1811" target="_blank" rel="noopener nofollow">SP1811     LCS - Longest Common Substring    </a> 最长公共子串 SAM裸题

<a href="https://www.luogu.com.cn/problem/SP10570" target="_blank" rel="noopener nofollow">SP10570     LONGCS - Longest Common Substring   </a> 对于 SAM 上每一个点求出 mn[i] 表示最短的最长匹配

<a href="https://www.luogu.com.cn/problem/P2408" target="_blank" rel="noopener nofollow">P2408     不同子串个数</a> SAM $\sum len(u)-len(fa)$ /SA $\sum sa[i]-height[i]$

<a href="https://www.luogu.com.cn/problem/P3181" target="_blank" rel="noopener nofollow">[HAOI2016] 相同子串个数</a> <a href="http://poj.org/problem?id=3415" target="_blank" rel="noopener nofollow">POJ3415 -- Common Substrings</a>

广义 SAM 每一个节点分别求一下 $siz[0],siz[1]$ 表示在不同串中出现次数，答案 $\sum (len[u]-len[fa])*siz[u][0]*siz[u][1]$

本质是算 <strong>所有后缀的 $lcp$ 长度之和</strong> ，后缀来自两个不同字符

可考虑容斥，先把两个字符串拼起来算一遍贡献，然后减去每一个字符串单独算的贡献

问题变成计算所有后缀的 lcp 长度之和，单调栈维护 cnt "合并”即可

或者是直接计算，分A串的子串在前、B的子串在前两种情况分别用单调栈求出答案 ，每次合并的时候只在处于不同子串时 cnt++ ，具体可见 <a href="https://www.luogu.com.cn/blog/_post/23398" target="_blank" rel="noopener nofollow">  </a>

<a href="https://www.luogu.com.cn/problem/P1368" target="_blank" rel="noopener nofollow">【模板】最小表示法   </a> 对 S+S 建立 SAM 贪心走 /SA 板子，第一个 sa[i]&lt;n 的位置开始

<a href="https://www.luogu.com.cn/problem/SP8093" target="_blank" rel="noopener nofollow">SP8093 JZPGYZ - Sevenk Love Oimaster</a> 给定 $n$ 个模板串，以及 $m$ 个查询串，依次查询每一个查询串是多少个模板串的子串

SAM：对模板串建立 SAM，考虑 parent tree 。预处理出每一个等价类答案，可使用 线段树合并/子树数颜色

ACAM：对查询串建立 ACAM，丢模板串进去，转化为子树数颜色，用<a href="https://www.luogu.com.cn/blog/_post/379547" target="_blank" rel="noopener nofollow">树状数组</a>/莫队解决

其实这道题还可以根号分治

<a href="https://www.luogu.com.cn/problem/P6640" target="_blank" rel="noopener nofollow">[BJOI2020] 封印</a> 每次询问 $s[l \dots r]$ 和 $t$ 的最长公共子串长度

对 t 建立 SAM，求出对于 $s_i$ 的最长匹配长度 $l[i]$ ，即 $s_{i-l[i]+1}\dots s_i$ 有匹配

每次即问 $\max _{k\in [l,r]} \min(r-l+1,l[k])$  ，考虑干掉 min 即可

<a href="https://www.luogu.com.cn/problem/CF235C" target="_blank" rel="noopener nofollow">CF235C Cyclical Quest </a> 给定一个主串 $S$ 和 $n$ 个询问串，求每个询问串的所有循环同构在主串中出现的次数总和

对 S 建立 SAM，发现循环同构可以理解为前面扣掉一个字符，加到后面去。前面扣字符对应跳后缀链接，后面加对应匹配。

去重直接用 vis 标记一下即可

<a href="https://www.luogu.com.cn/problem/P2852" target="_blank" rel="noopener nofollow">[USACO06DEC]Milk Patterns G</a> 二分答案，检查 Height数组中所有长度为 k 的区间（O（n）个查询是否合法）是否满足LCP大于等于mid

<h3 id="解耦思想--两个-dp-互相转移">解耦思想 / 两个 DP 互相转移</h3>

<a href="https://www.jianshu.com/p/5543b2eee223" target="_blank" rel="noopener nofollow">什么是解耦</a>

解耦思想可以用于循环的优化。

对于一类枚举 i,j 的问题，j 的枚举受到 i 的影响（耦合）然后计算某种贡献的问题。

为了优化复杂度，可以想到求出在固定 i/j 时所有 j/i 的贡献（可能交换枚举顺序），此时我们面对的实际上是一个范围更小的类似的问题，可以新设一个数组，两个数组交替转移（解耦）

<a href="https://atcoder.jp/contests/abc252/tasks/abc252_g" target="_blank" rel="noopener nofollow">ABC252G Pre-Order</a> 发现问题在于接上一个儿子的合法性耦合太大，于是新设一个 dp 解耦。

<a href="https://www.fzoi.top/problem/6422" target="_blank" rel="noopener nofollow">20220921 战斗</a> dp[l,r,x] ，优化：设 f[l,r] 表示 l 赢，g[l,r] 表示 r 赢，状态数量优化。 考虑 f[l,r] 转移需要枚举分割点和另一部分的胜者，二者耦合很大。考虑先枚举另外一部分的胜者，用 h[l,r] 表示 l 胜或 r 胜的分割点方案数，那么 f[l,r] = \sum h[l,x]f[x,r]win(l,x)， win(a, b) 表示 a 胜 b 的概率，确认 x 胜利。那么能不能先枚举分割点？不能，需要算 win。

解耦思想其实在一类优化建图里面也可以用到。

[SNOI2019]通信 本来是两重 for 枚举 i，j 连边，这里单独拉出一条数轴出来，作为一个“缓冲区”，就很对味。

<h3 id="倍增">倍增</h3>

<a href="https://www.luogu.com.cn/blog/ShadowassIIXVIIIIV/solution-p3295" target="_blank" rel="noopener nofollow">倍增法</a>

倍增法不仅局限于快速幂，后缀数组，ST表，求lca之类的特定算法，还可以用来解决一类特定问题

倍增法可以优化空间，有时候，如若存在修改查询时间复杂度不平衡的情况，可以考虑倍增预处理一手

倍增源于两个数学等式

<ul>
<li>
$2^n=2^{n-1}+2^{n-1}$
</li>
<li>
$N=\sum (p_i\times 2^i) [i\in (0,+∞)] $ 其中 $pi$ 为 $N$ 的二进制第 $i$ 位
</li>
</ul>

而倍增法的唯一要求的是，被称之为“+”的运算满足结合律(有些人也成操作满足可合并性)

注意不要倍增傻了，如若“+”就是加法，直接前缀和就好了，倍增你马呢

（倍增傻的典型案例：<a href="https://www.luogu.com.cn/problem/P3948" target="_blank" rel="noopener nofollow">P3948 数据结构</a> 想到后面的 Final 询问很多，于是考虑处理 $ans[i][j] $ 表示 $[i,j]$ 内答案，发现 $O(n^2)-O(q)$ 不是很科学，复杂度不平衡，于是考虑倍增，$O(n\log n)-O(q\log n)$ ，但其实直接前缀和就好了哈哈）

具体来讲，倍增法用到的地方一般是，我们现在要询问/修改一些东西，然后我们发现操作具有合并性，之后呢，我们对于每个大小为 $2^i$ 的东西预处理出来一个答案，等到询问的时候，就将询问的东西大小用二进制表达，将预处理出来的每个东西合并起来就得到了答案

常用的两类二进制拆分代码

```C++
for(int i=0;p;p&gt;&gt;=1,i++){/*do sth*/}
for(int i=20;p&gt;0;i--){if(p&gt;po[i]){p-=po[i]}/*do sth*/}
```

其中上面一个适用于 $p$ 已知的情况(比如倍增求 lca，跳到深度相等的那段代码)

下面的一个适用于 $p $ 未知，但是可以比较 $p$ 和某个值的大小，以及可以做相减运算 (比如 lca 第二段，两个点一起向上跳，但是不知道 lca 的深度的情况，但是我们用这个代码在未知 lca 深度的情况下对 lca 深度做了二进制拆分)

<a href="https://www.luogu.com.cn/problem/P3295" target="_blank" rel="noopener nofollow">SCOI2016 萌萌哒</a> 区间对应点连边，问连通块个数：易知 $O(n^2)$ 暴力并查集连边，$O(n)$ 查询，考虑 <strong>平衡修改查询复杂度</strong>  ，将一个点拆成 $\log n$ 个点，代表从点 $i$ 开始，长度为 $2^k$ 的子串。查时，推标签即可

对于倍增在 lca 上的应用，在跳 lca 的时候也可以同时记录路径上的信息

比如，询问树上两个点路径上最大值，一般的想法是 $log^2n$ 的树剖，但在静态的背景下，也可以直接倍增

<a href="https://www.luogu.com.cn/problem/P4616" target="_blank" rel="noopener nofollow">[COCI2017-2018#5] Pictionary</a> 让 $m-i+1$ 和它的倍数连边（联通就不连了），形成一棵树。问题转化为路径两点最大值。<a href="#%E5%86%B7%E6%88%98" rel="noopener nofollow">类似</a> ，区别在于它不是静态的，是动态树 <a name="[COCI2017]Pictionary"></a>

<hr>

对于一些静态最大值问题，我们可以考虑倍增法解决，或者说套在 DP 上

虽然有时候这个所谓的最大值其实是一个 DP 的形式，但是注意到 ST 表的本质其实就是 DP

<a href="https://www.fzoi.top/problem/5598" target="_blank" rel="noopener nofollow">[CCC2019]Triangle: The Data Structure - 题目 - FZUOJ (fzoi.top)</a> 设 $f[i][j][k]$ 表示以 $i,j$ 为左上角， 大小 $k$ 的三角形最大值，$f[i][j][k]=\max \{f[i][j][k-1],\max\{f[i+(1&lt;。观察转移，发现 $f[i][j]$ 和 $f[i][j+1]$ 转移的差距不大，滑动窗口即可

<hr>

一个 DP 技巧叫做<strong>交换答案和状态</strong>，通常用于解决空间开不下的问题。现在告诉你的是，交换过后，新状态（原答案）可以考虑使用 $2$ 的幂次的形式进一步优化时间/空间复杂度

<a href="https://www.luogu.com.cn/problem/P7562" target="_blank" rel="noopener nofollow">[JOISC 2021 Day4] イベント巡り 2 (Event Hopping 2) </a> 容易想到策略就是尽量从 1 到 n 选，然后在过程中判断是否需要按照最优方案选而不是按字典序选才能满足条件。考虑在判断的时候实际上就是在计算区间 $l,r$ 中最多能选多少个区间，容易想到区间 DP $f[l][r]$ ，但是会 TMLE 双飞。考虑答案和状态交换，设 $f[l][i]$ 表示从 $l$ 开始选 $2^i$ 个区间到达的最近位置，那么有 $f[i][0]\leftarrow f[i+1][0]$ ，$f[i] [j]\leftarrow f[i+1][j],f[f[i][j-1]][j-1]$ 。然后具体计算 $l,r$ 中能选多少个区间的时候倍增跳一下即可。

<hr>

没想到吧，还可以 <strong>倍增优化矩阵乘法</strong>

具体就是说在需要做多次转移的情况下，对于转移矩阵 $G$ ，预处理 $G^{1&lt;，然后可以 log 算 G^x

<a href="https://www.luogu.com.cn/problem/P6569" target="_blank" rel="noopener nofollow">NOIOL 魔法值</a> RT

<a href="https://www.luogu.com.cn/problem/solution/P6772" target="_blank" rel="noopener nofollow">NOI2020 美食家</a> <a href="https://www.cnblogs.com/wyb-sen/p/16288217.html" target="_blank">题解</a>

<a href="https://www.luogu.com.cn/problem/CF576D" target="_blank" rel="noopener nofollow">CF576D Flights for Regular Customers</a> 不要受限于矩阵维护最小值的思路，本题中是矩阵维护可达性。发现存在一些特殊转移，且特殊转移对后续都有影响的时候且修改幅度小，与 “美食家” 不同的是，美食家只是当前一次的特殊转移。因为修改幅度只有 log ，不影响复杂度，所以干脆直接根据新增边再原来 log 个转移矩阵的前提下得到新的转移矩阵。

<hr>

倍增维护字符串比较，动态在前面加一个字符啥啥的，这玩意是可以每次更新倍增数组的。

20230704 T3

<a href="https://www.fzoi.top/problem/7645" target="_blank" rel="noopener nofollow">括号 bracket</a>

<h3 id="值--x-的答案---x-的答案----x--1-的答案">值 = x 的答案 = (&gt;= x 的答案) - (&gt;= x + 1 的答案)</h3>

<a href="https://www.luogu.com.cn/problem/P8290" target="_blank" rel="noopener nofollow">省选联考2022 填树</a> 我们尝试枚举非 0 的节点的最小值，然后对应有一个较大值的上限，但存在一个问题：我们需要让至少一个非 0 节点取到最小值。尝试容斥，我们可以快速地解决求非 0 点权在[l,r] 之间的方案数（此时每个节点能取的范围就是 [lu ,ru] 与 [l,r] 的交），所以我们用 [l,r] 的方案数减去 [l+1,r] 的方案数，就可以得到取到最小值的情况

<h3 id="根号分治">根号分治</h3>

看到 $\sum\le 10^5$ 之类的，请自动联想根号分治

有时候是值域 $O(n)$ ，可以考虑值域分治

<a href="https://www.luogu.com.cn/problem/SP8093" target="_blank" rel="noopener nofollow">SP8093 JZPGYZ - Sevenk Love Oimaster   </a> <a href="https://www.luogu.com.cn/blog/_post/378067" target="_blank" rel="noopener nofollow">唐一文</a>

<a href="https://www.luogu.com.cn/problem/CF1422F" target="_blank" rel="noopener nofollow">Boring Queries</a> <a href="https://www.cnblogs.com/wyb-sen/p/15884893.html" target="_blank">题解</a>

<a href="https://www.luogu.com.cn/problem/CF862F" target="_blank" rel="noopener nofollow">CF862F Mahmoud and Ehab and the final stage </a> <a href="https://www.cnblogs.com/wyb-sen/p/15866694.html" target="_blank">题解</a>

<a href="https://www.luogu.com.cn/problem/CF914F" target="_blank" rel="noopener nofollow">CF914F Substrings in a String</a> 暴力的想法是每次重构 SAM，考虑分块，每一个块维护一个 SAM。考虑串长大于块长的不会太多，这部分直接重构；对于串长小于块长的，只涉及相邻两个块的重构，注意考虑块之间的贡献。取块长 $\sqrt n$ ，复杂度 $O(q\sqrt n)$

一些数学题，若观察到当枚举值比较大，另外一个值范围缩小/可确定/有性质，那么也可以考虑小于某一个闸值的部分爆算，大于闸值的部分 推式子/找规律 等，这个闸值有时候会是 $\sqrt n$

<a href="https://www.luogu.com.cn/problem/P2425" target="_blank" rel="noopener nofollow">P2425 小红帽的回文数 - 洛谷</a>

对于涉及到质因子分解的问题，常常可以把质因子分成两部分，$&gt;\sqrtV$ 的质因子显然只有一个。此时常常可以拆成两部分计算。

<a href="https://www.luogu.com.cn/problem/P8292" target="_blank" rel="noopener nofollow">省选联考2022 卡牌</a>

<a href="https://www.fzoi.top/problem/6843" target="_blank" rel="noopener nofollow">20230127 T4 神奇的变换</a> 对于 &lt;= 1000 的质因子，可以暴力处理 pre[i][j] 表示前i个里面有多少个素数j。&gt; 1000 的质因子每个数最多一个，所以从 n 个队列排成一行的问题转化为一个序列的问题，后续可以使用根号算法。

还可以对数据进行分治，如若我有 2^{x} 的做法，又有 2^{m - x} 的做法，等价于我有 $2^{m / 2}$ 的做法

<a href="https://www.luogu.com.cn/problem/CF1336E2" target="_blank" rel="noopener nofollow">Chiori and Doll Picking (hard version)</a>

<a href="https://uoj.ac/problem/705" target="_blank" rel="noopener nofollow">黄忠庆功宴</a>

对于一些需要枚举调和级数的情况，可以强行把原本的 nlogn 干一部分，即对于 i 比较小的采用另外一种方法处理。

<a href="https://www.fzoi.top/problem/5152" target="_blank" rel="noopener nofollow">[UOJ33] 树上GCD</a>

<h3 id="完全-dag">完全 DAG</h3>

完全 DAG，使得每一个点的 SG 值不同。

CF1442F Differentiating Games 构造一个大小为 20 的完全 DAG，剩下的点连自环，使得没有完全 DAG 向其他点连的边，使得所有其他点联向完全 DAG 的点集不同。这样，通过分析，可以区分所有点。

<h3 id="欧拉序">欧拉序</h3>

求 lca

ETT

<h3 id="树的直径">树的直径</h3>

求法：dfs or bfs(边权为正)；dp 例题：[APIO2010]巡逻

动态树的直径：<strong>欧拉序</strong>+数据结构(线段树）<a href="https://www.luogu.com.cn/problem/P6845" target="_blank" rel="noopener nofollow">[CEOI2019] Dynamic Diameter</a>

子树内的直径：线段树([L,R]的直径可以由[L,Mid],[Mid+1,R]的直径端点组合得到) My Beautiful Madness

“扩展”子树内直径结论：例如 WC 通道，给树上挂一些虚点，直径结论任然适用。同时关注到此时，我根本不在意树的结构了，只关心直径的端点和求 dis 的手段。

Gym 104160G Meet in the middle：对于 Tree2 的点 u 的所有询问，实际可以理解是给 T1 中每一个点 v 多挂一个虚儿子，边权是 T2.dis(u, v)，然后求 T1 的直径端点。

显然不能暴力做，考虑 u 在 T2 上移动到某一个儿子 son，那么对于 son 子树中的节点集合 S，每一个 v\in S 在 T1 中的虚点边权 -w（注意 S 在 T2 中表现为离散的集合），子树外的虚点 + w。可以想到 <strong>线段树维护树上联通块直径</strong> 的套路，发现由于是两棵树，所以如果想要在 T1 上建立这样的线段树，每次 u-&gt;son 的时候稚只能憨憨地做 |s| 次单点修改；实际上可以在 T2 上建立线段树，对于线段树上某一节点，维护其 S 在 T1 中对应子集的直径端点，因为我们可以实现 T1.getdis，所以这是可以做到的。即我们不关心树的结构，我们只关心 dis，所以不必要在 T1 上建立线段树，因为根本没有用到树的结构信息。利用欧拉序求LCA，做到 nlogn。

<h3 id="树的重心">树的重心</h3>

<ul>
<li>树的重心最多两个 —— 无根树判同构</li>
<li>第二定义：$\sum dis$ 最小（自然定义）</li>
<li>每加入一个点，树的重心不变或者会往新加入的点方向挪一步</li>
<li>第一定义：树的重心满足每个子树大小&lt;=n/2 —— 化简问题 or 贪心 or 构造</li>
<li>第三定义：最深的子树大小 $\ge \frac{n}2$ 的点 CF1667E Centroid Probabilities（容斥）</li>
</ul>

动态换根维护重心：发现重心一定可以通过不断跳重儿子得到。那么动态维护重儿子的倍增数组就好。或者，如果你是天才，你可以暴力求出所有的答案。

<hr>

一个结论：写出树的 DFS 序，并且将 $i$ 重复 $a_i$（点权）次，那么最浅的带权重心的子树对应的区间一定包含这个序列的中位数（因为其长度应该大于等于总长一半，否则可以再往上走一步）。

与上面重心一定可以通过不断跳重儿子得到的结论恰恰相反，我们找到中位数之后，倍增往上跳就可以得到真正的重心。

这启示我们，我们想要找到重心，和找到重心子树内某一个点是等价的。

<a href="https://codeforces.com/gym/102759/problem/I" target="_blank" rel="noopener nofollow">Query On A Tree 17</a>

<h3 id="等价转化">等价转化</h3>

经典等价转化 / 改变枚举量：[20220718海亮NOI集训] 数数 详见 HL集训总结

<strong>普通树唯一对应一棵二叉树</strong>

<a href="https://atcoder.jp/contests/abc252/tasks/abc252_g" target="_blank" rel="noopener nofollow">ABC252G</a> <a href="https://atcoder.jp/contests/abc252/editorial/4511" target="_blank" rel="noopener nofollow">inszva</a>

<strong>DAG 自身和补图同时传递闭包只有 n! 个</strong>

我们随便拿一个排列 p 出来。对于 a &lt; b ，如果 a 在 p 中的位置比 b 靠后，那么就连边 (a, b) 。可以发现这恰好对应一个自身和补图同时具有传递性的 DAG 。容易知道这是一个双射

<a href="https://www.fzoi.top/problem/5901" target="_blank" rel="noopener nofollow">排列 - 题目 - FZUOJ (fzoi.top)</a>

<h3 id="势能法算期望">势能法算期望</h3>

这个方法非常强，可以做一大堆调来调去的问题，对于有些期望题可以起到激光剑打原始人的效果。大致思想就是构造一个势能函数，使得每一步调整前的势能和期望 = 调整后的势能和期望 + 1，这样就可以通过把终态的势能 - 初态的势能得到期望步数。

其实不 +1 也没有问题，只要是 +c（c 是常数）或者任何奇妙的东西都可以，只要他满足可以用终态的势能 - 初态的势能的值推算出期望步数就行。

注意势能法所用的推导有一些前后不是等价的，但势能法本质是个构造的过程，所以只要满足条件就可，不一定需要等价。

<a href="https://www.fzoi.top/problem/5232" target="_blank" rel="noopener nofollow">Slime and biscuits</a>

<a href="https://www.fzoi.top/problem/5235" target="_blank" rel="noopener nofollow">CF850 Rainbow Balls</a>


$$
\begin{align*}
\sum f(c_i) &amp;= 1 + \sum \dfrac{c_i(c_i - 1) + (s - c_i)(s - c_i - 1)}{s(s - 1)}f(c_i) + \dfrac{c_i(s - c_i)}{s(s - 1)}f(c_i+1)+\dfrac{c_i(s-c_i)}{s(s - 1)}f(c_i-1) \\
\Leftarrow s(s - 1)f(c_i) &amp;= c_i(s - 1) + c_i(c_i - 1) + (s - c_i)(s - c_i - 1)f(c_i) + c_i(s - c_i)(f(c_i+1)+f(c_i-1)) \\
0&amp;=c_i(s - 1) + (sc_i - c_i^2)(f(c_i + 1) - f(c_i)) + (sc_i - sc_i^2)(f(c_i - 1) - f(c_i))
\end{align*}

$$

设 $d_i = f(i) - f(i - 1)$，令 $d_0 = 0$，容易知道 d 的递推式，容易知道 f

注意到本题中 $s$ 可能会很大，O(s) 不被接受，不能直接利用递推式计算。但是稍微推一推式子可以发现 $f(s) = s(s - 1)$

<h3 id="势能分析法">势能分析法</h3>

<a href="https://www.fzoi.top/problem/5167" target="_blank" rel="noopener nofollow">20211112 给国的锟题 - 题目 - FZUOJ (fzoi.top) </a>XX Open Cup GP of Tokyo – A

考虑相邻的堆 A, B ，不妨假设 A 是小根堆，B 是大根堆。定义 它们的势能是 A, B 在 A, B 的值域的交内的元素个数，那么容 易发现一次替换就会使得势能至少减小 1 ，而势能减到 0 时必 然需要交换。 不过交换之后的合并操作对势能的影响是未知的，所以要这样分 析：设现在有 L 个大根堆和 L 个小根堆，相邻的配对，得到 L 组。只考虑同一组的势能，那么总势能不超过 m ，所以至少有 L/2 组的势能都不超过 2m/L 。那么做 2m/L 次操作，一个组 的交换只会把相邻两个组搞爆，所以至少有 L/6 个组能交换， 也就使得组数乘了 5/6 。因此复杂度是一个 log 。

对于区间操作，如果发现存在使得操作次数足够多之后会使得区间内的数趋近相同，那么可以设势能函数为区间内不同的数字个数，或称之为混乱度。常见的证明是每次递归会使得混乱度至少减少1，混乱度只会在那些不完全包含的节点增加。

<a href="https://www.cnblogs.com/xyz32768/p/12590112.html" target="_blank">[学习笔记]吉司机线段树 - epic01 - 博客园 (cnblogs.com)</a>

[P6792 [SNOI2020] 区间和 ](<a href="https://www.luogu.co" target="_blank" rel="noopener nofollow">https://www.luogu.co</a> m.cn/problem/P6792) 动态区间最大字段和

对于区间求max的问题，先使用segment tree beats将问题转化为每次给一个节点的最小值增加一个值，这部分时间复杂度为 $𝑂(𝑛log(𝑛))$。 接下来处理最大子段和，维护一个区间的整段和，最大前缀，最大后缀，最 大子段和，还需要分别记录这些值包含的区间最小值数量，方便进行区间最小值加

在区间加操作中，最大子段和等信息会发生更新。最大子段和一定是由一个最大前缀和一个最大后缀凑成。由于操作只有区间 max，每个节点的最大前缀、 后缀都单调变长。维护每个节点最大前后缀发生变更时，最小值应该增加为 多少，进行区间 max 的时候如果达到这个值就对这个节点重构。每个节点转变转移方式最多1次，因此最多重构 $𝑂(𝑛)$ 次。

upd 2022.9.1

推翻了单 log 结论，觉得 log^2 很对，因为达到阈值之后需要递归下去算一遍新贡献，对吉司机线段树的复杂度是有影响的

然后又分析了一下，又觉得单 log 是正确的。

影响在于 $mn &lt; v &lt; sub, v &gt; val$ 的时候，需要往下面递归，此时对吉司机的势能不会改变，但是发生了有复杂度的递归操作。

如果简单地认为，每一个点只会发生一次跳跃闸值的动作，为了找到这些位置需要 log 的额外时间，那么就是 log^2 便错了。考虑从一个点往下递归之后，会再次往下面递归当且仅当这个节点也发生跳跃闸值操作。

所以复杂度就是 吉司机的 nlogn + 跳 4n 次闸值所产生的递归，大常熟而已。

这个不完全对，具体见 2022 9 月总结。

————————

对于下取整除法/取模，每个数字只会操作 log 次。

<a href="https://www.luogu.com.cn/problem/CF702F" target="_blank" rel="noopener nofollow">CF702F T-Shirts</a> 详见 2022 8 月总结

对于 and/or ，每个数字也只会操作 log 次。对于其交错出现的情况，容易发现多次操作之后区间内的数字是趋近于相同的，可以根据这个性质来设置表示混乱度的势能函数，分析之后可以发现正确的复杂度。

Csacademy and-or-max 详见 2022 8 月总结。

2018 雅礼集训 A

<hr>

$andsum, orsum, gcd$ 的变化量是 $O(log)$

<a href="https://www.luogu.com.cn/problem/P8421" target="_blank" rel="noopener nofollow">[THUPC2022 决赛] rsraogps</a>

<h3 id="wqs-二分">wqs 二分</h3>

wqs 二分用来解决一类问题，该类问题一个突出的特征是答案具有<strong>凸性</strong>，（比如选类东西有贡献也有一定限制，求选 K 个该类物品时的最优解）并且这类问题一般在不要求选 K 个时能够较轻松地做出来。

wqs 二分可以使用整数二分，只要构造出合法解即可。

当然，遇到凸性的题目，还有一种类型是直接用 DS(eg 平衡树)大力维护凸函数（维护差分数组）。

<a href="https://www.cnblogs.com/llmmkk/p/15947407.html" target="_blank">wqs二分 - llmmkk - 博客园 (cnblogs.com)</a>

<a href="https://zhuanlan.zhihu.com/p/340514421" target="_blank" rel="noopener nofollow">wqs 二分学习笔记 - 知乎 (zhihu.com)</a>

<a href="https://www.fzoi.top/problem/5904" target="_blank" rel="noopener nofollow">#5904. 清兵</a> 列出暴力 DP 方程，稍微优化一下，发现凸性，wqs 二分降维即可

ARC168E Subsegments with Large Sums 有趣的凸性。需要主动去发掘一个具有凸性的参数。

<a href="https://www.luogu.com.cn/problem/P6821" target="_blank" rel="noopener nofollow">[PA2012]Tanie linie</a>

<a href="https://www.fzoi.top/problem/6895" target="_blank" rel="noopener nofollow">20230203 T3 括号</a>

Gym102331H Honorable Mention

<h3 id="坐标变化">坐标变化</h3>

$(x,y)\to (x+y,x-y)$ ，新坐标系下的切比雪夫距离即为原坐标系下的曼哈顿距离。

$(x,y)\to (\frac {x+y}2,\frac {x-y}2)$ ，新坐标系下的曼哈顿距离即为原坐标系下的切比雪夫距离。

<a href="https://www.luogu.com.cn/problem/P3964" target="_blank" rel="noopener nofollow">P3964「TJOI2013」松鼠聚会</a>（切比雪夫距离转曼哈顿距离

[[JOISC 2021 Day2] 道路の建設案 (Road Construction)](<a href="https://www.luogu.com.cn/problem/P756" target="_blank" rel="noopener nofollow">https://www.luogu.com.cn/problem/P756</a> 1) 曼哈顿距离转切比雪夫距离。求前 $k$ 大距离，先考虑求出第 $k$ 大的距离。考虑二分答案。发现曼哈顿距离同时涉及到 $x,y$ 两个维度，非常恶心，考虑转化成曼哈顿距离，现在，距离式子变成了 $\max(|x_1-x_2|,|y_1-y_2|)$ 。问题变成一个二维数点问题，即问有多少对点满足 $\max(|x_1-x_2|,|y_1-y_2|)\le dis$ ，以 $x$ 轴为第一关键字排序，考虑滑动窗口，把点插入 set 来处理 $y$ 轴。判定的时候可以同时得到答案。输出答案的时候有点小细节。

$|x|+|y|=\max\{|x+y|,|x-y|\}$ ，此时，二维平面上的随机游走可以看作 $x+y,x-y$ 分别同时等概率的 $+1$ 或 $-1$

<a href="https://www.fzoi.top/contest/109/problem/5903" target="_blank" rel="noopener nofollow">游走 - 题目 - FZUOJ (fzoi.top)</a>

$(x,y)\to(x-y,x+y)$ 坐标轴旋转 $45^{o}$ ，有利于处理一些同时需要操作 $x,y$ 轴的问题

<a href="https://www.fzoi.top/problem/5400" target="_blank" rel="noopener nofollow">[JOI2016Final]断层</a>

<h3 id="笛卡尔树">笛卡尔树</h3>

借助建立（广义）笛卡尔树，可以更加清晰地分析问题。

<a href="https://www.fzoi.top/problem/6895" target="_blank" rel="noopener nofollow">20230203 T3</a>

<a href="https://www.fzoi.top/problem/8284" target="_blank" rel="noopener nofollow">20240220 房间 room</a> 注意对于一个位置，其走的顺序是固定的，且实际上就是在不断尝试跳笛卡尔树上的父亲，同时不断获得限制。发现这个限制只和跳到的祖先有关，那么可以直接视作是每一个点对子树有一个限制，这个自然是很好做的。

<a href="https://www.fzoi.top/problem/8287" target="_blank" rel="noopener nofollow">20240221 决斗 duel</a> 和上面一题差不多，对于已经确定的一个序列，注意决斗的顺序也是固定的。只不过这里需要计数合法序列，也并不难，设一个 DP，在 DP 过程中构建笛卡尔树即可。

<a href="https://www.luogu.com.cn/problem/P7219" target="_blank" rel="noopener nofollow">[JOISC2020] 星座 3</a> 建立笛卡尔树后，考虑 DP 的过程是两个子树的合并，套线段树合并

<h3 id="dp-值数量少">DP 值数量少</h3>

假如一个 DP ，取值少或取值具有某种性质，我们显然不需要维护 dp 数组，只需要维护一些特殊位置就可以了

<a href="https://www.fzoi.top/problem/5902" target="_blank" rel="noopener nofollow">棋子 - 题目 - FZUOJ (fzoi.top)</a> 考虑 $d_i(j)$ 表示第 $j$ 步中，$a_i$ 是否获得了 $i − 1$ 的棋子，这里 $d_i$ 是一 个长为 $m$ 的 01 序列。容易倒着扫一遍预处理出 $d_1$。 接着考虑从 $d_i$ 推到 $d_{i+1}$，首先是向右 $shift$ 一位，在最前面补上一个 $0$（最末尾的 &gt; m 扔了不管），然后把前 $a_i$ 个 $ 0$ 变成 $1$。 因此，可以用一个栈来维护所有是 0 的位置下标。

[<a href="https://www.fzoi.top/problem/5410" target="_blank" rel="noopener nofollow">AGC040E]Prefix Suffix Addition</a> 发现只有三种取值，而且取值连续，所以直接维护三种取值变化的断点即可

<a href="https://www.luogu.com.cn/problem/CF1500F" target="_blank" rel="noopener nofollow">[CF1500F]Cupboards Jumps</a>  直接设 $S_i$ 表示考虑了前 $i$ 个位置时 $d_i=|a_{i+1}-a_i|$ 的所有可能取值。考虑所有可能的转移。 1. 如果 $d_i + d_{i+1} = w_i$ ，那么就是 $S_i$ 整体打一个标记。 2 如果 $d_{i+1} = w_i$ ，那么向 $S_{i+1}$ 内新增一个元素 $w_i$ 。 3 如果 $d_i = w_i $，那么 $d_{i+1}$ 可以直接在 $[0, w_i ]$ 内任取，因此可以无视前两种转移。<a name="Cupboards_Jumps2"></a> <a href="#Cupboards_Jumps1" rel="noopener nofollow">相关 trick</a>

或者说，DP 表现为一个判定性问题，即 dp[...]=0/1

那么，可以考虑状态之间的关系，即是否满足 dp[i]=dp[j]=1 的时候，dp[i] 或 dp[j] 更具有潜力且与 i,j 大小有关的情况。那么此时，可以把 dp 中的一维甩成 DP 值，更改 dp[...]=0/1 为 dp[..]=min/max

<a href="http://codeforces.com/problemset/problem/1620/F" target="_blank" rel="noopener nofollow">CF1620F</a> <a href="https://www.cnblogs.com/wyb-sen/p/16281229.html" target="_blank">题解</a>

<a href="https://www.fzoi.top/problem/7294" target="_blank" rel="noopener nofollow">「AGC048D」 Pocky Game</a>

<h3 id="平均数中位数-二分答案">平均数/中位数 二分答案</h3>

一个序列平均值 $&gt;mid$，等价于序列中所有数减去 $mid$ 后，序列的和 $&gt;0$

中位数类似，如果 $A[i]&gt;=mid$ (注意此处的取等)，把权值赋为 $1$，否则权值赋为 $-1$  ，转化为判定是否序列和 $&gt;0$

<a href="https://www.fzoi.top/problem/5324" target="_blank" rel="noopener nofollow">[ABC236E]Average and Median</a>  判定的时候需要求最大子序列和，$dp[i][1/0]$ 表示考虑前 $i$ 个数，第$i $ 个数选/不选的最大子序列和即可

<a href="https://atcoder.jp/contests/arc075/tasks/arc075_c" target="_blank" rel="noopener nofollow">[ARC075E]Meaningful Mean</a> 管都不管，全部 -k，问题转化成有多少段区间和 $\ge 0$ ，前缀和后 BIT 即可

<a href="https://www.luogu.com.cn/problem/AT2165" target="_blank" rel="noopener nofollow">[AGC006D] Median Pyramid Hard</a> 操，想到二分答案但是不知道怎么二分，因为暴力模拟， x 是对的并不能知道 x+1 是不是对的，然后乱 gb 探究了好久的性质（你 tm 都暴力模拟了为什么还要二分答案？）。然后瞟了一眼题解，一看到题解说 &gt;= 标为 1 ,&lt; 标记为 0，就突然想起来标记过后就相当于判断 &gt;=x  的登基的合理性，因为如若 x 增大，1 的数量显然减少。具体判断，容易发现两个及以上相同的不会改变并且会传染，看那边传染得快就可以了。特判没有两个连续的情况。

注意本题有 O(n) 做法，思想是去二分化。

<a href="https://www.fzoi.top/problem/5962" target="_blank" rel="noopener nofollow">[JOI 2015 Final]舞会</a> 二分答案，三叉树上树形 DP 判断

<h3 id="期望线性性">期望线性性</h3>

$E(aX+bY)=aE(X)+bE(y)$

碰到有些期望的题目，一定要先秉承着白嫖的思路，在贡献具有独立性的时候，能拆则拆。

毕竟拆分出来的问题肯定比原问题要简单一些

<a href="https://www.luogu.com.cn/problem/P3239" target="_blank" rel="noopener nofollow">[HNOI2015]亚瑟王</a> <a href="https://www.cnblogs.com/wyb-sen/p/16258335.html" target="_blank">题解</a>

$\sum P(X = i)i = \sum P(X &gt;= i)$

非常典，非常有用。

<a href="https://www.fzoi.top/problem/7320" target="_blank" rel="noopener nofollow">CF1204E Natasha, Sasha and the Prefix Sums</a>

<h3 id="网格图上构造">网格图上构造</h3>

对于这种网格图，一个可能比较通用的构造思想是先满足一部分性质（比如行的性质），然后调整列。

<a href="https://www.fzoi.top/problem/6543" target="_blank" rel="noopener nofollow">20221015T2</a> 部分分解法。先把所有数字放在第一行。

<a href="http://codeforces.com/problemset/problem/1734/E" target="_blank" rel="noopener nofollow">CF1734E Rectangular Congruence</a> 先不管 b 的限制。通过整体+1 满足 b 的限制。

<a href="https://www.luogu.com.cn/problem/P7515" target="_blank" rel="noopener nofollow">2021 省选A卷 矩阵游戏</a> 先考虑随便搞一组解，然后考虑使得这组解满足值域限制。发现当前可以做的操作是给一行/列交错+1-1，设第 i 行做了 ci 次操作，第 j 行做了 cj+n 次，通过手法，将限制转化为差分约束的形式求解。

<h3 id="翻转棋子博弈">翻转棋子博弈</h3>

<blockquote>

经典的“反转棋子问题"的每个格子可以视为一个子游戏，所以整个局面的SG函数值就是所有黑格子的SG函数值异或起来的值。

</blockquote>

<a href="https://www.luogu.com.cn/problem/CF494E" target="_blank" rel="noopener nofollow">https://www.luogu.com.cn/problem/CF494E</a>

两个相同的棋子的和是 0， 所以可以看成一个位置有很多个，即因为如果一个位置有两个棋子，你就可以模仿对手走，所以两个棋子就是0个棋子。然后你每次操作是选择一个&gt;1的把它-1，把左上方+1，这个和原问题是等价的，所以很容易看出独立。

一个需要撇清的博弈论认知：

把 nim 游戏看作取走一个 a 放入一个 b，它们本质是相同的。因此我这里就是 -1，然后加一堆 1，然后后续博弈的时候只考虑这些加减 1 的位置，将所有加减 1 的位置集合的状态 视作当前的后继状态之一，当前的 SG 值就是这些后继状态的 SG 值的 mex。

也就是说，子游戏的后继可以不是单个子游戏，可以是一整个游戏，可以是多个子游戏的 SG 和。所以可以把整体游戏和单个子游戏的 SG 值等价视之，它们的地位是对等的。

然后这里由于异或运算是可结合可交换的所以你可以把一个游戏及其后继状态独立出来，所以有独立性。

<h3 id="无向图点权均不同的棋子博弈">无向图点权均不同的棋子博弈。</h3>

结论：先手必胜 $\Leftrightarrow$ 将无向图的边定向为从小到大，然后将得到的 DAG 视作博弈图，SG 不为 $0$

<a href="https://codeforces.com/problemset/problem/1658/E" target="_blank" rel="noopener nofollow">CF1658E Gojou and Matrix Game</a>

<a href="https://www.fzoi.top/problem/8279" target="_blank" rel="noopener nofollow">博弈</a>

<h3 id="一类树上边定向的-dp">一类树上边定向的 DP</h3>

「CF1394D」Boboniu and Jianghu

<a href="https://www.fzoi.top/problem/7591" target="_blank" rel="noopener nofollow">万分之一的光</a>

<a href="https://www.fzoi.top/problem/8279" target="_blank" rel="noopener nofollow">博弈</a>

<a href="https://qoj.ac/contest/1398/problem/7607" target="_blank" rel="noopener nofollow">The Doubling Game</a>

CTS2022D1T2

<h3 id="值域限制选数字类型-dp">值域限制选数字类型 DP</h3>

问题常常有 4 个限制：

值域、取值互不相同、取值个数、两个大小差不多的数字之间附加限制。

这种问题的解法是从小到大<strong>考虑每一个值</strong>，对值域 DP，可以干掉互不相同的限制，然后可以证明选出来的数字唯一对应一个原方案之类。

对于附加限制，常见的优化方法是只关注 &gt;= i - $\epsilon$ 的数字。大小差不多，所以 $\epsilon$ 很小，可以状压。

两道例题：详见 2022 10 月总结

<a href="https://www.luogu.com.cn/problem/CF1152F2" target="_blank" rel="noopener nofollow">CF1152F2 Neko Rules the Catniverse (Large Version)</a>

<a href="https://www.luogu.com.cn/problem/CF1463F" target="_blank" rel="noopener nofollow">CF1463F Max Correct Set</a>

<h3 id="对数轴上某点的覆盖次数进行-dp">对数轴上某点的覆盖次数进行 DP</h3>

感觉很典，见过很多次了，但是每次都没想到！

大概很有线头 DP 的味道，需要的性质是数轴上每一个点覆盖次数很少。

常见的是只有 0/1/2 次，然后把原贡献拆到每一个点上进行计算。

<a href="https://www.fzoi.top/contest/212/problem/7239" target="_blank" rel="noopener nofollow">20230801T2 送信</a>

<h3 id="点到根的距离权重-的和--子树权重和-的和">点到根的距离*权重 的和 = 子树权重和 的和</h3>

出现于 <a href="https://www.fzoi.top/problem/6549" target="_blank" rel="noopener nofollow">20221016T4 公交</a> 考虑怎么求一个点的答案，考虑将一个点的点权变为子树 size 的和，即选了这个点在线路中答案就会减去 size。

在权重 = 1 的情况下，点到根的距离就是深度。

<a href="https://www.luogu.com.cn/problem/P4211" target="_blank" rel="noopener nofollow">LNOI 2014 LCA</a> 假如题目问的是 $\sum_{i = 1}^n dep[lca(i, z)]$，那么答案就是 $\sum_{i \in path(rt, z)} siz_i$，题目还有 l, r 的限制，也就是说，只有 $i\in[l, r]$ 的点能够被算入 siz。假如只有一个询问，那么可以执行 r - l + 1 次树上差分；多个询问，一个愚蠢的想法是莫队，但是注意到答案具有可差分性，所以直接类似扫面线做就好。

<h3 id="双指针尺取法">双指针/尺取法</h3>

<blockquote>

我们可以通过双指针/滑动窗口解决一类问题，具体的，先让 $l=r=1$ ，然后让 $r++$ 一直到符合条件，然后算贡献，然后 $l++$ 算另一部分贡献。这样做的前提在于减法具有合法性，就是当 $l++$ 后，能保证取当前的 $[l+1,r]$ 是有正确性的

</blockquote>

对于一些极差问题，我们可以在权值排序后使用双指针算贡献

<a href="https://www.fzoi.top/problem/5396" target="_blank" rel="noopener nofollow">CF1555E Boring Segments</a> 先考虑加入区间，此时移动右端点，如果当前所取区间还没有完全覆盖数轴，那么我就一直取，直到被完全覆盖，便停止。如果当前左端点区间删去仍然使得条件满足，那么左端点向右移动一位。

<a href="https://www.fzoi.top/problem/5397" target="_blank" rel="noopener nofollow"> [NOI2016] 区间</a> 双倍经验

<a href="https://www.fzoi.top/problem/4528" target="_blank" rel="noopener nofollow">[CTS2021] 数组</a> <a href="https://www.fzoi.top/post/1927" target="_blank" rel="noopener nofollow">After_glow</a>

还有一些问题形如：给定一个值域在 $[L,R]$ 的序列，问有多少个子序列满足其中同时包含 $L,R$

能不能算同时包含 $L+1,R$ 的方案呢？这个时候你发现那些 $=L$ 的数没有贡献，于是你可以把一这些点作为分割点，分割成若干个 $[L+1,R]$ 的小段

<a href="https://atcoder.jp/contests/abc247/tasks/abc247_e" target="_blank" rel="noopener nofollow">ABC247E - Max Min</a> 把原序列分割成若干个值域在 $[L,R]$ 的段，然后滑动窗口

应用双指针求最大匹配数量，比如说给定 ai, bi，ai 和 bj 有边当且仅当 ai + bj &lt;= C，此时可以将 ai 升序，bi 降序，然后双指针匹配。这样做的原则是，如果 ai&gt;aj，那么 aj 能匹配的 ai 也可以，那么我们照顾更困难的，<s>然后更困难的就可以创造奇迹</s>

<a href="https://www.fzoi.top/problem/6841" target="_blank" rel="noopener nofollow">20230127 T2</a>

<h3 id="通过约束-dp-转移来满足限制">通过约束 DP 转移来满足限制</h3>

即可以首先设一个看起来比较危险，或者说容易算错/算重的 DP 状态，然后在转移中体现限制。

这样的 DP 讲究尝试的勇气，这样设 DP 的好处在于不用费尽心思在勾勒关键状态，而对于怎么样的转移是合法的 则是一个相对容易思考的问题。

<a href="https://www.luogu.com.cn/problem/CF1799H" target="_blank" rel="noopener nofollow">CF1799H Tree Cutting</a>：巧妙的 DP，设 $f[u][s]$ 表示通过砍子树 u 内的边并保留 u 这一块，可以构造出集合 s 中 c[i]，感觉这个 DP 状态是非常危险的 DP 状态，因为砍树边的顺序是很重要的，而状态中没能体现出这种顺序关系，此时我们考虑通过约束转移来满足条件。转移首先 $f[u][s]\gets f[u][t]\times f[v][s \backslash t]$，表示先不考虑 (u, fath) 这条边不砍。然后考虑 (u, fa) 要砍，对于一个状态 f[u][s]，可以算出此时 u 子树还剩下的真实 real_siz。如若当前决定保留 fath 这边，设 s 中最高位的 1 在 highbit，那么枚举 c[highbit+1...k]，与 real_siz 比较，看是否有新的满足；只枚举 highbit+1...k 实际上是为了满足“切掉子树以后子树内不能有操作”的限制。如果决定保留 u 这边，设 highbit 是 s 中最高的 0，即 u 已经构造 highbit+1...k 这些序列，然后看 real_siz 是否可以构造 c[highbit] ；实际上是为了满足“切掉子树外面以后所有的操作必须都在子树内”的限制。

<h3 id="对序列的空隙进行-dp">对序列的空隙进行 DP</h3>

详见 <a href="https://www.fzoi.top/problem/6895" target="_blank" rel="noopener nofollow">20230203 T3 括号</a>

<blockquote>

给定一个序列 $a$ 表示颜色为 $i$ 的点有 $a_i$ 个，相同颜色的点有区分；要求把这 $siz[u] = \sum a_i$ 个点排在一起，使得任意相邻两个点颜色不同，问方案数。

</blockquote>

最开始只有一个子树，假设它的大小为 $x$，此时序列只能是对这 $n$ 个点的排列，显然相邻两个点颜色相同，会产生 $n - 1$ 个不合法的间隙

考虑在当前序列中插入一个大小为 $m$ 的子树，可以想见，如果某一个点插入到一个不合法的空隙，那么这个不合法的空隙将会消失；但是如果同属于这个新子树的两个点相邻地插入，那么会增加一个不合法的空隙。

最后答案肯定希求没有不合法的点。

如是可以设出状态：$g_{x}[i][j]$ 表示考虑了前 $x$ 个子树，序列一共有 $i$ 个空隙，$j$ 个不合法。

转移时，对于 $siz[v]$，枚举 $a$ 表示这个子树里面选 $a$ 个数插入序列（显然一棵子树不用全部插入，可能在计算 $f[fath]$ 时被加入序列）；枚举 $b$ 表示这 $a$ 个数字被砍了 $b - 1$ 刀，形成的 $b$ 个段，表示同一个段内的数插入到原序列的同一个空隙，即会导致新产生 $a - b$ 个不合法空隙；枚举 $k$ 表示使得 $k$ 个原来的不合法空隙消失。


$$
g_x[i + a][j + a - b - k] \gets g_{x - 1}[i][j]\binom{siz[v]}a \binom{a - 1}{b - 1} \binom jk \binom{i + 1 - j}{b - k}\ a!

$$

实际实现的时候，如倒序枚举 $i$，可以省掉 $x$（和普通的树形背包类似）

如是，我们得到了 $f[u][i]$ 的初值 $= g[u][i - 1]$，表示没有比 $u$ 更深的 lca 了。

考虑在进行转移 $f[w][\star] \to f[u][\#]$，需要算出贡献系数。

一个方法是枚举 $w$，先算出新的 $g[i][j]$ 表示仅不插入 $w$ 的子树的答案。

对于一个 $g[i][j]$，枚举 $f[w][a]$，选取 $w$ 子树内的 $b$ 个插入序列。由于当前还剩下 $j$ 个不合法的空隙，那么这 $b$ 个有查漏补缺的义务，由于是最后一个插入的子树，所以不必要产生新的不合法漏洞。


$$
f[u][i + a + b]\gets f[w][a]\  g[i][j] \binom{siz[w] - a}{b} \binom{i - j}{b - j} \ b!

$$

注意这里不要 $\binom {siz[w]}a$，因为 $f[w][a]$ 算了。答案在 $f[1][n]$。

<h3 id="值域每次操作砍半">值域每次操作砍半</h3>

只会掉 log 层，操作次数少。

<a href="https://www.luogu.com.cn/problem/CF765F" target="_blank" rel="noopener nofollow">[CF765F]Souvenir</a> 离线之后，固定 r， DS 维护 l 的答案。详见 2022 10 月总结。

<a href="https://www.cnblogs.com/wyb-sen/p/15058080.html" target="_blank">[Ynoi2007] rgxsxrs 题解</a>

或者也可以用于计算 DP 的复杂度，比如转移类似 dp[k][i] \to dp[k + 1][i / 2]，那么有用的 DP 状态不会太多

<a href="https://www.fzoi.top/problem/6577" target="_blank" rel="noopener nofollow">20221022T4 序列</a>  不妨从后往前选数，这样 a_i 变成了单调不降。

设 f_{i, j} 表示目前有 j 个值为 i 的数的答案。那么加入 a_i，就有 $f_{a_i, j} - b_i + c_{a_i} \to f_{a_i, j + 1}$<br>
再考虑合并，从 i 合并到 i + 1 会有 $f_{i,j} + \lfloor\frac j2 \rfloor \times c_{i + 1} \to f_{i  +1, j / 2}$。<br>
由于 a_i 单调不降，所以以后不会再出现小于 a_i 的数。因为每次向上合并时个数都会除以二，所以合并的复杂度是 $O(n)$ 的，总复杂度 $O(n^2)$

<h3 id="求本质不同的子序列数目">求本质不同的子序列数目</h3>

<a href="https://www.cnblogs.com/ptno/p/16541669.html" target="_blank">pt/se/se/se</a>

<h3 id="启发式合并">启发式合并</h3>

不要以为并查集在不能路径压缩的情况下就完全没有办法了！

<a href="https://www.fzoi.top/problem/4287" target="_blank" rel="noopener nofollow">「WC2021」括号路径</a>

<a href="https://www.fzoi.top/problem/4167" target="_blank" rel="noopener nofollow">「BZOJ4668」冷战</a> 注意到时间是递增的，所以一条第一次链接两个连通块的边肯定很优，即两个联通块之间只有一条连边。上个并查集，容易发现这个最后形成的是一个树的结构，询问两个点转化成询问路径最大值。因为要维护树的形态，所以并查集不能路径压缩，但是可以启发式合并 。另外一种思路是，我就让并查集只有判断连通性的功能（<a href="#%5BCOCI2017%5DPictionary" rel="noopener nofollow">类似</a>），维护树上 LCT 就好，不过大常熟。<a name="冷战"></a>

<a href="https://www.fzoi.top/problem/6875" target="_blank" rel="noopener nofollow">20230131 字符串</a> 里面官方题解。

<h3 id="启发式分裂">启发式分裂</h3>

当分治不均匀且中断点可以有多个时，取最左边/最右边那一个分治下去，这样的话是 log 级别的，证明是反过来做类似启发式合并。

如 <a href="https://www.fzoi.top/problem/5558" target="_blank" rel="noopener nofollow">Cerc2012 Non-boring sequences</a>。

或者 <a href="https://www.luogu.com.cn/problem/P5979" target="_blank" rel="noopener nofollow">[BZOJ 3711][PA2014]Druzyny</a>

或者说，如果每次一个长度为 n 的序列分裂成长度为 len 和 n - len 的两部分，如若能够保证复杂度是 min(len, n - len) 相关，那么复杂度也是对的

<a href="https://www.fzoi.top/problem/6949" target="_blank" rel="noopener nofollow">20230213 T3 加边</a> 贡献可以拆分成三种：一条没有被覆盖过的边+其他任意边；一条仅被覆盖过一次的边+对应边；两条树边，使得覆盖这两条边的集合相同。一二种贡献可以用线段树随便维护，考虑第三种贡献。称被同样集合覆盖的为一个等价类，发现整个过程就是不断分裂等价类的过程，显然这个分裂过程最多进行 n 次。考虑每次分裂的时候需要给等价类中每一个点重标号，为了保证复杂度，使用启发式分裂。

<a href="https://www.fzoi.top/problem/6964" target="_blank" rel="noopener nofollow">20230227 T3 我是C题</a>

<h3 id="最小乘积生成树">最小乘积生成树</h3>

<a href="https://www.luogu.com.cn/problem/P5540" target="_blank" rel="noopener nofollow">[BalkanOI2011] timeismoney | 最小乘积生成树</a>

<a href="https://www.luogu.com.cn/problem/P3236" target="_blank" rel="noopener nofollow">HNOI2014 画框</a>

<h3 id="三元环计数">三元环计数</h3>

讲解：<a href="https://www.luogu.com.cn/blog/command-block/san-yuan-huan-xiao-ji-si-yuan-huan-post" target="_blank" rel="noopener nofollow">command_block</a>

例题：<a href="https://www.fzoi.top/problem/6893" target="_blank" rel="noopener nofollow">20230203 计数</a> xjb 推式子发现实际上的问题是如何计数三元环

<h3 id="转化贡献计算方式">转化贡献计算方式</h3>

[Lust]<a href="https://www.luogu.com.cn/problem/CF891E" target="_blank" rel="noopener nofollow">https://www.luogu.com.cn/problem/CF891E</a>

<a href="https://www.fzoi.top/problem/6878" target="_blank" rel="noopener nofollow">20230201 T2</a>

<h3 id="线段树叶子节点暴力做">线段树叶子节点暴力做</h3>

出现于 <a href="https://www.cnblogs.com/wyb-sen/p/15058080.html" target="_blank">[Ynoi2007] rgxsxrs 题解</a> 时间换空间之举。

但是还可以应用到卡常，因为有时候，按照正解来反而常数不优秀。

比如 <a href="https://www.luogu.com.cn/blog/_post/426540" target="_blank" rel="noopener nofollow">PA2014 Druzyny</a> CDQ 分治，没有分治到底层，r - l 比较小的时候直接 n^2 暴力艹。优雅。

<h3 id="dsu-on-tree-做树上路径计数">DSU on Tree 做树上路径计数</h3>

不知道有什么用，但是听起来很 nb。

lxl：其实 DSU on Tree 也是一种链分治，不过是静态的

commonAnts：链分治作为链分治出现似乎都是一些需要整条链一起做的题，比如分治fft，需要链上卷积。其他情况，一般都会写成dsu形式

<h3 id="树上联通块计数">树上联通块计数</h3>

可以是点分治一下，然后计算所有包含这个分治中心的联通块，方法是 dfn 序搞出来之后，每次要么跳一个子树，要么 dfn+1 之类。

还可以考虑直接树形 DP，随便抓一个根，考虑让一个联通块在深度最小的点被计算到，直接设 $f_u$ 表示以 $u$ 为最浅点的联通块数量。

<a href="https://blog.bill.moe/bzoj4182-shopping/" target="_blank" rel="noopener nofollow">「bzoj4182」Shopping </a>

[2018 集训队互测 Day 1]完美的集合

<a href="https://www.fzoi.top/problem/8304" target="_blank" rel="noopener nofollow">轻涟 La vaguelette</a>

<h3 id="点分治优化多重背包">点分治优化多重背包</h3>

<a href="https://blog.bill.moe/bzoj4182-shopping/" target="_blank" rel="noopener nofollow">「bzoj4182」Shopping </a>

<h3 id="点分治路径类询问">点分治路径类询问</h3>

对于一类给定树上 q 条路径询问，且询问时 lca 强相关的问题，可以枚举路径的 lca，容易发现只需要枚举分治中心作为 lca 就好了。

如此处理，好处是把 倍增等等做法的 qlog 变成 nlogn + q，非常优雅。

upd，错误的，时间复杂度分析错了，因为为了找到每一个询问对应的分支中心，每一个询问会判断 log 次，所以是 qlog。

那么这样做的好处一是代码简洁不费脑子，二是，在其他方法为 qlog^2 的时候，可以变成 log，常数优秀一点。

<a href="http://119.27.163.117/submission/23650" target="_blank" rel="noopener nofollow">CSPS2022T4</a> 复杂度 qlog+qk^2+nklogn

<h3 id="猫树分治">猫树分治</h3>

草，感觉很典啊，怎么之前一直没有记录下来。

大概就是把一个询问区间 $[ql,qr]$ 放到对应的 $l,mid,r$ 使得 $l\le ql\le mid\le qr\le r$。然后就变成前缀和后缀拼起来，这个会好做很多。

记得好像有一个狠题，是 XCPC 的，但是忘了。

这个思想可以表述为，没有性质，通过“分讨”，硬造性质。然后造性质的代价就是花 log 分治一下。

<h3 id="没有性质通过分讨硬造性质">没有性质，通过“分讨”，硬造性质</h3>

感觉这个应该是一个很广泛的描述。

猫树分治。

笑话：$min\times mex$

正经题目：[ARC142F] Paired Wizards

<h3 id="树上路径覆盖转二维数点">树上路径覆盖转二维数点</h3>

一种询问树上路径覆盖了多少个关键点对的问题，可以把关键点对搞出来，然后对于一个合法点对 (u, v)，只有所有路径端点分别在 u, v 子树内的路径才能够得到它，所以可以根据 dfn 序转化为一个二维数点问题。

[AHOI 钥匙]

[生生不息] wyb 自己出的题/hanx

其实，此时的一个强要求是关键点对很少，有些时候关键点对看起来很多，但是可以通过容斥使得关键点很少，诈骗！

[20230316 T1] 大骗子！！ $x\times y \ge lca(x, y)$ 的条件不是用于考虑根号算法之类，而是考虑容斥成为 $x\times y &lt; lca(x, y)$，此时发现可能合法的 (x,y) 只会有 nlogn 对，那么问题瞬间好做起来了。

这种诈骗东西，还有 AHOI2013 钥匙；wyb 自己出的水题：生生不息。

接下来的问题考虑换根 DP，容易求出 ans[1]，考虑换根 $u\to v$ 的过程，发现对于所有不经过边 $(u, v)$ 的路径，贡献不变，所以


$$
\delta = \sum_{(x, y) pass\ (u, v)} [x\times y &lt; v]-[x\times y &lt; u]

$$

考虑离线排序，问题是 nlogn 个链加和 2n 个单点查询，用树上差分。

<h3 id="诈骗">诈骗。</h3>

<a href="https://www.luogu.com.cn/problem/CF833C" target="_blank" rel="noopener nofollow">CF833C Ever-Hungry Krakozyabra</a> 看到这种计数的题目，第一反应就是分析合法/不合法条件，然后根据分析出来的条件做 DP，做容斥之类；但是这道题由于答案的数量很少，所以可以暴力枚举并判断。

<h3 id="去二分化">去二分化</h3>

据说是学过二分的都知道的思想，但是我不知道。

<blockquote>

其大概意思就是二分虽好，可以大大简化问题，方便性质的发现，但是时间复杂度会增加一个 ⁡log，有的时候跑一个 log 未必轻松，所以我们可以先用二分找到一个问题的较好解法，然后根据这个性质，将二分改为扫描，尝试用另一种方法动态维护这个性质，复杂度就会降低一个 log。

-- studyingfather

</blockquote>

<a href="https://www.luogu.com.cn/problem/AT_agc006_d" target="_blank" rel="noopener nofollow">[AGC006D] Median Pyramid Hard</a> <a href="https://www.luogu.com.cn/blog/_post/413547" target="_blank" rel="noopener nofollow">studyingfather</a>

<a href="https://www.fzoi.top/problem/6988" target="_blank" rel="noopener nofollow">20230307 T1 Easy</a> 先尽量选尽可能多的，然后此时还剩下一些空间，可以利用这些剩余空间把一下 b 小的换进来。

考虑这样贪心的问题是什么，问题是可能还可以删更多，交换的方式不一定是一换一。但是如果不是一换一的话，选择的总数处于变化之中，会导致一些本来可以记入答案的点变得不合法。

发现如果知道我到底选多少个就很好办了，就存在一种 nlogn 求出答案的方法。

猜测 f(选择个数) = 对应答案 的图像应该是一个单峰，那么可以考虑三分。三分过程中，发现很可能存在一大段相同，那么此时三分就失去了合法性，但是确实是单峰。

取消掉单峰性质之后，发现最终答案满足可二分性，即如果我有一种方案使得牢固掌握 x 个知识点，通过按照 bi 从大到小不买这些知识点，同时买入一些难以掌握的知识点的方式，可以构造出所有 $&lt; x$ 的情况。

二分答案后，直接从 1 到 n 枚举选择的总数，问题容易转换成 维护 $bi \le k$ 和 $bi &gt; k$ 的两个集合，每次查询集合前 k 小，或者从集合中插入/删除 一个数。这个问题可以 线段树/树状数组 上二分解决。

考场上最后大致想到这里了，但是看着时间不多了，决定去打 T23 的部分分。

上述算法的复杂度是 nlog^2n 的，广附老哥介绍了一种 <strong>去二分化</strong> 的思想。

（注意，上述做法存在一点细节，诸如当我枚举到总数 n，答案 m，此时我从第一个集合中查询前 m 大、第二个集合中的前 n - m 大，一个担忧是可能这两个集合没有达到对应的大小；另一个担忧是会不会从第一个集合里面多选一点，第二个里面少选一点更优。仔细分析后，发现只需要判掉 n &lt; m || 集合1大小 &lt; m || 不论集合差异，前 i 小的和 &gt; limit 的不合法情况 并 判定从集合 1 中选出前 i 小合法 之后即可）

即上述 DS 实际上给了我们 log 时间复杂度判定固定 总个数 和 掌握的知识点数量 之后是否存在合法方案的能力，而最终答案的值域是 O(n) 的，我们自然想到能不能只做 O(n) 次上面的判定，即让每次判定都能够让 ans ++

于是，考虑由本来的二分答案、枚举总数，转变成枚举总数，判定 ans + 1 是否可行。可以发现，对于一个特定的总数，合法的 ans 是一段区间，故而上述做法获得了合法性；nlogn 预处理出每一个 总数 对应的 l（第一个合法点），每次 ans = max(ans, l) 并不断判定 ans + 1 是否可行即可。

这样的话，如果 ans + 1 的判定失败了，由于一个总数的合法区间只连续的一个，所以可以直接 break，这样最多失败 O(n) 次；如果成功了，ans = ans + 1，最多成功 O(n) 次。总复杂度 O(nlogn)

Gym102331H Honorable Mention 去二分化。

<h3 id="点-边容斥">点-边容斥</h3>

点数 - 边数 = 1 的应用！首次出现于 20221005 yjz 树上问题。

常见的形式是树上多个点对应一个集合，需要选出一些点使得其对应的集合有交。

点-边容斥一转攻势，直接对这个交进行计算。钦定某一个点是交，计算选集合的方案数，减去钦定某一个边是边的情况。

那么，对于原方案，系数就是 1

<a href="https://www.cnblogs.com/evenbao/p/13827758.html" target="_blank">evenbao</a>

[xxx]<a href="https://www.cnblogs.com/impyl/p/16787032.html" target="_blank">https://www.cnblogs.com/impyl/p/16787032.html</a>

<a href="https://www.fzoi.top/problem/6606" target="_blank" rel="noopener nofollow">20221031T4</a> 详见 2022 10 月总结。

<a href="https://loj.ac/p/2462" target="_blank" rel="noopener nofollow">[2018 集训队互测 Day 1]完美的集合</a>

<a href="https://vjudge.net/problem/QOJ-7564/origin" target="_blank" rel="noopener nofollow">The 2nd Universal Cup Stage 4: Taipei Problem G Game</a>

<h3 id="自建自动机对状态重新标号">自建自动机/对状态重新标号</h3>

在状态确定，状态数较少，状态之间的转移可以手模的时候，我们可以考虑自己动手，建一个自动机

或者说实际上合法的状态数很少，可以把状态重新标号。

<a href="https://www.fzoi.top/problem/5411" target="_blank" rel="noopener nofollow">[ARC119F]AtCoder Express 3</a>

<a href="https://www.fzoi.top/problem/6467" target="_blank" rel="noopener nofollow">Chainword</a>

<h3 id="树上最大独立集">树上最大独立集</h3>

有一个可以优化状态数的结论：

重新定义 $f_{u,0/1}$​ 表示 $u$ 子树内<strong>是(1)否(0)强制不选</strong>节点 $u$ 时的最大方案大小，转移显然，如此设便得到了一条重要性质：$0\le f_{u,0}-f_{u,1}\le val_u$(强制不选的答案肯定小于等于无限制的答案，且两者一定不会差出 $u$ 点的权值）

题目是 <a href="https://www.luogu.com.cn/problem/P8352" target="_blank" rel="noopener nofollow">[SDOI/SXOI2022] 小 N 的独立集</a>

对于二叉树，也有类似的结论： <a href="https://www.luogu.com.cn/problem/P9090" target="_blank" rel="noopener nofollow">P9090 「SvR-2」G64</a> <a href="https://www.luogu.com.cn/article/w4scecqp" target="_blank" rel="noopener nofollow">Leasier</a> 这里的目的是为了解决在取模意义下没有办法取最大值。

<h3 id="固定-r维护-r---r--1">固定 r，维护 r -&gt; r + 1</h3>

题目一般要求计算的是 $\sum f(l,r)$ ，考虑固定 $l,r$ ，计算 $l\to l-1$ 或 $r\to r+1$ 的 $\Delta$ 即可

其实这个就是扫描线的思想。

<a href="https://www.fzoi.top/problem/3610" target="_blank" rel="noopener nofollow">「NOI Online 2020 #2 提高组」子序列问题</a> RT，固定 $r$ ，小推一下式子，发现可以线段树暴力维护

<a href="https://www.fzoi.top/problem/4084" target="_blank" rel="noopener nofollow">「CF1396D」 Rainbow Rectangles</a> <s>强加离散化的出题人都是 sb 出题人</s> 容易双指针优化到 $O(n^3)$，然后考虑固定上边界，用 Segment Tree Beats 维护下边界上移的 delta

<a href="https://www.luogu.com.cn/problem/CF765F" target="_blank" rel="noopener nofollow">[CF765F]Souvenir</a> 离线之后，固定 r， DS 维护 l 的答案

CF526F Pudding Monsters 3000 以及它的升级版：CF997E Good Subsegments

NOI2022 D2T2 最后一步计算产生逆序对最少的位置的线段树处理。

<a href="https://www.fzoi.top/problem/6862" target="_blank" rel="noopener nofollow">Gym102759E Chemistry</a> 首先可以发现，如果强制使得链包含某一个点，那么固定这条链的左端点之后，右端点最多可以到哪里是确定的。那么我们不妨固定 r，类似双指针确定最左边可以到的 l，这个需要用 LCT 判环。还需要汉满足的是边数 = 点数 - 1，这个可以线段树维护后缀最小值，这个类似 “连续值域区间计数”的方法。

<hr>

或者说，统计所有后缀的前缀（SAM 的思想：CF1142D Foreigner

<h3 id="网络流建模">网络流建模</h3>

<blockquote>

考虑一个 01 变量 $x$，将 $x=0 $ 视为其对应点与源点联通，$x=1$ 视为与汇点联通。那么考虑最小割中每条边的贡献：对于形如 $(S,x,a)$ 的边，当且仅当 $x$ 与汇点联通时这条边才会有 $a$ 的贡献，于是可以将这条边的贡献记为 $ax$ 。同理，形如 $(x,T,a)$ 的边可看成 $a(1-x)$，而形如 $(x,y,a)$ 的边则可看成 $a(1-x)y$。对于某些问题，我们可以将贡献拆成上述三种形式，便可以通过跑最小割求解。

</blockquote>

常见 trick：平面图转对偶图

<a href="https://www.luogu.com.cn/problem/P4001" target="_blank" rel="noopener nofollow">[ICPC-Beijing 2006] 狼抓兔子</a> 平面图转对偶图模板

<a href="https://www.luogu.com.cn/problem/P7916" target="_blank" rel="noopener nofollow">[CSP-S 2021] 交通规划 </a> <a href="https://www.cnblogs.com/wyb-sen/p/16229507.html" target="_blank">题解</a>

Gym 104821E Extending Distance 对于平面图最小割转对偶图最短路的逆用。考虑平面图最短路转对偶图最小割。那么现在的目标是使得最小割增加 k，这个是简单费用流。

<hr>

费用流除了常见的 Dinic 之外，我们还有 $O(fn\log n)$ 的<a href="https://oi-wiki.org/graph/flow/min-cost/#primal-dual" target="_blank" rel="noopener nofollow">原始对偶</a> 其中 $f$ 是流量

<a href="https://www.fzoi.top/problem/5391" target="_blank" rel="noopener nofollow">ARC112F Domination</a> 考虑不移动蓝点，如何判断是否合法。相当于有若干个区间，判断是否每一个位置被覆盖了至少 $k$ 次。 这等价于选出 $k$ 个区间集合，满足每个集合覆盖了所有点。考虑一个 $n+2$个 点的网络流模型，点 $0$ 向点 $1$ 连边，点 $i$ 向 $i-1$ 连边，这些边流量不限。对于一个区间 $[l,r]$，从点 $l$ 向点 $r+1$ 连边，流量限制为 $1$ 。考虑 $0$ 到 $n+1$ 的最大流，满足要求当且仅当最大流大于等于 $k$。考虑将移动蓝点设为费用，然后 xjb 连边。答案为个图上流量为 $k$ 的最小费用流，使用原始对偶

<hr>

有时候，在流量较小的情况下，FF 算法也能焕发新生

<a href="https://www.luogu.com.cn/problem/CF1383F" target="_blank" rel="noopener nofollow">CF1383F Special Edges </a> 考虑枚举 $S$ 为特殊边的边集的一个子集，将 $S$ 内的特殊边的边权设为 $0$， 外的特殊边的边权设为 $inf$。考虑最大流等于最小割，发现此时求得的最小割就是钦定 $S$ 内的边一定割掉，其他非特殊边的边权和，设为$g_S$ 。 那么设 $f_S$ 为 $S$ 内边的权值和，那么答案就是 $\min\{g_S+f_S\}$。 每次询问 $O(2^k)$ 预处理 $f_S$。$g_S$ 每次可以选择一个 $i\notin S$ 加入，然后根据 $g_{S\cup\{i\}}$ 以及 $S\cup \{i\}$ 的残余网络来取得，注意到每次增广不会超过 $w$ 次，使用 FF 算法增广即可单次增广 。复杂度 $O(q2^k+w(n+m)2^k)$

<hr>

如若涉及到选了一组，组中两者都不能再选这类限制，可以往二分图匹配方向思考

<a href="https://atcoder.jp/contests/abc247/tasks/abc247_g" target="_blank" rel="noopener nofollow">ABC247G - Dream Team</a> 发现其实就是二分图最大权匹配，大学向学科连边 <a name="ABC247G - Dream Team"></a>

<hr>

割边的意义代表选择另外一个，对于组合起来的附加贡献，只需要构造边使得选择了对应组合之后图仍然联通即可。

<blockquote>

最小割解决的就是分成两个集合，求最优解，带限制的题

——fsy

</blockquote>

<a href="https://www.luogu.com.cn/problem/P1361" target="_blank" rel="noopener nofollow">P1361 小M的作物 </a><br>
UOJ 77 A+B Problem

<h3 id="线头-dp">线头 DP</h3>

<a href="https://www.cnblogs.com/Itst/p/10339605.html" target="_blank">https://www.cnblogs.com/Itst/p/10339605.html</a>

<h3 id="单点修改后缀查询进行扫描线">单点修改，后缀查询进行扫描线</h3>

一般的扫描线的方式是，对于一个修改区间 [l, r]，在 l 出 push_back(+, id)， r + 1 出 push_back(-, id)

这样的维护方式是有一定局限性的，此时或许可以采用在 DS 上的 r 位置处插入信息，每次询问一个后缀 [x, n]，所有被询问到的信息都满足 $l_i \le x \le r_i$，也做到了扫描线的效果

感觉是有出题的空间的，这样的好处在于一个 r 被插入之后，可以随着 x 的增加，对这个区间的信息进行修改。而一般的扫描线，是完全静态的。

<a href="https://www.luogu.com.cn/blog/lzqy/solution-cf1648d" target="_blank" rel="noopener nofollow">CF1648D Serious Business</a> 注意这篇题解里面写错了一点,insert1 里面 k 和 kk 应当是独立处理的。

<h3 id="树上点的贡献--父亲贡献子树贡献">树上点的贡献 = 父亲贡献+子树贡献</h3>

有时，我们涉及到树上某一个点的修改，如若我们每个点都维护 父亲贡献+子树贡献，那显然修改会爆，所以我们不妨每个点只维护子树贡献，在询问的时候加上父亲的贡献即可

<h3 id="基向量思想">基向量思想</h3>

<blockquote>

任意n个向量能表示的组合一定能用两个基向量(d,0),(x,y)(x&lt;d,y&gt;=0,d,x,y 都是整数) 的组合

</blockquote>

这里的组合指的是整数倍的组合。

比如，最开始只有两个向量 (x, y) (a, b)，在图上把能够表示的向量画出来，可以发现是一个一个一个一个全等的菱形。这些菱形的基向量显然可以认为是 (d, 0) 和 (x, y)

我们的思考是用尽量简洁的基向量来表达，最理想的情况是 x = 0，所以我们可以让 x 尽量往小靠，即把 x % d。

此时，再次加入一个向量 (a, b)，考虑变化，只要保证新的 d2 是 d 的约数，y2 是 y 的约数，x2 是 x 通过和 y 一样的“初等行变换”得到的（初等行变换是的），那么可以表达原来的所有。

显然，y2 = gcd(y, b)

用 (x, y) 和 (a, b) 线性组合出在 x 轴上的基向量，即考虑他们两个向量构成的菱形：


$$
\frac yg(a, b) - \frac bg(x, y) = (\frac 1g(ya - bx), 0)

$$

和原来的 d 组合，$d2 = \gcd(d, \frac 1g(ya - bx))$

x2 是 x 跟着 y 的初等行变换得到的，解 exgcd 得到 y -&gt; y2 的方式：py + qb = gcd(y, b)

那么 x2 = (px + qa) % d2

Gym102769I Interstellar Hunter 模板

<h3 id="竞赛图相关">竞赛图相关</h3>

<a href="https://www.luogu.com.cn/blog/_post/215154" target="_blank" rel="noopener nofollow">强连通相关结论</a>

<a href="https://blog.csdn.net/a_crazy_czy/article/details/73611366" target="_blank" rel="noopener nofollow">兰道定理</a>

<a href="https://blog.csdn.net/hzk_cpp/article/details/94895213" target="_blank" rel="noopener nofollow">哈密顿路径相关</a>

<ul>
<li>缩点之后是一条链，满足前面的点都向后面的点有连边。</li>
<li>$n&gt;3$ 个点的强联通竞赛图存在大小为 $[3,n]$ 的哈密顿回路。</li>
</ul>

竞赛图，考虑先求出一条哈密顿路！

<a href="https://www.fzoi.top/problem/4548" target="_blank" rel="noopener nofollow">CF1514E」 Baby Ehab's Hyper Apartment</a> 2023 水土总结

<a href="https://www.fzoi.top/problem/8286" target="_blank" rel="noopener nofollow">20240220 图图 graph</a> 如果能够把最后的图的强联通分量求出来，那么这个题是容易的。由于这个图和竞赛图很接近，考虑先把这个图在不影响强连通性的情况下补成一个竞赛图。这个只需要按照度数排序之后，从前往后连边即可。补成强联通之后，用兰道定理就可以划分强联通块了。

<h3 id="拆哈密顿路">拆哈密顿路</h3>

对于无向图上一条 $x\leadsto y$ 的哈密顿路，可以拆成 $x\leadsto 1\leadsto y$ ，并将 $x\leadsto 1$ 反向，这样常常可以达到只做一次 DP 的目的。

<a href="https://www.fzoi.top/problem/6975" target="_blank" rel="noopener nofollow">Gym 104197G Graph Problem With Small</a>

<a href="https://www.fzoi.top/problem/5307" target="_blank" rel="noopener nofollow">CF1616G Just Add an Edge</a>

<h3 id="oplus---cupcap--xor-or-and-互换">oplus -&gt; cup/cap / xor or and 互换</h3>

看到一些维护异或的题目，可以考虑往 and 和 or 上面转，可能转一转就好做了

<a href="https://www.fzoi.top/problem/6952" target="_blank" rel="noopener nofollow">鸽子做加法</a> 首先，为了避免混循环小数，我们先把分母通过乘上2的幂次都变成奇数。对于整数部分，直接异或就好。

考虑小数部分，首先求出循环节，设 a 的循环节长度 n, b 的长度 m，显然有 O(nm) 的暴力做法。

思考我为什么必须要扫描 O(nm) 个位置，想到我如果能够只扫描 O(n) 个位置就好了，考虑把异或运算替换一下。发现对于 &amp; 运算而言，仅在二者都为1的时候有值，看起来就比异或优秀，于是，首先通过 a \oplus b = a + b - 2 * (a \cup b) 转化一下。

设 a, b 二进制循环节 p, q, 现在需要求解的是 $\sum_{i, j, p_i = q_j = 1} 2^{-pos(i,j)}$，其中 pos(i, j) = x，表示 x % n = i, x % m = j。

看到下面的式子可以联想到中国剩余定理，但是中国剩余定理要求 n, m 互质。实际上也好办，设 g = gcd(n, m)，可以将每一个位置表示成 kd + i 的形式，容易发现对于 除 d 余数不同的位置，其 pos 是无解的。那么可以枚举 i = 0...d - 1，将 p,q 分类然后计算。

下面所称的 p, q 都是经过分类的。全部当成 i = 0 进行计算，最后乘上 2{-i} 即可。

运用 exgcd，求出 n mod m 意义下的逆元 n^{-1}, m mod n 意义下的逆元 m^{-1}，令 i = i * m * m^{-1}, j = j * n * n^{-1}，此时即是求解 $\sum_{i, j} (2^d)^{(i + j) \bmod nm}$

这个是好处理的，排序后，双指针分开处理 $&lt; nm$ 的部分和 $nm \le x &lt; 2\times nm$ 的部分。

复杂度 nlogn，由于快速幂出现较多，有一定常数开销。

<a href="https://atcoder.jp/contests/abc291/tasks/abc291_g" target="_blank" rel="noopener nofollow">ABC291G</a>

<h3 id="_">$\cap \Leftrightarrow \times$</h3>

把两个数组对应位 and 起来，等价于枚举二进制位把这两个数组拆开之后对应位做乘法

而对应位做乘法，就很有卷积的味道了，或许可以用到。

<a href="https://atcoder.jp/contests/abc291/tasks/abc291_g" target="_blank" rel="noopener nofollow">ABC291G Or Sum</a>  or-&gt;and-&gt;times，把 a 扩成两倍，做一遍卷积得到循环位移 1...n 位的答案。

<h3 id="-在--中为-1-当且仅当-">$\binom{n + m}m$ 在 $F_2$ 中为 1 当且仅当 $n \ and\  m == \varnothing$</h3>

具体证明可以思考 Luacs 定理的过程。

[ARC137D] Prefix XORs

有结论，$\binom{n + m}m$ 在 $F_2$ 中为 1 当且仅当 $n \ and\  m == \varnothing$，具体证明可以思考 Luacs 定理的过程。

所以 $b[k][n] = \oplus a[i] [k - 1 \ and\ n - i == \varnothing]$， 令 a[i] = a[n - i], 那么 $b[k][n] = \oplus_{i \subsetneq C(k - 1)} a[i]$，即超集和，FWTand 即可。

[CF1713F]Lost Array 见 2022 11 月总结

<h3 id="维护染色连通块">维护染色连通块</h3>

很多与树有关的题目，当边权不好处理时，有时候会转化为此边子节点的点权处理。因为有根树中除了根，每个点都有唯一的父边。当然也可以把点权放到父边上

<a href="https://www.luogu.com.cn/problem/SP16549" target="_blank" rel="noopener nofollow">SP16549 QTREE6 - Query on a tree VI </a> 容易想到开黑白两颗 LCT，两个点如若颜色相同，那么连边，然后 LCT 维护子树信息即可。考虑到可能会被菊花图卡爆，于是我们把颜色放到父边上，容易发现每一次只会涉及到 $(f[u],u)$ 的 cut 和 link。询问的时候，findroot 然后输入根右儿子即可。

<h3 id="lcs-转-lis">LCS 转 LIS</h3>

转化方法：<a href="https://blog.csdn.net/niuox/article/details/7709805" target="_blank" rel="noopener nofollow">https://blog.csdn.net/niuox/article/details/7709805</a>

这样搞的好处在于优化复杂度，因为 ai 在 b 中出现的次数显然是不满的。特别体现在 <a href="https://qoj.ac/problem/2622" target="_blank" rel="noopener nofollow">Gross LCS</a> 一题。此题，容易注意到有用的 x 只有 n^2 种，本来是要做 n^2 LCS，复杂度是 n^4, 但是如果转化成 LIS，容易发现 ai 在 b 中出现的总次数是 n^2，所以复杂度变成了 n^2log。

<h3 id="对比赛过程建树">对比赛过程建树</h3>

<a href="https://atcoder.jp/contests/abc263/tasks/abc263_f" target="_blank" rel="noopener nofollow">ABC263F Tournament</a> 建树完毕之后，分析一下胜场情况，简单 DP

<a href="https://uoj.ac/problem/702/statistics" target="_blank" rel="noopener nofollow">UOJ702 张飞卷精兵</a> 胜者是败者的父亲，贡献的计算方式是奇数层的和-偶数层，同时要保证父亲的权值比儿子大，填了父亲才可以填儿子（解锁）。那么一个显然的贪心想法是从大向小填，如果当前能填入偶数层节点则直接填入，否则选一个节点填入，使得被解锁的节点数最多，如有多个任选即可。画出来可以发现如果存在多个点儿子数量相同，对于儿子数量相同的点，子树都是同构的，所以可以任取。后续只剩下计算答案的问题。

<h3 id="树上拼路径">树上拼路径</h3>

考虑枚举中转点，我们有 dp/淀粉质 等思路

从深度的角度来看，我们还可以在线段树合并的过程中计算路径贡献

<a href="https://www.luogu.com.cn/problem/CF490F" target="_blank" rel="noopener nofollow">CF490F Treeland Tour</a> <a href="https://www.cnblogs.com/wyb-sen/p/15911357.html" target="_blank">题解</a>

<a href="https://www.luogu.com.cn/blog/_post/277528" target="_blank" rel="noopener nofollow">Piwry 的题解也是绝世经典</a>

注意其中所提到的一种对于 dsu on tree 的应用：考虑用 DS 维护重儿子信息，然后暴力枚举轻儿子来拼路径。同时，在 dfs 过程中开 log 个线段树的方法，即一条重链可以不断继承自己重儿子的线段树并暴力枚举合并轻子树。注意撤回。

---&gt; 这个方法其实就是启发式合并。其本质都是把轻的往重的上面并。所以这里其实阐述的是 dsu on tree 其名称：树上启发式合并 的来源，不要仅仅把它理解成是一种简单的获取所有子树信息的手段，那个其实才是它的扩展应用，他在获取子树信息过程中的“拓扑”关系所蕴含的信息量并没有很好地体现在简单的“子树信息”内。仿佛对于分治 dfn 序的手段，由于独特的拓扑关系，使得分治 dfn 序可以解决 stcm 的问题。

---&gt; 这个方法可以导向最后的线段树合并方法，即考虑不暴力枚举轻儿子，而是在合并轻儿子的线段树的过程中贡献答案。

同时，结合用维护辅助数组的 dp 方法，也可以dsu on tree维护 log 个辅助数组。

---&gt; 同时，注意到这个做法的复杂度和子树大小无关而是和辅助数组长度有关，所以不要用 dsu on tree 去维护这个辅助数组，而是搞一个长剖。这样也可以做到 $O(nlogn)$

<hr>

也可以考虑从一个端点入手，树剖后线段树区间合并维护信息。典型的，对每条重链开一棵线段树

对于线段树上的一个节点 $p$，其对应区间为 $[L,R]$（重链上的一条路径）

记 $lmx_p$ 表示从 $L$ 出发，在 $[L,R]$ 间的某个节点拐出重链能到达的最远符合要求的点，$rmx_p$ 同理

<a href="https://www.luogu.com.cn/problem/SP16580" target="_blank" rel="noopener nofollow">SP16580 QTREE7 - Query on a tree VII </a> <a href="https://www.luogu.com.cn/blog/_post/413020" target="_blank" rel="noopener nofollow">After_glow</a>

<h3 id="dfs-序差分">DFS 序+差分</h3>

对于单点加和链上查询，一般的处理手法是上一个树剖然后 $O(\log n)$ 修改，$O(\log^2n)$ 查询

然而使用 DFS 序和差分，将单点加转化为子树加，将链上查询转化为单点查询，这样就变成了 $O(\log n)$ 修改和查询

具体的，设 $sum[x]$ 表示点 $x$ 到根路径上的贡献即可。

或者一类链加单点查询的问题，可以首先通过容斥变成每次加一条到根的链，然后变成单点加子树求和。

这样的局限性在于难以处理一类信息为 max/min 的问题，只能重算了。

<a href="https://www.luogu.com.cn/problem/P4216" target="_blank" rel="noopener nofollow">[SCOI2015]情报传递</a> 发现只有在时刻 $i-k+1$ 之前才会有贡献，于是把询问离线一下，问题变成了单点加和链上查询。

<a href="https://www.fzoi.top/problem/6695" target="_blank" rel="noopener nofollow">2022 杭电多校 G.Treasure / 2022 11 月三校 ACM 欢乐赛 A</a>  <a href="https://ddosvoid.github.io/2022/07/24/2022%E6%9D%AD%E7%94%B5%E5%A4%9A%E6%A0%A11-G-Treasure/" target="_blank" rel="noopener nofollow">ddovoid</a> 注意这里因为是子树求和，所以每次更新的时候需要在父亲处减去自己的贡献

<h3 id="meet-in-the-middle">Meet in the Middle</h3>

最开始，这项操作用于优化 DFS ，使用于当同时知道起点和终点的时候，每次从起点/终点开始搜，只向外搜一个闸值范围的内容。这样可以把 DFS 的复杂度砍成原来的根号

慢慢的，由于发现其实很多计数题之类的具有和 dfs 类似的性质，然后每次考虑把原来的集合分成两份，分别算出两部分内部的信息，然后考虑合并两个集合的信息即可。

一般的，$1\le n\le 40$ 的数据范围差不多是这个 trick 的标配，属于暴力状压 DP 裂开的范畴。因为一般计算信息、合并复杂度大概是$O(\frac n2 2^{\frac n2})$ ，刚刚好和 $nlog n(n=1e6)$ 差不多。

<a href="https://atcoder.jp/contests/abc220/tasks/abc220_h" target="_blank" rel="noopener nofollow">ABC220H - Security Camera</a> <a href="https://www.cnblogs.com/wyb-sen/p/16161098.html" target="_blank">题解</a>

<a href="https://www.luogu.com.cn/problem/CF1336E1" target="_blank" rel="noopener nofollow">CF1336E2 Chiori and Doll Picking (easy version)</a> 考虑首先我们容易得到一个 2^{rank} 的做法，其中 rank 是线性基的秩。看到 m \le 37，是折半搜索的“标配”。考虑把线性基分成高位低位两部分，设 g[s] 表示低位部分异或和为 s 的方案数， f[i][s] 表示 高位的 ppc 为 i，低位的异或和为 s。设 $h_c = f_c \ast g$，$h_{0...m - mid}$ 显然可以使用 FWT 在 $\mathcal{O}(m^22^{mid})$ 的复杂度做出来，答案随便统计。

<a href="https://uoj.ac/problem/704" target="_blank" rel="noopener nofollow">马超战潼关</a>

<h3 id="离散化优化-dp">离散化优化 DP</h3>

常见于每一个数字取值在范围 [l, r] 内，计算满足 xx 条件序列的方案数。

此时可能需要将当且数字的取值放入状态，那么状态数将是值域级别，当然，如果转移可以用 DS 维护，那么可以直接用 DS 维护。

否则，可以考虑离散化，那么此时值域变成了 O(n) 级别，可以转移。

<a href="https://www.luogu.com.cn/problem/P3643" target="_blank" rel="noopener nofollow">APIO2016 划艇</a>

或者说，当每一个数字的贡献可以表示成为 f(x) 的形式，其中 x 是一个枚举量， f(x) 是一个次数不高的（分段）函数，那么可以将需要枚举 x=0...1e9，通过离散化区间端点，变成需要对 O(n) 个区间进行处理，并且，在这每一个区间中，每一个数字的贡献是固定的，那么区间总贡献形如 多个 f(x) 的乘积 的前缀和，即多个多项式的乘积，可以考虑 <strong>拉格朗日插值优化</strong>。

<a href="https://www.luogu.com.cn/problem/P8290" target="_blank" rel="noopener nofollow">省选联考 填树</a>

<a href="https://www.luogu.com.cn/problem/P5469" target="_blank" rel="noopener nofollow">NOI2019 机器人</a>

<h3 id="费用提前计算">费用提前计算</h3>

DP 的时候，为了达到优化复杂度的目的，引入费用提前计算的思想（当然有时候是必须这样做）

<s>当然，你也可以费用推迟计算</s>

<a href="https://www.cnblogs.com/wyb-sen/p/15113590.html" target="_blank">任务安排</a>

<a href="https://www.fzoi.top/problem/4784" target="_blank" rel="noopener nofollow">[BZOJ1426]收集邮票</a> <a href="https://www.cnblogs.com/wyb-sen/p/16262564.html" target="_blank">题解</a>

<h3 id="分治">分治</h3>

常见的分治有图像的分形

<a href="https://www.luogu.com.cn/problem/P1003" target="_blank" rel="noopener nofollow">[NOIP2011 提高组] 铺地毯</a>

有时候，分治时间复杂度的证明需要借助启发式合并的思想

<a href="https://www.fzoi.top/problem/5558" target="_blank" rel="noopener nofollow">[Cerc2012]Non-boring sequences</a> 对于区间 $[l,r]$，如果其中第 $i$ 个只出现一次，那么所有包含第 $i$ 个数的子区间均符合条件 ，只需要递归处理左右两端区间 $[l,i-1],[i+1,r]$

注意到最值会把一个序列分成两段

<a href="https://www.fzoi.top/problem/5555" target="_blank" rel="noopener nofollow">[BZOJ3897]Power</a> $ S(l, r, st, ed) $表示第 $l$ 天到第 $r$ 天，初始体力 $st$ ，结束体力 $ed$ 的最大收益。显然，我们想让这个区 间中的收益最大的那天干的越多越好，于是分情况讨论： 如果从一开始就休息，一直休息到收益最大的那天，没有达到体力的上限，那就一直休息。  如果达到了体力上限，递归左半部分，将多余的体力用掉。  右边也是同理，如果在收益最多的那天用了所有体力之后一直休息，到最后一天体力小于 $ed$，那就从收益最大的那天中少工作一些，使得最后一天的体力为 $ed$。 否则递归处理右边，消耗掉多余的体力，使得最后的体力为 $ed$。 区间最大值，线段树/ST 即可。

一个经典的分治思想是 CDQ 分治，即考虑计算左区间对右区间的贡献

<a href="https://www.luogu.com.cn/problem/P6406" target="_blank" rel="noopener nofollow">[COCI2014-2015#2] Norma</a> 虽然注意到最值会把一个序列分成两段，但是并不适合套用这个方式。考虑左边对右边的贡献，分类讨论后发现可以前缀和优化一下算贡献，前缀和正确性来自于最大值小值的单调性

看到网格图，可以考虑整体二分

<a href="https://www.luogu.com.cn/problem/P3350" target="_blank" rel="noopener nofollow">[ZJOI2016]旅行者</a> <a href="https://www.cnblogs.com/wyb-sen/p/16082521.html" target="_blank">题解</a>

<strong>CDQ 分治优化建图</strong> 处理一类连边条件具有多重偏序关系的题。

<a href="https://www.luogu.com.cn/problem/P5331" target="_blank" rel="noopener nofollow">SNOI2019 通信</a> 详见 2022 10 月总结。

<h3 id="配对堆代替-set-优化常数">配对堆代替 set 优化常数</h3>

卡 nm 的烂常！

<h3 id="威佐夫博弈">威佐夫博弈</h3>

<a href="https://baike.baidu.com/item/%E5%A8%81%E4%BD%90%E5%A4%AB%E5%8D%9A%E5%BC%88/19858256" target="_blank" rel="noopener nofollow">威佐夫博弈_百度百科 (baidu.com)</a>

两堆石子 $(a,b)$ ，每次可以到  $(a-k,b),(a,b-k)$ 或 $(a-k,b-k)$ ，$(0,0)$ 结束

有结论，若 $(b-a)\times\frac {\sqrt 5-1}2==a$ ，则先手必败，否则，先手必胜。

<a href="https://www.luogu.com.cn/problem/P3024" target="_blank" rel="noopener nofollow">[USACO11OPEN]Cow Checkers S </a>  板子题

<h3 id="最小链覆盖">最小链覆盖</h3>

有向边 $(u, v) \to (u, v + n, 1)$，得到二分图，最小不交连覆盖=总点数-最大匹配数量，初始视所有点都是一条路径。

最小可交链覆盖：添加 (u + n, u, inf) 的边！！！！不需要跑传递闭包，注意此时应连边 (u, v + n, inf)。<a href="https://www.fzoi.top/problem/6999" target="_blank" rel="noopener nofollow">法克</a>

<h3 id="二分图必匹配边">二分图必匹配边</h3>

若一条边一定在最大匹配中，则在最终的残量网络中，这条边一定满流，且这条边的两个顶点一定不在同一个强连通分量中。

证明也很简单：首先满流的要求是很显然的，其次，如果这两个点在同一个强连通分量中，那么一定有一个环经过这条边，沿着环增广一下，网络仍然满足流量限制，但是这条边就不满流了，于是就得到了一组新的最大匹配。

<a href="https://www.luogu.com.cn/problem/P3731" target="_blank" rel="noopener nofollow">[HAOI2017]新型城市化</a> 一定是一个二分图，想删边后最大独立集变大，那么最大匹配变小，也就是说删的是必匹配边

<h3 id="二分图必匹配点">二分图必匹配点</h3>

结论：对于任意一个最大匹配不为 0 的二分图，一定存在一个节点，使得我们将这个节点删去后，最大匹配刚好减 1，即所有的最大匹配存在一个公共的匹配点。

最大匹配 = 最小点覆盖 = $n$ − 最大独立集，每次一定可以找到一个不在最大独立集里面的点，删除它的所有边，使得最大独立集变大，最大匹配变小。

<a href="https://www.luogu.com.cn/problem/CF1525F" target="_blank" rel="noopener nofollow">https://www.fzoi.top/problem/7336</a>

<h3 id="网格图转二分图">网格图转二分图</h3>

CF1887E Good Colorings

[Ptz Winter 2020 Day 6 E] The Destruction of the Crystals

<h3 id="建模转化">建模转化</h3>

/kk 这种题是真的神奇，只能碰到一道记录一道了

建模转化的题目是指把题目给的条件转化到图论解决

JOISC 2019 day2 两道料理 <a href="https://www.cnblogs.com/wyb-sen/p/16537916.html" target="_blank">题解</a>

<a href="https://www.luogu.com.cn/problem/AT4353" target="_blank" rel="noopener nofollow">[ARC101D] Robots and Exits</a> <a href="https://www.cnblogs.com/wyb-sen/p/16127071.html" target="_blank">题解 </a>

对前缀和建模：

CF1458D Flip and Reverse：神仙建模转化题。考虑用 -1 代替 0，设 si 表示前缀和，si 向 s{i+1} 连边。在这个意义下k考虑操作 [l,r]，首先要求 s[l - 1] = s[r]，那么此时操作的是一个环，操作等价于将环上的边反向。观察到操作前后，边集 E 是保持不变的，因为一旦我翻转了有向边 x-&gt;y，由于操作的是一个环，我必然翻转 y-&gt;x，因此我们可以把这些边都当作有向边。此时，有结论，新的无向图的任意一个欧拉回路，和原字符串经过操作可以得到的字符串可以建立双射。具体的证明可以考虑欧拉回路的路径和原字符串的第一个分界点，利用翻转操作，容易使得两条路径重合，即消去这个分歧点，归纳即可。

题目条件类似于如若选择了一组，那么组中两个数不必再选，我们可以让组内两个点连边

<a href="https://atcoder.jp/contests/abc247/tasks/abc247_f" target="_blank" rel="noopener nofollow">ABC247F - Cards</a> 连边后，由题目条件，可以知道构成若干个环，每一个环可以单独处理。每一个环的贡献先当于每个点不能同时断两条边，求断边方案数。[类似](#ABC247G - Dream Team)

<h3 id="计数技巧">计数技巧</h3>

<blockquote>

条件计数的路子无非两条，要么寻找更简洁的充要条件，要么容斥

——<a href="https://www.luogu.com.cn/user/58705" target="_blank" rel="noopener nofollow">command_block </a>

</blockquote>

寻找更简洁的充要条件有这样两个思路，一个是推一推式子，另外一个是直接看满足条件的终态需要满足的条件，然后验证一下充要性

看终态 <a href="https://www.luogu.com.cn/problem/P5156" target="_blank" rel="noopener nofollow">[USACO18DEC]Sort It Out P</a>  <a href="https://www.cnblogs.com/wyb-sen/p/16185949.html" target="_blank">题解</a>

容斥 <a href="https://www.luogu.com.cn/problem/P1450" target="_blank" rel="noopener nofollow">[HAOI2008]硬币购物</a> <a href="https://www.cnblogs.com/wyb-sen/p/16219046.html" target="_blank">题解</a>

容斥常常需要把问题拆分掉，对某个合法状态计数时，往往可以拆成一个更小的合法状态和一些导致不合法的额外元素。

如果不保证找到的 更小的合法状态 + 导致不合法的状态 的计算方法使得这个合法状态不能再扩大，也就是说在加入不合法状态的时候有可能在某段时间内是合法的，那么此时实际上会算重，此时对应的情况就是需要使用二项式反演之类处理，或者借助 主旋律题解中 提到的推容斥系数的思考。

虎年 final T2 可重集计数 <a href="https://www.fzoi.top/problem/6637" target="_blank" rel="noopener nofollow">https://www.fzoi.top/problem/6637</a> 后面有具体写。

如果保证用以转移的 更小的合法状态 + 导致不合法的状态 使得这个合法状态是极大的，那么容斥系数全是 -1

<a href="https://www.fzoi.top/problem/6904" target="_blank" rel="noopener nofollow">20230205 T1 异或集合</a> 首先我们利用 取值均匀 的技巧求解 fi 表示 i 个数字使得 \oplus ppc = k 的方案数。一个不合法状态总是由  合法的 i 个不同的 ppc = k 的数字 + j 对盈余的数字（这 j 对在 i 个里面取）+ k 对相同数（这 k 对值域和那 i 个无交）。所以很自然的，我们设 $g_{i, j}$ 表示选 i 个数不同数，盈余 j 对的方案数，那么 $g_{i, j}f_i$ 就是“合法的 i 个不同的 ppc = k 的数字 + j 对盈余的数字”；设 $h_{i, j}$ 表示选出 i 个不同数，一共选出了 j 对。


$$
\begin{align}
g[i][j] \gets g[i - 1][j - k]\binom{i + 2 * j - 1}{2 * k} \\
h[i][j] \gets h[i - 1][j - k]\binom{2 * j}{2 * k} \\
f[i] -= f_j g_{j, k} h_{l, (i - j)/2 - k} \binom{n - j}l \binom{i}{j + 2 * k}
\end{align}

$$

<hr>

<blockquote>

<strong>围绕基准点构造一个整体</strong>

——算法竞赛进阶指南

</blockquote>

<a href="https://atcoder.jp/contests/abc253/tasks/abc253_h" target="_blank" rel="noopener nofollow">ABC253EX</a> <a href="https://www.cnblogs.com/wyb-sen/p/16327493.html" target="_blank">题解</a>

<a href="https://www.fzoi.top/problem/1854" target="_blank" rel="noopener nofollow">「NOI2001」陨石的秘密</a> 考虑去重的问题，ss 串一定可以想成是 $(a)(b)[c]{d}(e)...$ 这样的形式，那么我们枚举 a 和 $(b)[c]...$ ，即以 a 为基准点，考虑在 a 外面套什么括号即可

[AGC028D] Chords 详见 <a href="https://www.cnblogs.com/wyb-sen/p/16475686.html" target="_blank">2022 HL 集训</a>

<hr>

通过思考对于合法情况的判定性问题从而在构造过程中计数答案，同时可以达到去重的效果

<blockquote>

计数中，最常出现的问题便是数重或数漏。比如要求数合法的元素个数，但一个元素可能有多种合法的方式，那么对合法方案计数就会数重。<br>

一种解决方法是，把合法元素唯一对应到一种合法方案上，相当于添加了限制条件。这样就不会出问题了。

—— pb

</blockquote>

其实 pb 这种思想和上面“围绕一个基准点”很像，比如说 “钦定贡献在左端点时计算到” 之类

CF1605F PalindORme：神仙转化题！本道题的关键在于找到“唯一确定”的东西进行计数，具体的，把一个不合法的情况，拆成一个极大的合法情况+一些不会算重的贡献。

[AGC008F] Black Radius：对于一个黑色连通块，钦定贡献在 (x, d) ，d 最小的时候被计算一次。

[20220718海亮NOI集训] 数数 详见 HL集训总结

<a href="https://uoj.ac/problem/748" target="_blank" rel="noopener nofollow">UNR#6 DAY1T2</a> 详见 2022 8 月总结

<a href="https://www.luogu.com.cn/problem/AT3871" target="_blank" rel="noopener nofollow">AGC021E ball Eat chamelemons</a> 详见 2022 8 月总结

<hr>

对于括号序列的计数套路：

by <a href="https://www.luogu.com.cn/user/85682" target="_blank" rel="noopener nofollow">绝顶我为峰</a>：记录 $f_{l,r,0/1}$ 表示区间 $[l,r]$ 是合法括号序列，上一次转移是外面套一层括号/两个区间拼起来的方案数，然后转移直接按照括号序列的递归定义转移即可，注意两个区间拼起来的时候钦定第一个区间必须是从 $f_{l,\star,0}$ 转移过来，这样才能保证不算重。

方法2：考虑通过数量的分配，在 DP 过程中实际上确定括号的匹配。比如 <a href="https://www.luogu.com.cn/blog/_post/538198" target="_blank" rel="noopener nofollow">Bracket Insertion</a> <img src="https://img2024.cnblogs.com/blog/2221225/202402/2221225-20240227211855787-1259250944.png" alt="" loading="lazy">

<h3 id="没有负环可以跑差分约束johnson">没有负环=可以跑差分约束/Johnson</h3>

<a href="https://www.luogu.com.cn/remoteJudgeRedirect/atcoder/agc036_d" target="_blank" rel="noopener nofollow">[AGC036D]Negative Cycle</a>

<h3 id="插眼法">插眼法</h3>

一个提到很多次，但是一直迟迟没有记录的方法。

CF1789F Serval and Brain Power nb “根号”分治题。设循环节长度 len，循环次数 x，答案是 len * x，答案显然小于等于 |s| = 80，看到 \times，DNA 动了。考虑循环次数为 x = 1,2,3，我可以暴力把原字符串划分成 x 段，然后跑 LCS，这样实际上包含了 x = 4 的答案。对于 x &gt;= 5，len &lt;= 16，那么 <strong>把序列每 16 个看成一组</strong>，容易发现任意一种 len &lt; 16 的分组方案中的每一组，一定存在某一个子区间使得是 len = 16 的某一组的字串。那么我们只需要 5\times 2^16 枚举循环节，对于每一个循环节暴力判定即可。

CF500F New Year Shopping：小清新 DP。首先有暴力线段树分治做法，但是关注到 p 是常数，且每次询问等价于问 [a - p, a] 时间内加入的物品；于是可以使用 插眼法，每 p 个插一个眼，那么任何一个询问区间刚好跨过一个眼，对于每一个眼预处理向左向右的 DP，合并一下就好。

<h3 id="分块处理区间询问">分块处理区间询问</h3>

对于一个区间询问，可以分成 散块到自身 整块自身 散块到整块 等等结合题目本身性质的询问。

<a href="https://www.fzoi.top/problem/6450" target="_blank" rel="noopener nofollow">BZOJ2122 工作评估</a>  块外进块内结束、块外进块外出、块内开始块内结束、块内开始块外出 四种贡献。

<a href="https://www.luogu.com.cn/problem/solution/CF765F" target="_blank" rel="noopener nofollow">Souvenirs</a> <a href="https://www.luogu.com.cn/blog/_post/425397" target="_blank" rel="noopener nofollow">OneZzz6174</a>

<h3 id="取值均匀">取值均匀</h3>

n 个无关变量的取值范围一样，取值等概率随机，这可以被称为他们取值均匀。

一个应用可以被叫做：单独出一个数字出来控制奇偶性

<a href="https://www.fzoi.top/problem/6904" target="_blank" rel="noopener nofollow">20230205 T1 异或集合</a> 令 n &lt;- n + 1, 首先需要求解出 $f_i$ 表示使用 i 个取值 [0, n) 的数，异或和的 ppc 是 x 的方案数。设选出的数 ai，枚举 $i = lg((\min_i a_i) \oplus n)$，即从高向低第一个二进制位使得存在 ai 没有顶住 n 的上界；枚举 j 表示有 j 个数这一位选 0（不顶上界），l 个数这一位选 1（顶），注意所有数字在 i + 1...lg(n) 位都是顶住了的。

设 rst 表示 0...i - 1 位需要的 ppc 数量，显然 $rst = k - (l \&amp; 1) - [(j \oplus l)\&amp; 1]ppc(n &gt;&gt; (i + 1))$


$$
f[j + l] \gets \binom{i}{rst} \binom{j + l}{j} (2 ^ i)^{j - 1} ((2 ^ i - 1) \&amp; n) ^ l

$$

其中之所以是 $(2 ^ i)^{j - 1}$，其实就是先让这 j - 1 个数字随便取，然后留下来一个数，由于取值均匀，所以总可以找到一个办法，使得 ppc = rst。

一个问题是会不会存在多个方法，是的，所以我们 $\binom {i}{rst}$

<blockquote>

线性基中组合成的 $2^{|B|} 种异或和出现的次数一样且每个数出现的次数都为 $2^{n - |B|}$

</blockquote>

证明：每一个线性空间中的数，可以唯一的被线性基中的一个子集表达。考虑 2^{n - |B|} 种方法选出一个子集，假设这个子集的异或和为 x，对于固定的 C，一定能够在线性基里面找到唯一的 y ，使得C = x ^ y。现在我们已经说明了下界，显然这个下界就是上界。

<a href="https://www.luogu.com.cn/problem/P4869" target="_blank" rel="noopener nofollow">P4869 albus 就是要第一个出场</a> 有了上述结论，直接乱杀

<h3 id="on-第-k-大">O(n) 第 k 大</h3>

[2023 集训队互测 R5T1] 在路上：对 快速排序思想 O(n) 求第 k 大 的利用 + 带权随机。

<h3 id="随机权值">随机权值</h3>

集合 HASH：CSPS2022T3 星战 共价大爷游长沙 <a href="https://www.fzoi.top/problem/6972" target="_blank" rel="noopener nofollow">H 神的序列</a> 出现奇数次 $\to$ 异或和

<a href="https://www.fzoi.top/problem/6815" target="_blank" rel="noopener nofollow">CF1746F Kazaee</a> <a href="https://www.luogu.com.cn/blog/_post/501791" target="_blank" rel="noopener nofollow">题解</a>

<a href="https://www.luogu.com.cn/problem/CF1622F" target="_blank" rel="noopener nofollow">Quadratic Set</a> 首先可以构造出 n - 3 的答案，那么只需要判断某一对 i,j 不选/某一个 i 不选是否可行，这个可以给每一个素数 rand 一下集合 hash 判断。

<strong>随机矩阵</strong>

<a href="https://www.fzoi.top/problem/7432" target="_blank" rel="noopener nofollow">20230720 T4</a> 只要找到某种二元操作，使得其不满足交换律，满足结合律，有逆元（有单位元），那么只需要给 0，1，2 分别赋值为三个随机值，只需要检查最后的答案的是否是单位元即可。发现这就是矩阵乘法。

我们还可以使得这个矩阵满足特殊的性质以更符合题目的要求，比如在 <a href="https://www.fzoi.top/problem/7252" target="_blank" rel="noopener nofollow">[GYM101754A] Letters Swap ( Yandex Algorithm 2018 Round 2 A )</a>，我们构造一个矩阵，满足 $A \times A = I$。

<h3 id="dijkstra-维护最短边">Dijkstra 维护最短边</h3>

适用于不是非常方便/（优化建图从而）需要从DS中取出一条边对应的点的时候，如果在优先队列里面维护边，那么每一个点只会从 DS 中取出一次，这样就可以均摊了。

[NOI 弹跳]

<a href="https://www.luogu.com.cn/problem/P7214" target="_blank" rel="noopener nofollow">[JOISC2020] 治療計画</a> 详见 2022 10 月总结。

[2023 集训队互测 R6T3] ：建立 AC 自动机，限制是一些点不能走。在 AC 自动机上 DP，即 $dis[x][y]$ 表示在 AC 自动机上 x，在原图上的 y 的最小 dis。需要可持久化线段树建立 AC 自动机，需要用 dijkstra 经典技巧：[NOI]弹跳，堆中维护边，每次从 ds 中取出点，每一个点只会被取出一次。

<h3 id="dijkstra-处理-dp-后效性">dijkstra 处理 DP 后效性</h3>

挺经典的思想，但是一直没有写下来。

记得有某经典期望题也使用了这个技巧，但是我忘了是哪道了。

<a href="https://www.fzoi.top/problem/8295" target="_blank" rel="noopener nofollow">20240222 隧道 tunnel</a> 发现最大的问题在于可能会来回跳然后就 G 大了，也就是说 DP 有后效性无法转移。由于 $v_i$ 的边是连向所有点的，所以我们考虑直接在求解最短路的过程中加入这些边的影响。由于 dijkstra 每次从堆中第一次取出一个点的时候即确认了这个点的最短路，那么注意到对于一个 $v_i\neq 0$ 的 $i$，如若 $i + 1$ 还没有取出，那么 $i + 2$ 不可能因为 $v_i$ 的原因而取出，因为即使假如因为 $v_i$ 导致需要取出，也一定是 $i + 1$ 先出。那么，考虑每次取出 $i$ 的时候，需要更新左/右侧第一个最短路还没有确定的点；比如，设第一个左侧点是 $j$，此时需要用 $[j + 1, i]$ 中的 DP 值进行更新，（由于正着做和反着做有区别，所以不能把所有点都插到同一个李超线段树里面），那么只需要每次取出 $i$ 的时候，把 $i$ 的李超线段数树和 $i-1$ 的合并即可。

<h3 id="考虑-dp-组合意义">考虑 DP 组合意义</h3>

[AGC013E] Placing Squares

很容易列出 DP 方程，$f_i = \sum f(i - k)k^2$ 。<strong>考虑 DP 组合意义</strong>， $k ^ 2$ 的意义就是在这段区间中放两个可重叠的黑白球的意思，注意这里球是有颜色的

所以，设 $g_{i, j}$ 表示当前做到位置 $i$，$i$ 所属的段已经放了 $j$ 个球。注意每一段放且必放两个不同色


$$
g_{i, 0} = g_{i - 1, 0} + [i - 1 \notin X]g_{i - 1, 2} \\
g_{i, 1} = 2g_{i - 1, 0} + g_{i - 1, 1} + [i - 1 \notin X]2g_{i - 1, 2} \\
g_{i, 2} = g_{i - 1, 0} + g_{i - 1, 1} + g_{i - 1, 2} + [i - 1 \notin X] g_{i - 1, 2}

$$

$X$ 表示不能放的位置的集合。现在可以暴力矩阵快速幂

[CF917D] Stranger Trees (hard version)

问题变成计算$ \sum\prod siz$

Naive 地，设 $f[u,i,j]$ 表示以 u 为根的子树有 i 个联通块，包含 u 的联通块大小为 j , O（n^3)

Trick ：<strong>各部分数量之积等于在每一部分选一个元素的方案数</strong>，即考虑组合意义

改设 f[u,I,0/1] 表示以 u 为根的子树中，I 个联通块，u 所在联通块是否选点的方案数，O（n^2)

gym 102769 H Holy Sequence

先考虑如何求 f_{i, j} 表示 i 个数，前缀最大值为 j 的方案数

$f_{i, j} = f_{i - 1, j - 1} + j \times f_{i - 1, j}$

即要么当前位置是新的前缀最大值，要么当前位置随便填一个。

其实可以发现这个就是第二类斯特林数，思考这个 dp 构造方案的过程，发现和 <a href="https://www.luogu.com.cn/problem/P4609" target="_blank" rel="noopener nofollow"> [FJOI2016]建筑师</a> 是一样的。

那么容易 O(n^4) 算出：f_{ijkh}，前 i 个点，前缀最大值为 j，数字 k 出现 h 次的方案数

这样是愚蠢的，考虑组合意义，参照上方 [AGC013E] Placing Squares ，设 f_{i, j, k, 0/1/2} 表示 k 钦定了几个位置。可以做到 n^3。

进一步优化 需要注意到一旦出现了第一次 k，那么后面任意位置都可以填 k，也就是说我完全可以枚举第一个 k 钦定在哪里。

假设第一个 k 钦定在位置 i，那么需要计算的是，g_{i, j} 表示在长度为 i 的后缀的开头放一个 j，这个后缀放数的方案

有 g_{i, j} = g_{i - 1, j + 1} + j * g_{i - 1, j}，其实就是刚刚的过程翻过来。

剩下的贡献容易统计。

<a href="https://www.fzoi.top/problem/7317" target="_blank" rel="noopener nofollow">arc110d</a>

[<a href="https://www.fzoi.top/problem/7323" target="_blank" rel="noopener nofollow">JLOI2015] 骗我呢</a>

<strong>考虑 DP 现实意义</strong>

DP 转移其实可以看做是 DAG 带权路径计数。

DAG 的作用实际上就是把本来不是非常清晰的 DP 转移过程刻画出来，使得我们可以更好地观察来优化

或者说，其实是提示我们 DP 优化不动的时候，不妨回头从一个总体的角度看一看这个 DP 到底做的是什么事情。去考虑一下每一条贡献的路径对应了一个怎样的方案，而这个方案又可以怎么样地去理解，去等价地构造（eg.构造补）

[CF506E] Mr. Kitayuta's Gift <a href="https://www.cnblogs.com/wyb-sen/p/16537979.html" target="_blank">题解</a>

[LibreOJ β Round #7] 匹配字符串 <a href="https://www.cnblogs.com/C202044zxy/p/15230886.html" target="_blank">C202044zxy</a>

<a href="https://www.fzoi.top/problem/5963" target="_blank" rel="noopener nofollow">[BZOJ 3329] Xorequ</a>  最后填写的数可以被划分成 $0$ 或者 $(10)_2$ 这样的段，所以设 $f[i]$ 表示长度为 $i$ 的数的划分方案，那么 $f[i] = f[i-1] + f[i - 2]$

<a href="https://www.luogu.com.cn/problem/P7214" target="_blank" rel="noopener nofollow">[JOISC2020] 治療計画</a> 详见 2022 10 月总结。

T1 2023集训队互测R1T1 astral birth

<h3 id="正反做两遍">正反做两遍</h3>

计数题可以考虑先拆条件，把需要计数的部分拆成多个独立的互不影响的子问题，比如把 max(a,b)=c 拆成 a &lt; b $$ b = c 和 b &lt; a &amp;&amp; a = c，这样的问题会具有一定的对称性而往往可以概括为正反做两遍。

<a href="https://www.cnblogs.com/Hs-black/p/13303148.html" target="_blank">连续值域区间计数</a> 中的 CDQ 解法，CDQ 本身也是一种条件的拆解。

<hr>

点分治的时候也常常用到正反做两遍算贡献的方式。

<hr>

一种 nb 的换根 DP 方式，适用于想要求出 g[fath] 表示 fath 走 u 子树以外的答案是困难的：

首先可以用常规方法求出 f[u] 表示往子树内走的答案。关注到换根 DP 的特点是仅有从根节点到 u 这条链上的点的贡献不是 f，把贡献拆成两部分：在到根路径上的/其他的

第一个部分找一个 DS 解决，第二个部分考虑类似点分治合并子树的手法，考虑搞一个 DS，每次 dfs 从某个点推出的时候就把这个点的信息加入这个 ds，那么实际上，当我求解到 v，ds 中就保存了所有的 dfn 序 &lt; dfn[u]，且非到根路径上的点的信息，正反做一遍即可。

<hr>

<a href="https://www.fzoi.top/problem/6955" target="_blank" rel="noopener nofollow">20230221 T2 强连通</a> 若 (u, v) 在同一强连通分量内，则强连通分量数会改变当且仅当反转 (u, v) 后 u 不能到达 v。若 (u, v) 不在同一强连通分量内，则强连通分量数会改变当且仅当反转 (u, v) 后 u 能到达 v。

此时需要对于每一个 u,v ，求出删去这条边之后 u 是否可以到达 v。处理方式是考虑顺序遍历 u 的出边，对于遍历到的出边，做一次 bfs 的过程，把新的可以到达的点标记，如果在刚刚遍历到 (u,v) 的时候，v 已经被标记了，那么说明是可达的。再逆序做一遍上面的过程即可。使用 bitset 优化，复杂度 n^3/w

[20230308T3 hole] 题目可以转化为树的直径，枚举树的中心，可以用简单 ds 维护。具体的，二分答案之后，枚举一个点，使得左侧所有到他的距离 $\le\lfloor\frac d2\rfloor$，右侧 $\le\lceil\frac d2\rceil$，倒过来再做一遍

<h3 id="确定图形态的交互">确定图形态的交互</h3>

都是先找到一个判定方法，然后分治判定之类。

T2 2023集训队互测 R1T3 树

<h3 id="矩阵树定理">矩阵树定理</h3>

计算的是 $\sum_T\prod_{e\in T} w_e$。

无向图：度数矩阵-邻接矩阵 去掉第 n 行列的行列式

有向图：入度矩阵-邻接矩阵 -&gt; 外向树；出度 -&gt; 内向树；根在 x，去掉 x 行列

可以处理重边。

求边权和： <a href="https://www.luogu.com.cn/problem/P6624" target="_blank" rel="noopener nofollow">省选联考 2020 A 作业题</a> 经典技巧，令 $w_e = w_ex + 1$，边权和是一次项系数，重定义 pair 加减乘除即可。

<h3 id="dijkstra网络流-维护-dp">dijkstra/网络流 维护 DP</h3>

其实 dijkstra 和 网络流 是可以类比的，因为平面图转对偶图之后，最小割等价于最短路。

大概意思是发现 DP 转移的过程本质上是一个求最短路/最小割的问题，然后直接艹。

PTZ summer 2020 Day6 J Setting Maps：高超的网络流建模。一个 DP：假装枚举了一个方案，计算 fi 表示到达 i 的最少步数，思考选择方案对于 fi 的影响，可以得到网络流的建模。首先对于 k=1 的情况，可以每个点拆点，入点向出点连 c ，原图中的边连 +inf，实际上是求最小割。对于 k&gt;1 的情况，考虑建 k 层图，每层点之间和上面一样，每层点向下一层对应的出点连 +inf，表示这个点被割掉了，不得不到下一层。s 向 (0,S) 连边，(x,E) 向 t 连边。可以发现如果一条路径有少于 k个选择的点，那么这条路径一定对应 s 到 t 的流量。所以我们一直割，直到二者不连通。

<a href="https://www.fzoi.top/problem/6943" target="_blank" rel="noopener nofollow">20230212 T1 内鬼</a>

<h3 id="状态设计">状态设计</h3>

插头 DP 的状态设计是 尽可能得缩减所需要的状态，尽可能地只保留精华信息 来优化状压的经典

<a href="https://www.luogu.com.cn/problem/P5056" target="_blank" rel="noopener nofollow">P5056 【模板】插头dp - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)</a> 发现需要保留现在递推到的位置、轮廓线上每个点是不是下插头、轮廓线上联通状态。注意到只有 $m$ 个下插头和可能的 $1$ 个右插头会对下一行有影响，所以只记录下插头联通状态。表示连通状态常用的方法有用点对表示联通、把同一个连通块中的点赋同一个值，选用第二种。发现在本题中由于需要形成回路，所以插头对应的路径不相交，也就是说每一个连通块里面实际上只有恰好  $\rm 2$ 个点，所以可以使用括号序列表达。经过这一通状态精简，我们可以使用 $f(i,j,s)$ 表示当前做到 $i$ 行 $j$ 列，下插头联通状态为一个三进制数 $s$ 。为了方便，可以用四进制代替三进制。暴力递推即可，细节题+小码量。

有时候 DP 会出现算重的尴尬局面，一种处理方式是容斥一下，当然也可以通过优雅的状态设计解决这个问题

[AGC028D] Chords 详见 <a href="https://www.cnblogs.com/wyb-sen/p/16475686.html" target="_blank">2022 HL 集训</a> 挖掘分析，想清楚什么是主要矛盾，什么是可以 DP 的。

[CF506E] Mr. Kitayuta's Gift <a href="https://www.cnblogs.com/wyb-sen/p/16537979.html" target="_blank">题解</a> 高明的性质观察、状态合并。

一种思考是<strong>缩减状态</strong>，来使得转移的路径唯一，即可能本来有多种转移路径，但是缩减状态之后，没有办法走其他的路了。

<a href="https://www.fzoi.top/problem/6526" target="_blank" rel="noopener nofollow">20221021T3</a>  <a href="https://cdn.luogu.com.cn/upload/image_hosting/uknkyier.png" target="_blank" rel="noopener nofollow">TX</a>

<h3 id="重设状态状态优化的出发点">重设状态/状态优化的出发点</h3>

一个重要的“交换状态和答案”单开了一个标题写。

一种经典的状态更换是 dp[l][r] 然后总结出 l, r 之间的关系，然后变成 dp[l + r]。

<a href="https://www.fzoi.top/problem/6526" target="_blank" rel="noopener nofollow">20221021T3</a>  <a href="https://cdn.luogu.com.cn/upload/image_hosting/uknkyier.png" target="_blank" rel="noopener nofollow">TX</a> 固定 |j - k| &lt;= 1 使得不会算重。

<a href="https://atcoder.jp/contests/arc112/tasks/arc112_e" target="_blank" rel="noopener nofollow">ARC112E Cigar Box</a> 经过一定的性质分析，得到 DP。此时的 dp 是 dp[i][l][r]，优化成 dp[i][l + r] \binom(l + r, l) = dp[i][l][r]

<hr>

还有一类重设状态的出发点在于当前状态实在没有优化空间了。此时对于新状态的发现需要一点勇气和想象力，一些可能你认为必须的东西实际上有可能是不需要的。

<a href="https://www.fzoi.top/problem/5713" target="_blank" rel="noopener nofollow">CF708E Student's Camp</a> 朴素的状态时 dp[i][l][r]， 但是实际上 l 可以去掉，通过容斥技巧体现，然后前缀和干掉。

<hr>

仔细分析，发现很多转移方式雷同，此时思考是不是状态设的有点累赘了。

<a href="https://www.fzoi.top/problem/6580" target="_blank" rel="noopener nofollow">20221024T3</a> 一个 sb 的状态是 f[i][j] = v 表示前 i 个换了 j 次，a[i] 的最小值（属于是交换答案状态上头了，反而扼杀了自己优化的可能性）。实际上观察之后可以发现只需要设 f[i] 表示 i 固定不变的代价。

<hr>

<strong>状态转移形如 f[i] -&gt; f[i * j]</strong>， 答案在 f[&gt;=m] 的时候，可以改设状态为 f[i] 表示还需要乘上 i 才能 &gt;=m。这样的话，状态数只有 \sqrt。我们可以把这个 trick 叫做 <strong>喇嘛哑巴trick</strong>！

<a href="https://www.fzoi.top/problem/6652" target="_blank" rel="noopener nofollow">喇嘛哑巴和喇叭鳎犸</a>

<a href="https://codeforces.com/problemset/problem/1801/F" target="_blank" rel="noopener nofollow">CF1801F Another n-dimensional chocolate bar</a>

<hr>

认真思考状态里面记录的东西到底用在了什么地方，探究一个状态所记录的信息之间是否有什么联系，仔细分析，约束转移，大胆削减状态。

[EGOI2021] Lanterns / 灯笼

<hr>

<a href="https://www.fzoi.top/problem/8262" target="_blank" rel="noopener nofollow">20240109T3 cut</a> （fzoi 内置题解）天下无敌巧妙的状态设计。注意到平凡的状态设计 $f[l][r]$ 本身成为了时间复杂度的瓶颈，考虑改变答案的计算方式。本来的计算方式是，$ans= \sum_t 时刻\ t\ 存活数量$，<strong>注意到</strong>，<strong>尝试用数学方法刻画出当前状态</strong>，令


$$
F_t(i)=
\begin{cases}
t,  i 当前存活\\
t_i, i 已经 G 了，t_i 表示 i 死的时刻
\end{cases}

$$

那么 $ans=\sum_{i=1}^n F_{\infin}(i)$，$\sum_{i=1}^nF_0(i)$ 的计算是容易的，只需要计算 $t=0\to \infin$ 的过程中每一个 $t\to t+1$ 的增量变化即可。

这着实是一个 ad-hoc 的思考方法，这样思考的好处在于，类似于势能法算期望的技巧（不需要保证推导过程中的每一步都是充要的），在这里，我只需要构造出一个 $F$，使得：

<ul>
<li>$F_{\infin}$ 和原来答案相同</li>
<li>$F_{t}\to F_{t+1}$ 的转移是容易的</li>
</ul>

那么就可以了。

其实在本题中，我们刻画时间的方式是关注当前已经 G 了那些东西，且 G 的一定是一个区间 $L,R$，故下面用 $L,R$ 代替 $t$，可以说现在的构造是：


$$
F_{L,R}(i)=
\begin{cases}
t,  i 当前存活\\
t_i, i 已经 G 了，t_i 表示 i 死的时刻
\end{cases}

$$

而这里面的 $t$ 具体是多少已经不重要了，因为我们只关心增量。

注意到到最后所有的 $i$ 都会取 $t_i$，所以对于第一行可以魔改，有这样一个构造：


$$
F_{L,R}(i)=
\begin{cases}
t+dis(p,i),  i 当前存活\\
t_i, i 已经 G 了，t_i 表示 i 死的时刻
\end{cases}

$$

其中 $p$ 是目前所在的位置 $L$ 或者 $R$。

注意到这样的好处在于，当我从 $p=R$ 移动到 $R+1$ 的时候，所有 $F_{L,R+1}(i\ge R+1)=F_{L,R}(i)$

这意味着，对于一个固定的 $L$，所有的 $R$ 对应的 $F_{L,R}$ 都一样。

注意到会出现的 $L$ 都 $\le k$，会出现的 $R$ 都 $\ge k$，

设 $f_i$ 表示 $\sum_j F_{i}(j)$

比较最开始所设计的 $f[l][r]$，发现上面的一系列操作实际上是<strong>通过一个巧妙的状态设计，使得对于一个 $l\le k$，$f^{\prime}[l][r]$ 都一样，那么自然只需要记录 $l$ 而不需要记录 $r$</strong>

感觉非常震撼，这是一个从来没有出现过的角度，如果想要直接从 $f[l][r]$ 里面砍一个维度是不可能的，因为 $r$ 记录了一些重要的信息，但是居然通过这样一个状态的转化，就使得 $r$ 失去了作用。感觉这个可能会对应着更高思考维度上的一些事情。

接下来，考虑 $f$ 的转移，自然的，可以认为转移过程中每一个点都是 $p$ 行径的拐点，那么：


$$
f_j(j\ge k)=\min_{i\le k}\{f_i+(i-1)dis(i,j) \}

$$

另一侧同理。

注意 $dis$ 可以拆开，然后转移式会成为一个斜率优化的形式。经过分析，发现只需要单调栈维护下凸壳即可。$O(n)$

<h3 id="对整除分块的敏感性">对整除分块的敏感性</h3>

看到存在下取整的东西就要开始考虑整除分块的结论了。

<strong>状态转移形如 f[i] -&gt; f[i * j]</strong>， 答案在 f[&gt;=m] 的时候，可以改设状态为 f[i] 表示还需要乘上 i 才能 &gt;=m。这样的话，状态数只有 \sqrt。

<a href="https://www.fzoi.top/problem/6652" target="_blank" rel="noopener nofollow">喇嘛哑巴和喇叭鳎犸</a>

<a href="https://www.fzoi.top/problem/6733" target="_blank" rel="noopener nofollow">食物</a> 发现我们只关注 \frac{n}i，所以只用维护 \sqrt n 个位置。

<h3 id="boruvka">Boruvka</h3>

遍历所有边，对于每个连通块，找到从这个连通块出发，不在最 小生成树中的、到达别的连通块的最短边。 全部找完后，遍历所有点，将找到的边加入最小生成树中。

（可能出现两个连通块互连的情况，那么这时在第一个连通块连完这条边后，标 记一下，说明该边已被加入最小生成树，下一次弹掉即可)

总时间复杂度是 $O((V+E) log V)$

一般不用于跑裸的 MST，而仅仅是借鉴这种思想，正确性可以理解为 ”多路增广的 Kruskal “。

<blockquote>

我们发现，它唯一的难点在于如何给每个连通块找到它连出去的最小边。暴力是O(m)，但我们会发现，有许许多多的题目，边的数目非常大，而边权又有某种规律。

这时候，就可以套上各种各样的数据结构/dp/分治来维护它了。采用Boruvka以后，基本就变成了另一道全新的题，爱出什么出什么。

仔细思考，prim直接n^2，Kruskal初始就要对边排序，都不适合套上太多东西。

将最小生成树分解为找最小边的问题，正是Boruvka最有趣的地方。<br>

—— <a href="https://blog.csdn.net/shiveringkonnyaku/article/details/88691601" target="_blank" rel="noopener nofollow"><em>shivering</em></a>

</blockquote>

<a href="https://www.luogu.com.cn/problem/CF888G" target="_blank" rel="noopener nofollow">CF888G Xor-MST</a> 对点权排序后（不用启发式合并了），考虑把点从小到大插入一颗 Trie 树，显然，对于每一个有两个儿子的点，我们可以将其两颗子树看作是说两个联通集合，那么现在的目标既是说在这两个集合之间找到一条最小的边。遍历左子树，在右子树寻找。当前层做完后，递归到儿子进行。

<h3 id="搭楼梯dp">“搭楼梯”DP</h3>

需要维护这样的一个东西：

```c++
***
******
******
********
********
********
**********
**********
```

常见的手法是整体砍 $1$ ，那么所有情况都可以被化归到存在 $a = 1$，可以干掉一些枚举量来方便计算答案

对于不同的题目，可以从左往右做，采用上面的手法化归；也可以竖着做，枚举在上面/下面塞一行，计算方案数

常见状态是 $f_{i,j}$ 表示前 $j$ 个位置一共填写了 $i$ 个 $\star$

实例1：N^2 递推求解 $f_{i, j}$ 表示选了 i 个正整数，和为 j 的方案数。

递推式：$f_{i, j} = f_{i - 1, j - 1} + f_{i, j - i}$

表示在右边新增一个 1，或者给当前所有数字都在下面塞一层。

实例2：N\sqrt N 递推求解 $g_{i, j}$ 表示选了 i 个不同的正整数

递推式：$g_{i, j} = g_{i - 1, j - i} + g_{i, j - i}$

表示新增一个 i，或者给整个序列确定之后的最大的 i 个数字 + 1。

可以这样理解：这个 DP 分为两部分，第一部分是搞出一个形如 1,2,3,4,5... 的序列；第二部分是每次选择一个后缀 + 1，很有道理。

sqrt 的来源是 g[i][0... (i+1)i/2] 没有值

上述两个实例被应用到 <a href="https://www.fzoi.top/problem/6637" target="_blank" rel="noopener nofollow">虎年finalT2 可重集计数</a>

<a href="https://www.luogu.com.cn/problem/P1025" target="_blank" rel="noopener nofollow">P1025 树的划分</a>

<a href="https://www.cnblogs.com/wyb-sen/p/16308996.html" target="_blank">「CH5105」Cookies</a>

[WF2018] Gem Island  转化成了 $\sum_{\sum a = d}\sum min(b_j, r)$ ,$b_i = \sum [a_j = i]$  之后，我们记 $f_{i, j}$ 表示 $j$ 个数的和为 $i$ 的方案数, $g_{i, j}$ 表示在此情况下 $\sum\min{b_x,r}$ 的和。

枚举 $b_1 = k: $f_{i, j} = \sum_{k = 0}^j\binom jk f_{i - k, k}$， $g_{i, j}$ 相应转移。即往下面塞一行

<hr>

这个东西甚至加上一点转移系数也是可以算的，比如在 <a href="https://www.fzoi.top/problem/7529" target="_blank" rel="noopener nofollow">20230821 T3 君子兰</a> 中，对于一个高度相同的段，会有 $ifac_{段长}$ 的贡献。

此时，将 DP 方式改为：

```c++
for(int i = 1; i &lt;= n; ++ i)
    for(int j = i; j &lt;= m; ++ j)
        for(int l = 0; l &lt;= i; ++ l)
            f[i][j] += f[i - l][j - i] / fac[l];
```

相当于说，当 $l = 0$ 的时候，是单纯增加高度。否则，每次新增一段的时候，一定要确认这一段的长度。

<h3 id="主元法">主元法</h3>

有时候，对于一些 DP ，我们可能需要高斯消元

但有时候这样复杂度会炸，我们可以考虑解出所对应的系数

解系数类型一般有一个特点，就是大多和一个特定的未知量相关

<a href="https://zoj.pintia.cn/problem-sets/91827364500/problems/91827368253" target="_blank" rel="noopener nofollow">ZOJ3329</a> <a href="https://www.cnblogs.com/wyb-sen/p/16254757.html" target="_blank">题解 </a>

有一个印象很深刻的技巧就是把这个方法放到树上去，但是不知道为什么好象一直没有记录。

<a href="https://www.luogu.com.cn/problem/CF1823F" target="_blank" rel="noopener nofollow">CF1823F Random Walk</a>

[CF963E]Circles of Waiting 主元可以是一坨/qd

<h3 id="多项式插值">多项式插值</h3>

有时候可以把 DP 状态看作是多项式系数，然后把它插出来

<blockquote>

构成一多项式需要大胆猜想，看到这种 $n ≤ 10^9$  就要往这方面想。

——hhoppitree

</blockquote>

<a href="https://blog.bill.moe/FCS-NOI2018-day1-equ/" target="_blank" rel="noopener nofollow">[FJWC2018] 残缺的算式 题解</a> 或 2022 HL 集训总结

<h3 id="倍增-fft">倍增 FFT</h3>

在某些动态规划题目中常常可以将 $n$ 转移到 $n+1$，或是转移到 $2n$，并且这两个转移都可以通过 FFT 来维护。

这个时候我们可以将 FFT 套上倍增以 $O(nlog^2n)$ 的复杂度通过。

CF755G PolandBall and Many Other Balls

CF773F Test Data Generation

<h3 id="组合数递推公式">组合数递推公式</h3>

很简单的，$\binom nm = \binom{n - 1}{m - 1} + \binom{n-1}m$

很困难的，注意观察一些柿子里面如果出现了组合数，那么常常可以通过利用这个递推公式，把某些玩意儿搞得可以递推，然后复杂度变成优雅的线性

<a href="https://www.fzoi.top/problem/4335" target="_blank" rel="noopener nofollow">CF755G PolandBall and Many Other Balls</a> 的容斥做法的最后一步。

<a href="https://www.luogu.com.cn/problem/CF932E" target="_blank" rel="noopener nofollow">CF992E Team Work</a>

<a href="https://www.luogu.com.cn/problem/P6667" target="_blank" rel="noopener nofollow">[uoj 269]【清华集训2016】如何优雅地求和</a>

<a href="https://www.luogu.com.cn/blog/_post/242572" target="_blank" rel="noopener nofollow">「TJOI / HEOI2016」求和</a>

<h3 id="树的拓扑序">树的拓扑序</h3>

求n个点的树的拓扑序列的个数：

随机序列有 n! 种，我们考虑计算这么多方案书中成功的概率

对于以 $i$ 为根的子树，要想让 i 排在所有子树中的点的前面，只有 $\frac 1{siz_i}$ 的概率先抽到 $i$

所以答案就是 $n! \prod \frac 1{siz_i}$

[CTS2019] 随机立方体  二项式反演之后，从限制少的位置（较大值）开始考虑，发现是树的拓扑序模型

<a href="https://www.luogu.com.cn/problem/P4099" target="_blank" rel="noopener nofollow">HEOI2013 SAO</a> 对于这类限定元素大小关系的序列/树等结构，我们可以进行以下容斥：将小于号视为正向边，大于号视为反向边，遇反向边是将其改为正向边并乘上容斥系数 -1 或断掉该边。这么容斥可行的原因是断边本质上与不限制大小关系相同，即 $f(a<b) =="" f(a?b)-f(a="">b)$</b)>。当一棵树所有边都为同一个方向的时候，其方案数为 树的拓扑序，即 $n!\prod\frac 1{siz_i}$

<h3 id="min-max-容斥">min-max 容斥</h3>

覆盖所有 = E(max(S))

min-max 容斥一个常见技巧是计算所有 $E(min(T))$ 的容斥系数和

[集训队作业 2018] 小Z的礼物 考虑 min-max 容斥 $= \sum (-1)^{|t|+1} \frac{the\   count\  of\  black\  block}{f(T)}$ f(T) 表示能够覆盖任意一个 T 集合中的黑格子的不同骨牌数量。2^600 不可接受， 考虑计算相同 f(T) 的容斥系数和，即<strong>将### ) 相同的统一计算</strong>，由于同时选择相邻两个黑格子会影响方案数，考虑状压 DP。可轮廓线 dp(i,j,S,f) 表示轮廓线上的黑格子是否在 T 集合中， f(T)=f 的方案数

<a href="https://www.luogu.com.cn/problem/P4707" target="_blank" rel="noopener nofollow">P4707 重返现世</a> <a href="https://www.cnblogs.com/wyb-sen/p/15831511.html" target="_blank">题解</a>

<h3 id="bitset-诈骗">bitset 诈骗</h3>

看着一些 n &lt;= 2000 的题，不一定是 n^2log，可能是 n^3/w

ABC287EX

<a href="https://www.fzoi.top/problem/6955" target="_blank" rel="noopener nofollow">20230221 强连通</a>

<h3 id="bitset-乱搞字符串匹配">bitset 乱搞字符串匹配</h3>

正解可能很神必，于是 std 常数很大，时间限制宽松，然后 bitset 一拳打爆！

<a href="https://www.cnblogs.com/alex-wei/p/bitset_yyds.html" target="_blank">Alex_Wei</a>

<h3 id="bitset-乱搞多维偏序">bitset 乱搞多维偏序</h3>

设处理 $k$ 维偏序问题。

发现，可以花费 $O(kn^2/w)$ 的时间处理出每一个位置可以转移到/被转移到的位置。

具体的：

<ul>
<li>处理可以被那些位置转移：开 $k\times n(值域)$ 个 bitset，然后做一个前缀或。</li>
<li>处理可以转移到的位置：初始 $reach[i] = (1 &lt;&lt; n) - 1$。枚举维度，按照此维度权值从大到小排序，遍历，每次 <code>reach[i] &amp;= tmp, tmp.set(i)</code>。</li>
</ul>

当然，仅仅得到这些肯定是没有做完的。

注意到一个关键性质，即每次只关心 可以被哪些位置转移 的 dp 最大值，所以有一个分治做法：

<blockquote>

对于 <code>solve(l, r)</code>，先计算完 $[l, mid]$，然后把 $[l, mid]$ 按 dp 值排序（重标号），然后计算出 $\forall i\in[mid + 1, r]$ 可以被 $[l, mid]$ 中哪些位置转移，执行 $_Find_first()$ 就好。

复杂度：$T(n) = O(\frac{n^2}w) + 2T(\frac n2) = O(n^2/w)$

</blockquote>

但是这个做法的问题在于，由于需要保证分治到 $l, r$ 的时候，bitset 的复杂度是 $O((r - l)^2 / w)$，所以 bitset 大小只能开到 $O(r - l)$，但是 bitset 的大小必须是一个常数

一个别扭的解决方案是 solve(0, 2^n - 1)，以保证每次使用的 bitset 的大小一定是 2 的幂次。

一个思考是对于 <code>bitset&lt;N&gt; B(m)</code>, 发现 $m$ 可以不是常数。但是问题在于不仅值域是变量，区间长度也是变量！

所以分治做法可以认为是 G 了。

有如下分块做法：

<blockquote>

设块长 B

对于同一块内部点，采用 $O(B^2)$ 的暴力 DP

计算完一块之后，$O(Bn/w + n)$ 处理出当前块内每一个点可以转移到的位置集合（遍历 1...n），然后类似 bitset 优化 bfs，$O(n + Bn/w)$ 做完所有转移。

总复杂度 $O(k(nB + n^2/B + n^2/w))$，取 $B = \sqrt n$ 就好。

</blockquote>

[HDU7301]King's Ruins

<h3 id="bitset-优化-bfs">bitset 优化 bfs</h3>

$O(n + m) \to \mathcal{O}(n + \frac mw)$

<h3 id="对询问分块">对询问分块</h3>

Dynamic Reachability

<h3 id="最小链覆盖-1">最小链覆盖</h3>

节点数-最大匹配 = 最小不相交链覆盖 = 最长不上升子序列  （<a href="https://blog.csdn.net/litble/article/details/85305561" target="_blank" rel="noopener nofollow">Dilworth定理</a>）

简单模型：<a href="https://www.luogu.com.cn/problem/P1020" target="_blank" rel="noopener nofollow">导弹拦截</a>

复杂模型：「JOISC 2016 Day 1」俄罗斯套娃 详见 2022 HL集训总结

但是不要忘记对于题目其他性质的分析 <a href="https://www.cnblogs.com/dixiao/p/15947305.html" target="_blank">Non-prime DAG</a>

<h3 id="操作可逆中间状态">操作可逆，中间状态</h3>

最简单的一个应用是 Floyd

XXI Open Cup GP of Krakow – H 由于操作是可逆的，对于判定状态 S 是否和 T 在同一个连通块的问题，可以寻找于 S 联通的一个优美的状态 M, S 和 T 的连通性与 M 和 T 的连通性等价。

这个漂亮的 M，显然是尽量把所有老鼠往叶子塞。所以我们每次把 S 的深度最深的老鼠塞到一个菊花里面，然后对 T 尝试是否可以塞进对应位置。如果成功，那么这个菊花显然可以在树上删去了，因为它不影响其他老鼠的移动。

这样，O(n^2)

<a href="https://www.fzoi.top/contest/359/problem/6037" target="_blank" rel="noopener nofollow">2024寒假模拟赛T1 移动箱子</a> 把会产生 rst 的堆通过逆操作合并起来。

<h3 id="同余最短路">同余最短路</h3>

<blockquote>

同余最短路是一种优化最短路建图的方法

通常是解决给定 m 个整数，求这 m 个整数能拼凑出多少的其他整数(这 m 个整数可以重复取)或给定 m 个整数，求这 m 个整数不能拼凑出的最小（最大）的整数。

——Ariel_

</blockquote>

思想：将问题分成两部分，求出一个部分的最优解然后和另一个合并

这个思想很像 meet in the middle ，在此处的题目条件下，就变成了同余的关系

模板：<a href="https://www.luogu.com.cn/problem/P3403" target="_blank" rel="noopener nofollow">P3403 跳楼机</a>

<h3 id="一道奇妙的期望题">一道奇妙的期望题</h3>

常见的， $f[i] = f[i - 1] * \lambda + 1$，但是在某些特殊情况下是不能 + 1 的，比如:

<blockquote>

给定 n 种颜色的球，第 i 种颜色的球有 $a_i$ 个，每步进行如下操作：<br>

依次不放回地选出两个球，把第一个球的颜色改为第二个球的颜色。当所有球的颜色相同时，过程终止。求期望步数。<br>

$1≤n≤2500，1≤a_i≤10^5$

</blockquote>

考虑枚举颜色 c，计算 f_i 表示场上现在有 i 个 c，期望多少步到达 $f_{\sum a_i}$

在位置 i 每步有 $p_i = \frac{i(s - i)}{s(s−1)}$ 的概率 +1 或 −1，1−2p_i 的概率不动，那么

$f_i = (f_{i - 1} + f_{i + 1})p_i + (1 - 2p_i)f_i + \red{1}$

这样的问题在于，$f_0$ 实际上是一个不合法的状态，计算的时候将 f_0 直接当作 0 显然是扯淡的。设应该加上 v_i，那么 v_i 的实际意义类似于数轴上一个点 x 每次等概率向左或者向右走，求走到 0 之前到达 s 的概率。

$v_0 = 0, v_s = 1, v_i = (v_{i - 1} + v_{i + 1})p_i + (1 - 2p_i)v_i $，可以预处理知道 $v_i = \frac is$

那么，只要用 v_i 代替 $\red{1}$，就可以把 f_{0} 视作 0 了，因为转移中已经去掉了所有会到达 0 的路径。

什么叫做去掉呢？

<blockquote>

v_i 还可以理解为，从 i 出发的所有路径（以 0 终止或以 s 终止）中，求出每条路径中每步概率的乘积作为权值，则等价于在这些路径中按照权值随机一条走，权值和为 1，其中能到达 s 的路径权值和为 v_i。也就是说，把从 i 开始走的这第一步，视作由随机出来的路径决定，则只有 v_i 的概率选择了能到达 s 的路径，这一步对期望步数的有效贡献就是 1×v_i

———— luogubot

</blockquote>

<h3 id="可重集计数的奇妙容斥">可重集计数的奇妙容斥</h3>

虎年 final T2 可重集计数 <a href="https://www.fzoi.top/problem/6637" target="_blank" rel="noopener nofollow">https://www.fzoi.top/problem/6637</a> :

<blockquote>

有多少个可重集 T 满足以下条件？

<ul>

<li>元素都是正整数。</li>

<li>元素和为 n。</li>

<li>元素个数（即 |T|）为 k。</li>

<li>每种元素出现次数不超过 m。<br>

例如，当 n = 4, k = 3, m = 2 时，T = {1, 1, 2} 满足条件。<br>

给定 n, mod，设 $f(n, m0, k0)$ 为 $m = m0, k = k0$ 时上述问题的答案模 mod 的值。请你求出 $S =\sum^n_{i=1}∑^n_{j=1} f(n, i, j)$ 的值。注意，f(n, i, j) 的值需要取模，但是 S 的值不需要取模。</li>

</ul>

</blockquote>

设 $f_{i, j}$ 为 $i$ 个数和为 $j$ 的方案数，$g_{i, j}$ 为 $i$ 个不同的数和为 $j$ 的方案数，则 $f$ 可以 $O(n^2)$ 求，$g$ 可以 $O(n\sqrt n)$ 求。容斥，


$$
ans(N, M, K) = \sum_k\sum_l(-1)^k g(k, l) f(K - k(M + 1)， N - l(M + 1))

$$

来估计一下 $K$ 的上界：假设 $K = mT$，也即我们能选完 $m$ 个 $1\sim T$，则 $n$ 是 $mT^2$ 级别的，所<br>
以 $T$ 是 $\sqrt{n / m}$ 级别的，所以 $K$ 是 $\sqrt{nm}$ 级别的，否则答案是 $0$。

因此第一维的大小其实是 $O(\frac{\sqrt{nm}}{m + 1})$，第二维的大小是 $O(\frac n{m + 1})$，单次计算的复杂度是两个东西乘起来，就是 $O(\frac{n\sqrt n}{m\sqrt m})$。

对于每个$m$ 一共有 $O(\sqrt{nm})$ 个 K 需要计算，因此每个 $m$ 复杂度是 $O(n^2/m)$，对所有 $m$ 求和，总复杂度为 $n^2\log n$。常数很小，可以通过。

<h3 id="一个巧妙的-dp-状态设计">一个巧妙的 DP 状态设计</h3>

<a href="https://www.fzoi.top/problem/7696" target="_blank" rel="noopener nofollow">序列 sequence</a>

只关心最大值，考虑设出最大值的位置。

设 $f_{i,j,0/1/2}$ 表示最大值在 $i-0/1/2$，值为 $j$，<strong>最大值位置+1 到 $i$</strong> 还没有填的答案。

这样的好处是，知道 最大值位置+1 到 $i$ 这一部分的值的填写方案其实和两头的最大值填写啥同时有关，所以放在填写下一个最大值的时候再决策。

转移使用前缀和优化。

<h3 id="线段树上同时二分">线段树上<strong>同时</strong>二分</h3>

[atcoder-pakencamp_2022_day2_h] Habatu

线段树上二分可以写一个递归函数进行。

但是如果需要同时二分 答案左端点 和 答案右端点，虽然也可以使用 <a href="https://atcoder.jp/contests/pakencamp-2022-day2/submissions/43064859" target="_blank" rel="noopener nofollow">递归</a>，但是这样是不优雅的。

更优雅的方法是 <a href="https://atcoder.jp/contests/pakencamp-2022-day2/submissions/40752943" target="_blank" rel="noopener nofollow">jiangly</a>

初始化线段树 $[0, 2^k)$，线段树上一个节点代表 $[l, r)$，因而 <code>build(l, mid, ls), build(mid, r, rs)</code>，因而 <code>if(l + 1 == r) return ;</code>

然后，二分的时候，我们只需要保证区间长度一直是 2 的次幂，那么就能够在线段树上唯一对应找到一个节点，从而获取信息。如何保证呢？

比如，如果当前二分到 $[l, r)$，那么区间 $[r - lowbit(r), r)$ 和 $[l, l + lowbit(l))$ 都是看起来不错的选择，这样搞，复杂度显然也是 log。

还有一个担心是 $r - lowbit(r) &lt;= l$，那么这个时候令 $r = r - lg(r - l - 1)$ 即可。

<h3 id="合并次数-on">合并次数 O(n)</h3>

很典啊，感觉最近（2023.8.9）突然见到了很多这样的题。

只需要保证每次检查两个集合是否可以合并的时候，要么可以合并，一旦不可以合并立刻停止就可以保证复杂度。

合并集合常常采用启发式合并。

CF1648E Air Reform

CF1801E Gasoline prices

Gym104337D Darkness II

<h3 id="找和某一个区间相交的区间">找和某一个区间相交的区间</h3>

Gym104337D Darkness II

找到和 $[xl, xr]$ 有交的矩形。

将前面已经处理好的矩形在 x 轴上的区间拆成 $log$ 个插入线段树。

考虑线段树上维护两个 set, <code>cur[rt]</code> 存储所有插入的时候 <code>xl &lt;= l &amp;&amp; l &lt;= xr</code> 的区间；<code>sub[rt]</code> 存储不满足 cur 的条件的区间，即在经过的线段树上的所有节点都 <code>insert</code>。

实际上，cur 维护了所有包含 $[l, r]$ 的矩形，sub 维护了所有被 $[l,r]$ 包含的。

新加入一个区间，设拆到线段树上节点 $u_1,u_2,\dots,u_x$，途径线段树节点 $v_1,v_2,\dots v_y$（ $v$ 包含 $u$），发现所求就是所有 $sub[u]$ 和 $cur[v]$

<blockquote>

容易证明这样一定可以检查到所有有交的区间，只需要证明对于每一个 u，都检查到了它的所有有交区间就好

和 u 有交，对于包含 u 的部分，已经被 $cur[v]$ 检查完了；对于被 $u$ 包含的部分，$sub[u]$ 一定可以检查完；对于剩下的部分，由于 $u$ 是线段树上的一个节点，所以一定会还是 $sub[u]$ 的一部分。

</blockquote>

<h3 id="欧拉回路">欧拉回路</h3>

欧拉回路可以吊打很多奇奇怪怪的题目

CF1458D Flip and Reverse：神仙建模转化题。考虑用 -1 代替 0，设 si 表示前缀和，si 向 s{i+1} 连边。在这个意义下k考虑操作 [l,r]，首先要求 s[l - 1] = s[r]，那么此时操作的是一个环，操作等价于将环上的边反向。观察到操作前后，边集 E 是保持不变的，因为一旦我翻转了有向边 x-&gt;y，由于操作的是一个环，我必然翻转 y-&gt;x，因此我们可以把这些边都当作有向边。此时，有结论，新的无向图的任意一个欧拉回路，和原字符串经过操作可以得到的字符串可以建立双射。具体的证明可以考虑欧拉回路的路径和原字符串的第一个分界点，利用翻转操作，容易使得两条路径重合，即消去这个分歧点，归纳即可。

[IOI2016] railroad

<a href="https://www.luogu.com.cn/problem/P6628" target="_blank" rel="noopener nofollow">[省选联考 2020 B 卷] 丁香之路</a>

<a href="https://www.fzoi.top/problem/7818" target="_blank" rel="noopener nofollow">网格图</a> 假如每一个点都是偶数点，那么win了。否则考虑补一下。<br>
记得还有一到类似的 AT，但是忘记了。

<h3 id="错解不优">错解不优</h3>

放宽限制使得题目可做，保证多算的部分（错解）不会计入答案，正解一定被算入答案。

感觉挺常见的，一直没有收录。（2023.10.2）

<a href="https://www.fzoi.top/problem/6934" target="_blank" rel="noopener nofollow">THUSCH2017 巧克力</a>

<a href="https://www.fzoi.top/problem/7945" target="_blank" rel="noopener nofollow">[QOJ 1189] [petrozavodsk winter 2020 Day 2 C] Wall Painting</a>

Forged in the Barrens

Loj3366 [IOI2020] 嘉年华奖券

<h4 id="对取-minmax-的方式进行枚举">对取 min/max 的方式进行枚举</h4>

需要计算 $\sum \min\{constant, f(i)\}$ 之类

对 a 执行 $n$ 次操作 $a\leftarrow max(0,a+x_i)$，可以看成是走路径问题，每次要么选择 0，要么选择 +x，然后求最大值。

那么这里实际上就变成了求最大后缀和。

当然有更复杂的一点的情况：<a href="https://www.fzoi.top/problem/7250" target="_blank" rel="noopener nofollow">20230803 T2 中等</a> fzoi 内置题解+总结里面写了。

<a href="https://www.fzoi.top/contest/231/problem/7475" target="_blank" rel="noopener nofollow">E Bowcraft</a>

<h3 id="-里面有减号考虑取一个补变成加号">$max$ 里面有减号，考虑取一个补变成加号</h3>

ARC076D Exhausted?

[Gym102354B] Yet Another Convolution

<h3 id="一类二元求和最优化问题">一类二元求和最优化问题</h3>

[ABC257Ex] Dice Sum 2

<a href="https://www.luogu.com.cn/remoteJudgeRedirect/atcoder/wtf19_d" target="_blank" rel="noopener nofollow">AT_wtf19_d Distinct Boxes</a>

将所有解拍在坐标轴上，发现只有凸包上的点有用。

为了拿到凸包上的点，考虑用某斜率的直线切这个凸包，找到这个斜率对应的意义，以某种方式获取这个切点。

不能 枚举/二分 实数斜率，可以考虑两个斜率可以被区分当且仅当切到不同的点，或者考虑凸包上的点并不多，所以可以随便点一个大数，然后令 $p+q=S$，二分 $p$，得到 $p/q$

<h3 id="若干变量乘积转化为多项式">若干变量乘积转化为多项式</h3>

$\prod c \to [x^0]\prod (x+c)$

[2018 集训队互测 Day 1]完美的集合

庫的F序计数

<h3 id="转化为-01-序列">转化为 01 序列</h3>

<h4 id="排序考虑对-01-序列进行排序">排序考虑对 01 序列进行排序</h4>

<a href="https://www.jianshu.com/p/2b06f4b0302e" target="_blank" rel="noopener nofollow">01 原理</a>

上面讲述了如何判定一个排序网络可以通过 $n!$ 种情况的检验只需要判定 $2^n$ 个 01 序列即可。

进一步的，如何判定一个排序网络是否可以通过某一个确定的排列呢？按某 solution 的说法：

<blockquote>

考虑对于一个排列 $a$ 和一个数 $x$ ，令 $01$ 序列 $b$，其中 $b_i = [a_i \ge x]$。<br>

显然 $a$ 可以被成功排序当且仅当对于所有 $x$，对应的 $b$ 都可以被成功排序。

</blockquote>

我们尝试进行一个证明：逆否命题是：存在一个 $x$ 使得 $b$ 不能排序，那么 $a$ 不能成功排序。好像确实很显然。

（以前似乎做过一道 AT 题用到了这个，但是想不起来了）

<a href="https://www.luogu.com.cn/problem/P2824" target="_blank" rel="noopener nofollow">TJOI2015 排序</a> 二分答案，对 01 序列反复排序是简单的

<a href="https://www.fzoi.top/problem/8268" target="_blank" rel="noopener nofollow">20240122 T3</a>

<h4 id="转化为-01-序列进行计数">转化为 01 序列进行计数</h4>

Gym104373H Permutation on Tree

对于题目求 $\sum |p_i-p_{i-1}|$，这个绝对值很烦，考虑枚举一个 $pivot$，计算 $\sum [(p_i&gt;=pivot)\neq (p_{i-1}&gt;=pivot)]$

感觉这个就和求 $\sum i\times f_i$ 变成求 $\sum_i \sum_{j\le i}f_j$ 一样，非常厉害。

<h3 id="树上求链交">树上求链交</h3>

一个神奇的结论

求 $(u,v)$ 和 $(x,y)$ 的交，方法是找出 $lca(u,x),lca(u,y),lca(v,x),lca(v,y)$ 中深度前两大的点 $p_1,p_2$

当且仅当 $p1==p2$ 且 $dep[p1]&lt;\max\{dep[lca(u,v)],dep[lca(x,y)]\}$ 时，交集为空，否则链交就是 $(p_1,p_2)$

<a href="https://codeforces.com/contest/500/problem/G" target="_blank" rel="noopener nofollow">New Year Running</a>

<h3 id="是否存在排列">是否存在排列</h3>

$\forall i, (\sum_j [lim_j\le i])\le i$

CSPS2023T4

<a href="https://www.fzoi.top/contest/288/problem/7526" target="_blank" rel="noopener nofollow">排列</a>

<h3 id="应对这种计算排列方案数的问题一种非常有价值的角度是视作把一个一个数插入进已有的排列而不是提前-reserve-好位置">应对这种计算排列方案数的问题，一种非常有价值的角度是视作把一个一个数插入进已有的排列，而不是提前 reserve 好位置。</h3>

<a href="https://www.fzoi.top/problem/7778" target="_blank" rel="noopener nofollow">排列之美</a>

<a href="https://www.luogu.com.cn/problem/CF1886D" target="_blank" rel="noopener nofollow">CF1886D Monocarp and the Set</a>

<h3 id="经典的对于这种一次从-需要枚举--而导致复杂度上升的-dp考虑一步步走">经典的，对于这种一次从 $i\to r$，需要枚举 $r$ 而导致复杂度上升的 DP，考虑一步步走。</h3>

T1 2023集训队互测R1T1 astral birth

NOIP2023 T3

[ARC118E] Avoid Permutations

<h3 id="lgv-引理">LGV 引理</h3>

典题是 路径交点 turtle 之类

同时，LGV 引理可以证明 Cathy-Binet 公式，进而证明矩阵树定理，关于 Cathy-Binet，有这样的一道例题：

[2020-2021 集训队作业]Yet Another Linear Algebra Problem

<h3 id="利用强制在线反解答案">利用强制在线反解答案。。。</h3>

[TYVJ 1927] 『Citric II』一道防AK好题：

<blockquote>

Lemon手上有一个长度为n的数列，第i个数为xi。他现在想知道，对于给定的a,b,c，他要找到一个i，使得$a*(i+1)*xi^2+(b+1)*i*xi+(c+i)=0$ 成立。如果有多个i满足，Lemon想要最小的那个i。Lemon有很多很多组询问需要你回答，多到他自己也不确定有多少组。所以在输入数据中a=b=c=0标志着Lemon的提问的结束。强制在线，真正要询问的a=a0+lastans,b=b0+lastans,c=c0+lastans.

</blockquote>

<a href="https://qoj.ac/problem/10" target="_blank" rel="noopener nofollow">卡常数</a> <a href="https://qoj.ac/download.php?type=solution&amp;id=10" target="_blank" rel="noopener nofollow">solution</a>

<h3 id="维护-lisk-的-fi-集合">维护 lis=k 的 fi 集合</h3>

一个 DS 手法，写在这里怕忘了。

<a href="https://www.fzoi.top/problem/7428" target="_blank" rel="noopener nofollow">数据结构题</a> <a href="https://www.fzoi.top/post/2453" target="_blank" rel="noopener nofollow">题解</a>

<a href="https://www.fzoi.top/problem/8261" target="_blank" rel="noopener nofollow">lis</a>

<h3 id="ad-hoc">ad-hoc</h3>

2023NOIPT3

<a href="https://www.fzoi.top/problem/8262" target="_blank" rel="noopener nofollow">20240109T3 cut</a> fzoi 内置题解。自己的理解放在“重设状态”部分。

AGC019F Yes or No

<h3 id="最小割和支配树">最小割和支配树</h3>

可以尝试思考一下这方面，这两者感觉还是存在一定的联系的。

割其实就是以 S 为根建立支配树，然后把 T 的一些啥啥玩意割了。

一个应用：<a href="https://codeforces.com/gym/103860/problem/H" target="_blank" rel="noopener nofollow">ICPC2021 Final H</a> 想要以 $u,v$ 为源点跑出两条增广路，等价于最小割 $\ge 2$，等价于 $u,v$ 在支配树上的 lca 是根（汇点 T）

<h3 id="完全平方数-和-线性基">完全平方数 和 线性基</h3>

注意“取值均匀“一节中涉及到的结论。

basic：「CF895C」 Square Subsets

advance：「THUSCH2017」杜老师

<h3 id="same-game">Same Game</h3>

[CSP-S 2023] 消消乐

<a href="https://www.fzoi.top/problem/8280" target="_blank" rel="noopener nofollow">消消乐</a>

<ol>
<li>将串首尾相接，不改变可消除性。这是因为，如果一个串 $s$ 可消除，那么将其末尾一位放到开头得到的 $s^{\prime}$ 也可消除。这是因为，显然可以将 $s$ 的消除过程中消除头尾的步骤放到最后。那么在 $s^{\prime}$ 中同样做 $s$ 的消除过程中除头尾部分的操作，然后剩下先消 $s_2^{\prime}$ 所在连续段即可。因此如果某个首尾相接的串可消除，那么取出其最后一步之前的任意一个剩余位置后的空隙断开即可。</li>
<li>将 $s$ 接成环后拿出相邻异或和 $t$ 也组成一个环，设其中 $1$ 的个数为 $c$（显然为偶数），最长连续 $1$ 段长 $m$，那么 $s$ 可消除当且仅当 $m\le\frac c2$。这是因为可以把单步操作视作将连续一段 $0$ 删去，然后将两侧两个 $1$ 并成一个 $0$。于是考虑所有极长连续 $1$ 段的长度 $l_1,\dots,l_k$ 形成环，那么每次操作相当于把相邻两个 $l$ 同时减去 $1$，减到 $0$ 就扔掉。这样结论就显然了</li>
</ol>

[PTZ summer 2023 Day 6 B] Bocchi the Rock 将 Y-R-P 赋值 -1, P-R-Y 赋值 1, P/Y-B-Y/P 赋值 0，发现合法的条件等价于选出的权值 $\sum=n$

<h3 id="拉格朗日数乘法">拉格朗日数乘法</h3>

数学知识，但是非常厉害。看了这几篇看懂了：

<a href="https://www.cnblogs.com/shine-lee/p/11715033.html" target="_blank">直观理解梯度，以及偏导数、方向导数和法向量等</a>

<a href="https://zhuanlan.zhihu.com/p/352106770" target="_blank" rel="noopener nofollow">https://zhuanlan.zhihu.com/p/352106770</a>

<a href="https://zhuanlan.zhihu.com/p/520475502" target="_blank" rel="noopener nofollow">https://zhuanlan.zhihu.com/p/520475502</a>

<a href="https://zhuanlan.zhihu.com/p/368334607" target="_blank" rel="noopener nofollow">https://zhuanlan.zhihu.com/p/368334607</a>

2023CCPC 哈尔滨 Energy Distribution

<h3 id="拉格朗日插多项式系数">拉格朗日插多项式系数</h3>

<a href="https://www.cnblogs.com/zzctommy/p/14603719.html" target="_blank">拉格朗日如何插出系数</a> 大意是设 $g(z)=\prod (z-x_i)$，然后递推求解 $h_i(z)=\frac{g(z)}{z-x_i}$

这意味着我们如果知道 $n$ 个点值，便可以通过拉格朗日插值法的构造，$O(n^2)$ 求解得到其对应的 $n-1$ 次多项式。

<a href="https://www.fzoi.top/problem/8304" target="_blank" rel="noopener nofollow">轻涟</a>

<h3 id="类欧几里得">类欧几里得</h3>

仅仅是复杂度计算方法类似。

对于一些计算和同余相关的问题，注意寻找子问题，思考是否可以递归做。

<a href="https://www.luogu.com.cn/problem/P5170" target="_blank" rel="noopener nofollow">标准类欧</a>

<a href="https://www.luogu.com.cn/problem/CF500G" target="_blank" rel="noopener nofollow">New Year Running</a> 类欧求解同余不等式最小解。

<a href="https://www.fzoi.top/problem/8310" target="_blank" rel="noopener nofollow">proton 质子纠缠</a> 类欧计算两点距离

<h3 id="集合-exp">集合 exp</h3>

FWT 之后，暴力 n^2 exp。

首先要注意理解 <a href="https://www.luogu.com.cn/blog/Fly37510/Exp-Meaning" target="_blank" rel="noopener nofollow">exp 的组合意义</a>，关键是注意使用 EGF，这样才能保证有标号。

通过 <a href="https://www.fzoi.top/problem/7486" target="_blank" rel="noopener nofollow">大度一点</a> 和 清华集训主旋律，可以得到一些关于 exp 等的理解。

条件容易转化为删成若干个边双，单点 $O(3^n)$ 的做法是容易的。

solution：

给出的图一定是 一些边双，里面某一些点挂了若干子树 + 森林，称前面部分方案数为 noforest, 后面部分方案数叫做 forest。不管怎么说，得先把 noforest 算出来吧。

forest 是可以算的，是 tree 的集合幂级数 exp（$O(n^22^n)$），tree 也是可以算的。

设 $e[S] = 2^{cnt[S]}$，其中 $cnt$ 表示 $S$ 的导出子图的边数，那么有 e 就是 noforest 和 forest 的无交并

只要做一个子集逆卷积就可以了，这也是 $O(n^22^n)$

剩下最后一步，需要把 nof 里面的 tree 砍掉，设 $g_{i,s}$ 表示 $\ge i$ 的点都在边双内的图的方案数，初始令 $g_{n, s} = nof_s$

需要排除掉 $i$ 有一个子树的情况，需要计算的 tree 使得 $i$ 是根，假装已经会做这个问题。


$$
g_{i, S} = g_{i+1,S} - \sum_{A \cup B = S, A\cap B = \varnothing, i = \max A} tree_A \times g_{i+1, B} \times edge_{i, B}

$$

仍然可以用子集卷积处理.

由于 max A 的约束，会多一个 n 的复杂度，这部分 $O(n^32^n)$

总复杂度 $O(n^32^n)$

<hr>

tree 也是可以 $O(n^22^n)$ 的，由于还有上述的要求，是每次新加入一个点 i 并给 i 选一点子树

```c++
for(int i = 0; i &lt; n; ++ i) {
    for(int s = 0; s &lt; (1 &lt;&lt; i); ++ s) 
        tmp[s] = mul(tree[s], cnt[msk[i] &amp; s]);
    exp(tmp, tree + (1 &lt;&lt; i), i);
}
```

注意到长度为 $2^i$ 的 exp 复杂度是 $i^22^i$，加起来也是 $O(i^22^i)$

<hr>

集合 exp 有两种实现方式，一种是老老实实 exp，另外一个是 std 给出的类似分治的做法。

```c++
void exp(int a[], int b[], int n) {
	b[0] = 1;
	rep(i, 0, n - 1) conv(a + (1 &lt;&lt; i), b, b + (1 &lt;&lt; i), i);
}
```

conv 表示对 $a,b$ 子集卷积并贡献到 c

<hr>

子集逆卷积有两种实现方式，一种是老老实实 逆卷积，另外一个是 std 给出的做法。

类似 清华集训 主旋律 的最后一步：


$$
\begin{align*}
nof_s &amp;= g_s - \sum_{b\subsetneq s} nof_b f_{s\backslash b}\\
&amp;= g_s - \sum_{b} g_b f_{s\backslash b} + \sum_{b}f_{s\backslash b}\sum_{a\subsetneq b} nof_a f_{b\backslash a}\\
&amp;= g_s - \sum_{b} g_b f_{s\backslash b} + \sum_{b}f_{s\backslash b}\sum_{a\subsetneq b} g_a - \sum_{b}f_{s\backslash b}\sum_{a}f_{b\backslash a}\sum_c nof_c f_{a\backslash c}\\
\end{align*}

$$

写到这里，就可以很清晰的发现，如果令 $f=-f$，那么 nof 就是将 forest 和 g 子集卷积的结果

注意如果直接认为 forest 是 tree 的 exp，按照 主旋律 的分析，知道实际上差了 -1 的容斥系数，所以这里已经取成负数了。（具体还是点进去看代码实现可以有更多体会）

<h3 id="杂">杂</h3>

给你一些数 a_n，让你选一些数去尽量逼近某一个数 x ，可以把每一个数看作是重量价值都是 a 的物品然后做 01 背包 <a href="https://www.fzoi.top/problem/5706" target="_blank" rel="noopener nofollow">POJ3211 Washing Clothes</a>

结论：对于一个 2n 阶的完全图，可以拆成 2n - 1 个完美匹配，有构造性证明：对于第 i 个匹配，将 i - j i + 1 + j 匹配在一起，把图画成一个圆的形式容易发现这样的正确性。(Least Annoying Constructive Problem)

tarjan 求 LCA，卡常的时候用吧。

<a href="https://www.luogu.com.cn/blog/AlexWei/leng-men-ke-ji-dfs-xu-qiu-lca" target="_blank" rel="noopener nofollow">dfs 序求 lca</a>

一个合法括号序列 $S$，必然可以被拆分成 $(A)B$ 的形式，其中 $A,B$ 也是合法括号序列。  Equilibrium Point

无限破环为链（复制）<a href="https://www.fzoi.top/problem/7874" target="_blank" rel="noopener nofollow">原石贸易</a>

分母固定时，去掉和式中的上/下去整：将 $a_i$ 拆成 $a_i/x$ 和 $a_i\bmod x$ 两部分分成两个独立的和式考虑。 <a href="https://www.fzoi.top/problem/7874" target="_blank" rel="noopener nofollow">原石贸易</a>

Hall 定理一个很有用的推论：最大匹配的大小为 $|X|-\max(|S|−R(|S|))$

删掉一组边集后图不连通当且仅当这些二进制数在异或运算下线性相关：开学前的涂鸦。

hd：计算机更喜欢的操作是枚举所有情况，不合法附加系数 0，合法系数就是 1。挺重要的化式子的思想。

BIT 的 $O(n)$ 重构：<code>c[i]=s[i]-s[i&amp;i-1]</code>

对于一些需要状压的题目，关键可能在于计算贡献的时刻放在哪里。 2023国庆FZCSP模拟赛三场联考 中的某一道题 以及 序列 sequence

二进制分组应对强制在线。

