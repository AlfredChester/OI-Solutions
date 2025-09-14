# 【8*】Slope Trick 学习笔记

### 前言

从 [一些快速笔记](https://www.luogu.com.cn/article/r3m57w52) 中分离出来的，因为这一知识点写的内容太多了，可以拿出来专门开一篇学习笔记。

Slope Trick 不是斜率优化！Slope Trick 不是斜率优化！Slope Trick 不是斜率优化！

**此类知识点大纲中并未涉及，所以【8】是我自己的估计，后带星号表示估计，仅供参考。**

### Slope Trick

Slope Trick 是一种优化 DP 的方法，核心思想是通过只存储DP转移的一些关键信息，从而利用数据结构高效维护转移。

Slope Trick 通常用于二维或高维 DP，把其中一维看作函数的自变量，其他维度都看作函数，转移就考虑两个函数之间的变化。一般使用 Slope Trick 优化的 DP 都满足如下性质：

$1$：是连续函数。

$2$：是分段一次函数或凸/凹函数，且斜率一般为整数。

在 Slope Trick 中我们一般只记录初始的斜率和斜率变化(一般为 $\pm1$)的位置(记作变化点)，放到数据结构中维护，如果一个位置的斜率变化多次则记录多次。

鉴于许多极为优秀的性质，Slope Trick 中有许多函数操作可以快速维护。这些操作快速维护的核心在于 Slope Trick 函数中会有一段水平的区间，这段区间往往是最大值或最小值。我们通常用两个堆分别维护这段水平区间左（包括水平段左端点）右（包括水平段右端点）的变化点。

$1$：相加。直接把初始斜率相加，并合并变化位置集合。

$2$：取前缀/后缀 max/min。以前缀 min 为例，把上升的位置，即水平的区间后面的变化点直接扔掉。

答案统计的时候有两种方法，一种是还原图像，另一种是记录决策点。

### 例题

例题 $1$：

[CF713C Sonya and Problem Wihtout a Legend](https://www.luogu.com.cn/problem/CF713C)

先把 $a_i$ 减 $i$ 转化为非严格递增。考虑写出朴素的 DP 式子。记 $f_{i,j}$ 表示 $a_i=j$ 时的最小花费，转移比较显然。

$$
f_{i,j}=\min_{k=1}^jf_{i-1,k}+\mid a_{i}-j\mid 
$$

记 $g_{j}$ 表示 $\min_{k=1}^jf_{i-1,k}$，注意到这是一个凸函数，而 $\mid a_{i}-j\mid$ 也是一个凸函数，故 $f_{i,j}$ 也为凸函数。且由于每次横坐标变化为 $1$，斜率是整数，符合 Slope Trick 的优化条件。考虑把改写后的式子写出来。

$$
f_{i,j}=g_j+\mid a_{i}-j\mid 
$$

于是转移就转化为了维护两个凸函数相加且取前缀最小值。我们不考虑初始斜率，加入 $\mid a_{i}-j\mid$ 等价于加入两个变化点 $\{a_{i},a_{i}\}$。我们按照 $a_{i}$ 和已有的最小点 $h$ 的位置分类讨论。

我们用一个堆维护 $g_j$ 的转移点。$h$ 即为堆顶的横坐标。

$1$：$a_i\ge h$，首先初始斜率加 $1$，加入的第一个 $a_i$ 把 $a_i$ 处的斜率变成了 $0$，加入的第二个 $a_i$ 取前缀最小值的时候删除了，所以只需要加入一个 $a_i$。

$2$：$a_i\lt h$，首先初始斜率加 $1$，加入了两个 $a_i$，$h$ 位置的斜率变化量为 $1$，在求前缀最小值的时候消掉了，因此我们先加入一个 $a_i$，弹出 $h$ 后再加入一个 $a_i$。此时新图象水平段向上平移了 $h-a_i$ 个单位长度，注意到最后到答案就是水平段的纵坐标，把 $h-a_i$ 累加到答案里即可。

注意到每次的决策点就是最小值的点，直接累加转移即可。初始斜率好像不需要记录。

```cpp
#include <bits/stdc++.h>
using namespace std;
long long n,a[4000],ans=0;
priority_queue<long long>q;
int main()
{
	scanf("%lld",&n);
	for(int i=1;i<=n;i++)
	    {
	    	scanf("%lld",&a[i]);
			a[i]-=i,q.push(a[i]);
			long long h=q.top();
	    	if(a[i]<h)ans+=h-a[i],q.push(a[i]),q.pop();
		}
	printf("%lld\n",ans);
	return 0;
}
```

例题 $2$：

[AT\_abc217\_h \[ABC217H\] Snuketoon](https://www.luogu.com.cn/problem/AT_abc217_h)

设 $f_{i,j}$ 表示时间 $i$ 结束后在位置 $j$ 的最小伤害，其余状态均不可优化。考虑先写出转移式子。

$$
f_{i,j}=\min\{f_{i-1,j-1},f_{i-1,j},f_{i-1,j+1}\}+\text{cost}(i,j) 
$$

把 $f_{i,j}$ 看成关于 $j$ 的函数 $f_i(j)$，不难证明函数 $f_i(j)$ 是凸的。考虑归纳法，$\text{cost}(i,j)$ 是凸的，显然 $f_1(j)$ 是凸的。假设 $f_n(j)$ 是凸的，则取 $\min$ 后 $f_n(j)$ 还是凸的，再加上一个凸函数 $\text{cost}(i,j)$，于是 $f_{n+1}(j)$ 为凸函数。所以 $f_i(j)$ 是凸的，且不难发现斜率为整数，所以可以使用 Slope Trick。

考虑前一步的取 $\min$，不难发现其实就是最小值想左右扩展，水平的那一段左边的往左平移一位，水平的那一段右边的往右平移一位，对于左右两个堆维护一个整体偏移量即可。

考虑 $D_i=0$，$\text{cost}(i,j)$ 为一个关于 $j$ 的函数，此函数初始斜率为 $-1$，存在一个变化点 $\{X_i\}$ 使斜率为 $0$。考虑添加变化点，如果这个变化点在水平段以及水平段左边，那么直接插入左堆，水平段纵坐标不变化。否则由于斜率加了 $1$，画图发现右堆水平段右端点处变成了左堆水平段左端点，弹出来右堆堆顶后插入左堆，并把新变化点插入右堆，顺便记录水平段纵坐标变化。

$D_i=1$ 也是同理。注意偏移量处理容易弄错。

注意初始状态 $f_0(j)$ 只有 $f_0(0)=0$，其余均为正无穷。我们可以把初始纵坐标设为 $0$，然后插入足够多个 $\{0\}$ 变化点，就可以实现这个效果。

```cpp
#include <bits/stdc++.h>
using namespace std;
long long n,t,d,x,lst=0,d1=0,d2=0,ans=0;
priority_queue<long long>l,r;
int main()
{
	scanf("%lld",&n);
	for(int i=1;i<=n;i++)l.push(0),r.push(0);
	for(int i=1;i<=n;i++)
	    {
	    	scanf("%lld%lld%lld",&t,&d,&x);
	    	d1-=(t-lst),d2+=(t-lst),lst=t;
	    	if(d==0)
	    	   {
	    	   	if(x<=-r.top()+d2)l.push(x-d1);
	    	   	else ans+=(x-(-r.top()+d2)),l.push(-r.top()+d2-d1),r.pop(),r.push(-(x-d2));
			   }
			else if(d==1)
			   {
			   	if(x>=l.top()+d1)r.push(-(x-d2));
	    	   	else ans+=(l.top()+d1-x),r.push(-(l.top()+d1-d2)),l.pop(),l.push(x-d1);
			   }
		}
	printf("%lld\n",ans);
	return 0;
}
```

例题 $3$：

[P3642 \[APIO2016\] 烟火表演](https://www.luogu.com.cn/problem/P3642)

记 $f_{x,i}$ 表示使点 $x$ 子树内与所有叶子节点距离均为 $i$ 的最小代价。我们不难列出如下方程。

$$
f_{x,i}=\sum_{v\in\text{son}(x)}\min_{j\le i}f_{v,j}+\mid e_d+j-i\mid 
$$

$f_{x,i}$ 是一个关于 $i$ 的凸函数，因为一定存在一个最佳长度使原 DP 值最小。无论偏小还是偏大，由于绝对值的影响，都只会增大而不会减小。于是我们定性地分析出了$f_{x,i}$ 的性质。

考虑令 $g_{v,i}=\min_{j\le i}f_{v,j}+\mid e_d+j-i\mid$，我们考虑先求出 $g_{v,i}$，然后通过合并求出 $f_{x,i}$。对于 $g_{v,i}$，设 $f_{v,j}$ 的左边最小值为 $l$，右边最小值为 $r$，我们进行分类讨论。

$1$：若 $i\lt l$，则 $f_{v,j}$ 至少以 $1$ 的斜率减小，而 $\mid e_d+j-i\mid$ 至多以 $1$ 的斜率增加，因此整个函数一定单调不增，取 $j=i$ 一定最小。此时 $g_{v,i}=f_{v,i}+e_d$。

$2$：若 $l\le i\lt l+e_d$，$\mid e_d+j-i\mid$ 的零点至多在 $l$ 处，同上得取 $l$ 是 $l$ 前一段的最小值。注意到斜率已经为 $0$，后面一定不优。此时 $g_{v,i}=f_{v,l}+e_d+l-i$。

$3$：若 $l+e_d\le i\lt r+e_d$，$\mid e_d+j-i\mid$ 的零点 $i-e_d$ 一定在区间 $[l,r)$，$f_{v,j}$ 取到最小值，两个函数都取到了最小值。此时 $g_{v,i}=f_{v,i-e_d}$。注意到此时 $i-e_d$ 一定在斜率为 $0$ 的那一段上，所以也有 $g_{v,i}=f_{v,l}$。

$4$：若 $i\gt r+e_d$，则函数 $\mid e_d+j-i\mid$ 的零点一定大于 $r$，且相加后在 $r$ 处斜率取到 $0$，于是取 $j=r$ 值最小。此时 $g_{v,i}=f_{v,r}+i-r-e_d$。注意到 $f_{v,l}=f_{v,r}$，所以 $g_{v,i}=f_{v,l}+i-r-e_d$。

注意到变化后还是一个连续的函数，在交界处都完美汇合，于是考虑观察 $g_{v,i}$ 和 $f_{v,j}$ 的变化。注意到第一种情况只是增加了初始值，没有改变斜率，不管它。第二、三、四种情况都只用到了 $f_{v,l}$，因此我们可以把 $l$ 以及之后的点都扔掉，也就是以这个点开头斜率 $\ge0$ 的点都扔掉。同时第二、三、四种情况斜率分别为 $-1,0,1$，我们加入 $\{l+e_d,r+e_d\}$，就完成了变换。

在这个题中，我们有办法偷懒不维护每个点的斜率。注意到点 $x$ 每有一个，$r$ 以及其右边的点数量就会加 $1$。因此，若儿子数量为 $k$，我们弹出前 $k-1$ 大的决策点就找到了 $l,r$，也找到了以这个点开头斜率 $\ge0$ 的点。

合并两个函数集合，直接左偏树就行了。初始状态直接 $\{0\}$ 即可，因为是和在下一层统计等效的，能省去一些特判。

最后统计答案可以通过 $f_{1,0}=\sum e_d$ 还原函数得到。

```cpp
#include <bits/stdc++.h>
using namespace std;
long long n,m,x,y,rt[400000],v[4000000],lc[4000000],rc[4000000],dist[4000000],cnt=0,ans=0;
vector<long long>s[400000],w[400000];
long long merge(long long x,long long y)
{
	if(!x||!y)return x+y;
    if(v[x]<v[y])swap(x,y);
    rc[x]=merge(rc[x],y);
    if(dist[rc[x]]>dist[lc[x]])swap(lc[x],rc[x]);
    dist[x]=dist[rc[x]]+1;
    return x;
}

void del(long long x)
{
	long long p=rt[x];
	rt[x]=merge(lc[rt[x]],rc[rt[x]]);
	lc[p]=rc[p]=dist[p]=0;
}

void insert(long long x,long long k)
{
	v[++cnt]=k;
	rt[x]=merge(rt[x],cnt);
}

void dfs(long long x)
{
	if(s[x].size()==0)insert(x,0);
	for(int i=0;i<(int)s[x].size();i++)
	    {
	    	dfs(s[x][i]);
	    	for(int j=0;j<(int)s[s[x][i]].size()-1;j++)del(s[x][i]);
	    	long long l=0,r=v[rt[s[x][i]]];
	    	del(s[x][i]),l=v[rt[s[x][i]]],del(s[x][i]),insert(s[x][i],l+w[x][i]),insert(s[x][i],r+w[x][i]);
	    	rt[x]=merge(rt[x],rt[s[x][i]]);
		}
}

int main()
{
	scanf("%lld%lld",&n,&m);
	dist[0]=-1,n+=m;
	for(int i=2;i<=n;i++)scanf("%lld%lld",&x,&y),s[x].push_back(i),w[x].push_back(y),ans+=y;
	dfs(1);
	for(int j=0;j<(int)s[1].size();j++)del(1);
	while(rt[1])ans-=v[rt[1]],del(1);
	printf("%lld\n",ans);
	return 0;
}
```

### 后记

[学习资料1](https://codeforces.com/blog/entry/77298) [学习资料2](https://www.cnblogs.com/cccomfy/p/17743031.html) [学习资料3](https://www.cnblogs.com/henrici3106/p/17093215.html#slope-trick-%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0)

感觉 Slope Trick 还是要多画图，瞪着想一上午不如随手画一下图。还是缺乏想象力啊。

> 天上数日幻作尘世百年 一瞬清风擦面
> 
> 何故撩拨我心弦 凌霄宝殿怎及
> 
> 阑珊处烟火点点