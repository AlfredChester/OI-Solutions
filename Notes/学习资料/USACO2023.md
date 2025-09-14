# USACO2023


<h3 id="dec1-breakdown">DEC1 Breakdown</h3>

将过程逆序，即加边并维护以下信息——

$f_{k,i,j}$表示从$i$到$j$恰走$k$步的最短路（其中$k\in [0,2]$）
$fs_{k,i}$表示从$1$到$i$恰走$k$步的最短路（其中$k\in [0,4]$）
$ft_{k,i}$表示从$i$到$n$恰走$k$步的最短路（其中$k\in [0,4]$）


任取$p,q\in [0,4]$且$p+q=K$，答案即$\min_{i\in [1,n]}fs_{p,i}+ft_{q,i}$

当加入边$(x,y)$时，影响的$f_{k,i,j}$必然有$i=x\or j=y$

任取$p,q\in [0,2]$且$p+q=k$，则$fs_{k,i}=\min_{j\in [1,n]}f_{p,1,j}+f_{q,j,i}$

结合前者，仅需更新$1=x\or j=y\or j=x\or i=y$的情况，共计$O(n^{3})$种

$ft_{k,i}$是类似的，时间复杂度为$O(Kn^{3})$

<hr>

<h3 id="dec2-making-friends">DEC2 Making Friends</h3>

记初始边集为$E$，维护$S_{i}$为$i$向$i<j$的边，初始$S_{i}=\{j\mid i<j\and (i,j)\in E\}$

对于第$i$天，即修改$\forall j\in S_{i},S_{j}=S_{j}\cup (S_{i}\cap (j,n])$

事实上，仅需取$x=\min_{j\in S_{i}}j$执行上述修改，其余修改会在$i=x$时实现

启发式合并即可，时间复杂度为$O(n\log^{2}n)$

<hr>

<h3 id="dec3-palindromes">DEC3 Palindromes</h3>

设区间$[l,r]$有$k$个G和$s$个H，第$i$个G前有$a_{i}$个H

设操作后第$i$个G前有$b_{i}$个H，则答案即$\min_{\begin{array}{}b_{1}\le b_{2}\le ...\le b_{k}\\b_{i}+b_{k-i+1}=s\end{array}}\sum_{i=1}^{k}|a_{i}-b_{i}|$

单调性可以在最小化中保证，同时$|a_{i}-b_{i}|+|a_{k-i+1}-b_{k-i+1}|\ge |a_{i}+a_{k-i+1}-s|$且总可以取到

特别的，当$2\not\mid k$时且$i=\frac{k+1}{2}$时，两项相同，需要一定特判

综上，则答案即$\begin{cases}\sum_{i=1}^{\frac{k}{2}}|a_{i}+a_{k-i+1}-s|&2\mid k\\-1&2\not\mid k\and 2\not\mid s\\\sum_{i=1}^{\frac{k-1}{2}}|a_{i}+a_{k-i+1}-s|+|a_{\frac{k+1}{2}}-\frac{s}{2}|&2\not\mid k\and 2\mid s\end{cases}$

枚举这$k$个G的中心并向两边拓展，记$c_{i}$为前$i$个位置中H的个数，则问题即：

<ul>
<li>若$(x,y)$为关于中心对称的两个G位置，则在可重集$S$中加入$c_{x}+c_{y}$</li>
<li>枚举所包含G恰为当前情况的区间$[l,r]$，求$\sum_{x\in S}|c_{l-1}+c_{r}-x|$</li>
</ul>

注意到$c_{l-1}+c_{r}$的变化量为$O(1)$，维护一个指针，每次询问时移动即可

同时，每个区间恰被询问一次，时间复杂度为$O(n^{2})$

<hr>

<h3 id="jan1-tractor-paths">JAN1 Tractor Paths</h3>

显然与$i$相邻的$j$构成区间$[L_{i},R_{i}]$，且$L_{i},R_{i}$均单调不降

对于询问，不妨假设$a<b$，那么最短路$d$即每次贪心从$a$移动到$R_{a}$直至$R_{a}\ge b$的次数

记$A_{i}$和$B_{i}$分别从$a$或$b$出发贪心时第$i$次移动到的位置，则答案也即$\sum_{i=0}^{d}|S\cap [B_{d-i},A_{i}]|$

显然区间非空，差分拆开后倍增时求和即可，时间复杂度为$O(n\log n+q\log n)$

<hr>

<h3 id="jan2-mana-collection">JAN2 Mana Collection</h3>

记$d(x,y)$为$x$到$y$的最短路，显然仅关心于每个点最后一次被访问的时间

设时间顺序为$a_{1},a_{2},...,a_{k}(=e)$​，即求


$$
\max_{\sum_{i=1}^{k-1}d(a_{i},a_{i+1})\le s}\sum_{i=1}^{k}m_{a_{i}}(s-\sum_{j=i}^{k-1}d(a_{j},a_{j+1}))=s\sum_{i=1}^{k}m_{a_{i}}-\sum_{i=1}^{k-1}d(a_{i},a_{i+1})\sum_{j=1}^{i}m_{a_{j}}
$$

关于$\sum_{i=1}^{k-1}d(a_{i},a_{i+1})\le s$的限制，若不满足，则去掉$a_{1}$后上式必然减小

去掉此限制后，记$f_{S,e}$为$\{a_{k}\}$构成集合$S$且$a_{k}=e$时$\sum_{i=1}^{k-1}d(a_{i},a_{i+1})\sum_{j=1}^{i}m_{a_{j}}$​的最小值，转移易得

在此基础上，答案即$\max_{e\in S}s\sum_{i\in S}m_{i}-f_{S,e}$，看作关于$s$的一次函数，对每个$e$求出其凸包即可

时间复杂度为$O(n^{2}2^{n}+q)$

<hr>

<h3 id="jan3-subtree-activation">JAN3 Subtree Activation</h3>

记$f(x,y)=\begin{cases}|sz_{x}-sz_{y}|&x和y成祖先后代关系\\sz_{x}+sz_{y}&otherwise\end{cases}$

问题即选择排列$\{a_{n}\}$，并最小化$sz_{a_{1}}+sz_{a_{n}}+\sum_{i=1}^{n-1}f(a_{i},a_{i+1})$

考虑确定$k$每个儿子子树内的排列后将$k$插入，有以下两种情况：

<ul>
<li>插入到某个儿子的子树内，设相邻两点为$x,y$，则变化量为$2sz_{k}-sz_{x}-sz_{y}-f(x,y)$
<ul>
<li>若$x$和$y$成祖先后代关系，该式也即$2(sz_{k}-\max(sz_{x},sz_{y}))$，取$x$或$y$为$k$的该儿子即可</li>
<li>否则，该式也即$2(sz_{k}-sz_{x}-sz_{y})$，显然劣于前者</li>
</ul>
</li>
<li>插入到两个儿子的子树间，即$sz_{a_{1}}$和$sz_{a_{n}}$的系数发生变化</li>
</ul>

由于对称性，定义$f_{k,x}$表示以$k$为根的子树中有$x\in [0,2]$个系数发生变化

转移可以自行分类讨论（注意一个子树自身不能匹配），时间复杂度为$O(n)$

<hr>

<h3 id="feb1-hungry-cow">FEB1 Hungry Cow</h3>

维护一棵权值线段树，对于每个区间，存储以下信息：

仅考虑区间内的食物和时间下，有食物的天数、天下标和 和 剩余的食物数

在此基础上，考虑查询一个区间内初始有$x$食物的答案——

<ul>
<li>若$x\le $左区间长度$-$左区间有食物的天数，则仅影响左区间</li>
<li>否则，即将左区间填满，并以$x-...+$左区间剩余的食物数询问右区间</li>
</ul>

合并时，即对右区间以左区间剩余的食物数查询，单次复杂度为$O(\log V)$

时间复杂度为$O(n\log^{2}V)$（其中$V$为值域）

<hr>

<h3 id="feb2-problem-setting">FEB2 Problem Setting</h3>

定义$g_{n}$表示$n$道题目任意顺序选非空子集的方案数，则$g_{n}=n(g_{n-1}+1)$

记$cnt_{S}$为Hard集合为$S$的题目数，定义$f_{S}$表示强制以$S$为结尾的方案数，则$f_{S}=g_{cnt_{S}}\sum_{T\subset S}f_{T}$

对此cdq分治，每次转移时用FWT求高维前缀和，时间复杂度为$O(nm+m2^{m})$

<hr>

<h3 id="feb3-watching-cowflix">FEB3 Watching Cowflix</h3>

若两个被选择的点间距离$\le k$，则可以贪心选择路径上所有点

在此基础上，每个连通块距离$\le \lfloor\frac{k}{2}\rfloor$的点（及自身）不交，即连通块数$\le \frac{n}{\lfloor\frac{k}{2}\rfloor+1}\le \frac{2n}{k}$

对此建立虚树并树形dp即可，时间复杂度为$O(n\log n)$

<hr>

<h3 id="open1-pareidolia">OPEN1 Pareidolia</h3>

关于$B(s)$，显然可以贪心匹配，状态为$bessie$的个数及匹配到的位置

考虑线段树，对于区间$[l,r]$，维护以下信息：

<ul>
<li>当之前已匹配$bessie$前$i$个字符时，$bessie$的个数 和 最终剩余的字符数</li>
<li>当之前已匹配$bessie$前$i$个字符时，每个前缀$bessie$的个数和</li>
<li>每个后缀$bessie$的个数和 和 最终剩余前$i$个字符的后缀数</li>
<li>区间内的答案$A(s[l,r])$</li>
</ul>

合并易得，时间复杂度为$O(n+m\log n)$

<hr>

<h3 id="open2-good-bitstrings">OPEN2 Good Bitstrings</h3>

问题即求满足以下条件的$(x,y)$个数：

$x\in [1,a],y\in [1,b]$
$\forall i\in [0,x],j\in [0,y],(i,j)\ne (x,y)$，满足$[\frac{i}{j}\le \frac{x}{y}]=[\frac{i}{j}\le \frac{a}{b}]$


考虑$\frac{x}{y}$在<a href="https://oi-wiki.org/math/number-theory/stern-brocot/" target="_blank" rel="noopener nofollow">Stern-Brocot树</a>中$\frac{X}{Y}$的子树内，并分类讨论：

<ul>
<li>若$X>a\or Y>b$，显然无解</li>
<li>若$\frac{X}{Y}\le \frac{a}{b}$，则$\frac{x}{y}$在$\frac{X}{Y}$的左子树内时取$\frac{i}{j}=\frac{X}{Y}$即矛盾，因此仅需考虑$\frac{X}{Y}$及其右子树内</li>
<li>若$\frac{X}{Y}>\frac{a}{b}$，同理仅需考虑$\frac{X}{Y}$及其左子树内</li>
</ul>

最终，$\frac{x}{y}$的取值即为一条链，并分类讨论：

<ul>
<li>
对于其中$\frac{X}{Y}>\frac{a}{b}$，则$\frac{x}{y}=\frac{tX}{tY}$时取$\frac{i}{j}=\frac{X}{Y}$即矛盾，仅能取$t=1$
</li>
<li>
对于其中$\frac{X}{Y}\le \frac{a}{b}$，设其构成序列$\{\frac{X_{k}}{Y_{k}}\}$，则$X_{i},Y_{i}$单调不降且$\frac{X_{i}}{Y_{i}}$单调递增
换言之，取$\frac{x}{y}=\frac{tX_{i}}{tY_{i}}$时仅需保证$x<X_{i+1}\or y<Y_{i+1}$，答案也即$\sum_{i=1}^{k-1}\max\{\lfloor\frac{X_{i+1}-1}{X_{i}}\rfloor,\lfloor\frac{Y_{i+1}-1}{Y_{i}}\rfloor\}+\frac{a}{X_{k}}$
</li>
</ul>

记值域为$V$，暴力递归最坏时间复杂度为$O(V)$，但"拐点"至多$O(\log V)$个，每次二分确定该段长度即可

前者直接加上长度即可，后者注意到$\lfloor\frac{x_{0}+(t+1)x_{1}-1}{x_{0}+tx_{1}}\rfloor$在$t\ge 1$时为$1$，仅需特殊处理$t=0$时

时间复杂度为$O(\log^{2}V)$

<hr>

<h3 id="open3-triples-of-cows">OPEN3 Triples of Cows</h3>

构造一张新图，形式如下：

<ul>
<li>共$2n-1$个点，对应原图中的点和边，并称前者为黑点、后者为白点</li>
<li>黑点向以其为端点的白点连边，并以$n$为根建树</li>
</ul>

第$i$天时，将$i$删除并将相邻的白点合并，则$(x,y)$存在$\iff $存在白点$j$与$x,y$相邻

（正确性归纳易得）

记$s_{i}$为（白点）$i$的儿子个数，并分类讨论：

<ul>
<li>$a,c$与$b$相邻的公共白点相同，答案即$\sum_{i为白点}(s_{i}+1)s_{i}(s_{i}-1)$</li>
<li>……不同且均为$b$的后代，答案即$\sum_{i为黑点}(\sum_{son}s_{son})^{2}-\sum_{son}s_{son}^{2}$</li>
<li>……不同且分别为$b$的祖先/兄弟和后代，答案即$\sum_{i为白点}2\cdot s_{i}\sum_{j}\sum_{son}s_{son}$</li>
</ul>

用并查集维护$i$的父亲，并维护相关信息即可，时间复杂度为$O(n\alpha(n))$

