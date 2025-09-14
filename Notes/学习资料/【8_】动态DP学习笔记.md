# 【8*】动态DP学习笔记

### 前言

WC 2024 的知识点，补个档。寒假时间紧促，这篇博客是边学边写的。

**此类知识点大纲中并未涉及，所以【8】是我自己的估计，后带星号表示估计，仅供参考。**

### 动态 DP

动态 DP 用于解决需要求出的值可以使用**树形 DP** 求出，并要求支持**单点修改**点权的操作。

---

给定一棵 $n$ 个点的树，点带点权。

有 $m$ 次操作，每次操作给定 $x,y$，表示修改点 $x$ 的权值为 $y$。

你需要在每次操作之后求出这棵树的最大权独立集的权值大小。

---

**最大权独立集**：在图上选择一些**不相邻**的点组成的集合，称为图的**独立集**。使选出的点权值之**和**最大的独立集，就是**最大权独立集**。

我们先考虑没有修改的情况。选取 $1$ 号点为整棵树的树根。设 $f_{i,0}$ 为**不选择** $i$ 号点时，以 $i$ 号点为根**子树**的**最大权独立集**，$f_{i,1}$ 为**选择** $i$ 号点时，以 $i$ 号点为根**子树**的**最大权独立集**。容易推出如下状态转移方程：

$$
f_{i,0}=\sum_{j\in son(i)}\max(f_{j,0},f_{j,1}) 
$$

$$
f_{i,1}=\sum_{j\in son(i)}f_{j,0}+a_i 
$$

对于叶子节点，$f_{i,0}=0,f_{i,1}=1$。最终的答案就是 $\max(f_{1,0},f_{1,1})$。

我们发现，当某一个点被修改时，只会影响到**这个点到根节点的路径**上的值。我们考虑用**树链剖分**维护这个东西。

由于树链剖分树的儿子有**轻儿子**和**重儿子**之分，我们需要再定义一个 $g$ 数组来辅助转移。设 $g_{i,1}$ 表示 $i$ 号点的**轻儿子**都**不取**的最大权独立集加上 $a_i$ 的值，也就是选择自己的带来的权值。$g_{i,0}$ 表示 $i$ 号点的**轻儿子**都**可取可不取**的最大权独立集。我们就可以简化上述式子：(式子中 $j$ 为 $i$ 的重儿子)

$$
f_{i,0}=g_{i,0}+\max(f_{j,0},f_{j,1}) 
$$

$$
f_{i,1}=g_{i,1}+f_{j,0} 
$$

考虑使用**矩阵**维护递推关系。我们定义**矩阵 `max` 运算**。对于一个 $p\times q$ 的矩阵 $A$ 和一个 $q\times r$ 的矩阵 $B$，它们的 `max` 运算结果为一个 $p\times r$ 的矩阵 $C$。在这个矩阵中，$c_{i,j}$ 满足如下式子：

$$
c_{i,j}=\max_{k=1}^q(c_{i,j},a_{i,k}+b_{k,j}) 
$$

和矩阵乘法类比：矩阵乘法中，两种运算满足加法交换律，加法结合律，乘法交换律，乘法结合律；矩阵 `max` 中，两种运算满足 `max` 交换律，`max` 结合律，加法交换律，加法结合律，加法分配律(对于 `max` 运算)。所以这个运算也是满足**结合律**的。

```cpp
struct matrix matrix_max(struct matrix a,struct matrix b)
{
	struct matrix c;
	for(int i=0;i<2;i++)
	    for(int j=0;j<2;j++)
	        c.v[i][j]=-2e9;
	for(int i=0;i<2;i++)
	    for(int j=0;j<2;j++)
	        for(int k=0;k<2;k++)
	            c.v[i][j]=max(c.v[i][j],a.v[i][k]+b.v[k][j]);
	return c;
}
```

先把这两个式子写成便于使用**矩阵 `max` 运算**维护的形式：

$$
f_{i,0}=\max(g_{i,0}+f_{j,0},g_{i,0}+f_{j,1}) 
$$

$$
f_{i,1}=\max(g_{i,1}+f_{j,0},-\infty) 
$$

于是，我们就可以把这个递推关系用**矩阵**的形式写出来：

$$
\begin{bmatrix}g_{i,0}&g_{i,0}\\g_{i,1}&-\infty\\\end{bmatrix}\begin{bmatrix}f_{j,0}\\f_{j,1}\end{bmatrix}=\begin{bmatrix}f_{i,0}\\f_{i,1}\end{bmatrix} 
$$

这里的转移矩阵必须写在前面，因为树链剖分中，链上的信息需要**线段树**来维护。线段树的合并是**从左到右**，由于矩阵运算不满足交换律，所以转移矩阵必须写在前面。

我们可以在树链剖分**第二次** DFS 的时候，顺便求出 $f$ 的值，并立即填充矩阵。注意 $g$ 中不计算重儿子的贡献，但是 $f$ 计算。

```cpp
void dfs2(long long x,long long pr,long long tf)
{
	id[x]=++dfc,top[x]=tf,ed[tf]=max(ed[tf],id[x]);
	f[x][1]=c[x];
	a[id[x]].v[1][0]=c[x],a[id[x]].v[1][1]=-2e9;
	if(hs[x]!=0)
	   {
	   dfs2(hs[x],x,tf);
	   f[x][0]+=max(f[hs[x]][0],f[hs[x]][1]);
	   f[x][1]+=f[hs[x]][0];
       }
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=pr&&e[i].v!=hs[x])
	       {
		   dfs2(e[i].v,x,e[i].v);
		   f[x][0]+=max(f[e[i].v][0],f[e[i].v][1]);
		   f[x][1]+=f[e[i].v][0];
		   a[id[x]].v[0][0]+=max(f[e[i].v][0],f[e[i].v][1]);
		   a[id[x]].v[1][0]+=f[e[i].v][0];
	       }
	a[id[x]].v[0][1]=a[id[x]].v[0][0];
}
```

由于我们知道这个东西是满足结合律的，所以我们用**线段树**来维护一段**重链**的矩阵 `max`。这样维护之后这棵树就有了几个性质：

$1$：每一段重链**链底**保存的是这个重链的**起始状态**。也就是说，假设链底为点 $x$，这个矩阵中必然存有 $f_{x,0}$ 和 $f_{x,1}$。但是我们需要手动模拟一下，才能发现这两个值存在哪里。比如，手动模拟一下这个转移，如果一个位置在模拟转移后**满足递推公式**，那么这个位置存的就是这个数。我们发现转移矩阵 $a$ 中 $a_{0,0}=f_{x,0},a_{1,0}=f_{x,1}$。

$2$：在转移的过程中，**轻儿子**的信息被存入了**矩阵**，转移时只用处理**重儿子**的信息即可。而重儿子的信息，可以通过**树链剖分**，用线段树来区间查询。

$3$：修改点 $x$ 之后，首先点 $x$ 的状态信息会**改变**。具体而言，由于 $g_{j,1}$ 包含了这个节点的**权值**，这个节点权值改变之后，这个值必然也要跟着改变。这个值存在矩阵第 $1$ 行第 $0$ 列中。然后，这条**重链**的信息也会改变。那么，根据树链剖分的性质，这条重链**链顶**必然作为某个节点的**轻儿子**。那么，这个重链链顶的父亲的轻儿子信息改变，它的**转移矩阵**自然也要改变。我们先计算修改前的重链贡献，**撤销贡献**，再把更新后的重链信息加上去即可。这样，又有一条重链的信息也会改变，递归继续修改，直到到达根节点。注意撤销操作中撤销与重新计算的是 $f$ 值，需要找到 $f$ 在答案矩阵中对应位置用于撤销或重新计算。**链尾**中 $f$ 对应的位置，就是**答案矩阵**中 $f$ 对应的位置。

```cpp
a[id[x]].v[1][0]=a[id[x]].v[1][0]-c[x]+y,c[x]=y;
rupdate(x,y);

void rupdate(long long x,long long y)
{
    while(x!=0)
      {
      	struct matrix pre=query(root,id[top[x]],ed[top[x]]);
      	update(root,id[x]);
      	struct matrix now=query(root,id[top[x]],ed[top[x]]);
      	x=fa[top[x]];
      	a[id[x]].v[0][0]=a[id[x]].v[0][0]-max(pre.v[0][0],pre.v[1][0])+max(now.v[0][0],now.v[1][0]);
      	a[id[x]].v[0][1]=a[id[x]].v[0][0];
      	a[id[x]].v[1][0]=a[id[x]].v[1][0]-pre.v[0][0]+now.v[0][0];
	  }
}
```

$4$：如果要求出一整条重链的答案矩阵，我们需要访问**一整条**重链，所以还需要维护一条重链的**链尾**。我们求出的答案是一个矩阵，**链尾**中 $f$ 对应的位置，就是**答案矩阵**中 $f$ 对应的位置。这点很重要，不然你都不知道答案在哪里。对于每一次查询整棵树的信息，就是查询树根 $1$ 所在的**重链**的值，直接求出并查询对应值即可。

```cpp
struct matrix ans=query(root,id[1],ed[1]);
printf("%lld\n",max(ans.v[0][0],ans.v[1][0]));
```

至此，我已经把我在学习动态 DP 中遇到的问题都讲解了一遍。调试比较困难，需要模块化调试。

动态 DP 的时间复杂度很好分析，就是**树链剖分**的时间复杂度 $O(n\log^2n)$。但是由于使用了矩阵，**常数巨大**，谨慎使用。

由于动态 DP 难写难调难推导，而且常数大，所以能不用就不用。

### 例题

例题 $1$：

[P4719 【模板】"动态 DP"&动态树分治](https://www.luogu.com.cn/problem/P4719)

动态 DP 模板题，不多赘述。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct matrix
{
	long long v[2][2];
}a[200000];
struct edge
{
	long long v,nxt;
}e[400000];
struct node
{
	struct matrix g;
	long long l,r;
}tr[800000];
long long n,m,x,y,c[200000],h[200000],f[200000][2],cnt=0,root=1;
long long lc[800000],rc[800000];
long long dep[200000],fa[200000],siz[200000],hs[200000],id[200000],top[200000],ed[200000],dfc=0;
void add_edge(long long u,long long v)
{
	e[++cnt].nxt=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

struct matrix matrix_max(struct matrix a,struct matrix b)
{
	struct matrix c;
	for(int i=0;i<2;i++)
	    for(int j=0;j<2;j++)
	        c.v[i][j]=-2e9;
	for(int i=0;i<2;i++)
	    for(int j=0;j<2;j++)
	        for(int k=0;k<2;k++)
	            c.v[i][j]=max(c.v[i][j],a.v[i][k]+b.v[k][j]);
	return c;
}

void pushup(long long x)
{
	tr[x].g=matrix_max(tr[lc[x]].g,tr[rc[x]].g);
}

void build(long long now,long long l,long long r)
{
	lc[now]=now*2,rc[now]=now*2+1;
	tr[now].l=l,tr[now].r=r;
	if(l==r)
	   {
	   	tr[now].g=a[l];
	   	return;
	   }
	long long mid=(l+r)>>1;
	build(lc[now],l,mid),build(rc[now],mid+1,r);
    pushup(now);
}

void update(long long now,long long k)
{
	if(tr[now].l==tr[now].r)
	   {
	   	tr[now].g=a[k];
	   	return;
	   }
	long long mid=(tr[now].l+tr[now].r)>>1;
	if(k<=mid)update(lc[now],k);
	else if(k>=mid+1)update(rc[now],k);
	pushup(now);
}

struct matrix query(long long now,long long l,long long r)
{
	if(tr[now].l>=l&&tr[now].r<=r)return tr[now].g;
	long long mid=(tr[now].l+tr[now].r)>>1;
	if(l>mid)return query(rc[now],l,r);
	else if(r<mid+1)return query(lc[now],l,r);
	else return matrix_max(query(lc[now],l,r),query(rc[now],l,r));
}

void dfs1(long long x,long long f)
{
	long long mx=0,ans=0;
	dep[x]=dep[f]+1,fa[x]=f,siz[x]=1;
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=f)
		    {
		    	dfs1(e[i].v,x);
		    	if(siz[e[i].v]>mx)mx=siz[e[i].v],ans=e[i].v;
		    	siz[x]+=siz[e[i].v];
			}
	hs[x]=ans;
}

void dfs2(long long x,long long pr,long long tf)
{
	id[x]=++dfc,top[x]=tf,ed[tf]=max(ed[tf],id[x]);
	f[x][1]=c[x];
	a[id[x]].v[1][0]=c[x],a[id[x]].v[1][1]=-2e9;
	if(hs[x]!=0)
	   {
	   dfs2(hs[x],x,tf);
	   f[x][0]+=max(f[hs[x]][0],f[hs[x]][1]);
	   f[x][1]+=f[hs[x]][0];
       }
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=pr&&e[i].v!=hs[x])
	       {
		   dfs2(e[i].v,x,e[i].v);
		   f[x][0]+=max(f[e[i].v][0],f[e[i].v][1]);
		   f[x][1]+=f[e[i].v][0];
		   a[id[x]].v[0][0]+=max(f[e[i].v][0],f[e[i].v][1]);
		   a[id[x]].v[1][0]+=f[e[i].v][0];
	       }
	a[id[x]].v[0][1]=a[id[x]].v[0][0];
}

void rupdate(long long x,long long y)
{
    while(x!=0)
      {
      	struct matrix pre=query(root,id[top[x]],ed[top[x]]);
      	update(root,id[x]);
      	struct matrix now=query(root,id[top[x]],ed[top[x]]);
      	x=fa[top[x]];
      	a[id[x]].v[0][0]=a[id[x]].v[0][0]-max(pre.v[0][0],pre.v[1][0])+max(now.v[0][0],now.v[1][0]);
      	a[id[x]].v[0][1]=a[id[x]].v[0][0];
      	a[id[x]].v[1][0]=a[id[x]].v[1][0]-pre.v[0][0]+now.v[0][0];
	  }
}

int main()
{
	scanf("%lld%lld",&n,&m);
	for(int i=1;i<=n;i++)scanf("%lld",&c[i]);
	for(int i=1;i<=n-1;i++)
	    {
	    	scanf("%lld%lld",&x,&y);
	    	add_edge(x,y),add_edge(y,x);
		}
	dfs1(1,0),dfs2(1,0,1);
	build(root,1,n);
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%lld%lld",&x,&y);
	    	a[id[x]].v[1][0]=a[id[x]].v[1][0]-c[x]+y,c[x]=y;
	    	rupdate(x,y);
	    	struct matrix ans=query(root,id[1],ed[1]);
	    	printf("%lld\n",max(ans.v[0][0],ans.v[1][0]));
		}
	return 0;
}
```

例题 $2$：

[P5024 \[NOIP2018 提高组\] 保卫王国](https://www.luogu.com.cn/problem/P5024)

无解的情况很好判断，同一条边上两个点都不能选，显然无解。其他情况均有解。

由于涉及树上 DP 和修改，想到动态 DP。

设 $f_{i,0}$ 表示不选 $i$ 以 $i$ 为根的子树内最小总费用，$f_{i,1}$ 表示选 $i$ 以 $i$ 为根的子树内最小总费用。

$$
f_{i,0}=\sum_{j\in son(i)}f_{i,1} 
$$

$$
f_{i,1}=\sum_{j\in son(i)}\min(f_{i,0},f_{i,1})+a_{i} 
$$

对于叶子节点，$f_{i,0}=0,f_{i,1}=a_i$。

设 $g_{i,1}$ 表示选择全部轻儿子的最小总费用，$g_{i,0}$ 表示可选可不选全部轻儿子的最小总费用加上选择自己的值，$j$ 表示重儿子。

$$
f_{i,0}=\min(g_{i,1}+f_{j,1},\infty) 
$$

$$
f_{i,1}=\min(g_{i,0}+f_{j,0},g_{i,0}+f_{j,1}) 
$$

同样的，我们定义矩阵 `min` 运算。对于一个 $p\times q$ 的矩阵 $A$ 和一个 $q\times r$ 的矩阵 $B$，它们的 `min` 运算结果为一个 $p\times r$ 的矩阵 $C$。在这个矩阵中，$c_{i,j}$ 满足如下式子：

$$
c_{i,j}=\min_{k=1}^q(c_{i,j},a_{i,k}+b_{k,j}) 
$$

这个东西显然满足结合律。

依据动态 DP 的套路，写成矩阵形式。

$$
\begin{bmatrix}\infty&g_{i,1}\\g_{i,0}&g_{i,0}\\\end{bmatrix}\begin{bmatrix}f_{j,0}\\f_{j,1}\end{bmatrix}=\begin{bmatrix}f_{i,0}\\f_{i,1}\end{bmatrix} 
$$

剩下的就是树链剖分加线段树维护即可。

对于必选点，我们把这个点的 $g_{i,1}$ 修改为正无穷，表示这个点不能不选。对于必不选点，我们把这个点的 $g_{i,0}$ 修改为正无穷，表示这个点不能选。需要修改在转移矩阵中对应位置的值。因为要修改转移矩阵，所以要修改整条重链，然后递归到根节点。

计算完答案之后，撤销影响，因为询问互相独立。

坑点很多，需要格外仔细。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct matrix
{
	long long v[2][2];
}a[200000];
struct edge
{
	long long v,nxt;
}e[400000];
struct node
{
	struct matrix g;
	long long l,r;
}tr[800000];
long long n,m,x,y,z,w,c[200000],h[200000],f[200000][2],cnt=0,root=1;
long long lc[800000],rc[800000];
long long dep[200000],fa[200000],siz[200000],hs[200000],id[200000],top[200000],ed[200000],dfc=0;
char typ[100];
void add_edge(long long u,long long v)
{
	e[++cnt].nxt=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

struct matrix matrix_min(struct matrix a,struct matrix b)
{
	struct matrix c;
	for(int i=0;i<2;i++)
	    for(int j=0;j<2;j++)
	        c.v[i][j]=1e16;
	for(int i=0;i<2;i++)
	    for(int j=0;j<2;j++)
	        for(int k=0;k<2;k++)
	            c.v[i][j]=min(c.v[i][j],a.v[i][k]+b.v[k][j]);
	return c;
}

void pushup(long long x)
{
	tr[x].g=matrix_min(tr[lc[x]].g,tr[rc[x]].g);
}

void build(long long now,long long l,long long r)
{
	lc[now]=now*2,rc[now]=now*2+1;
	tr[now].l=l,tr[now].r=r;
	if(l==r)
	   {
	   	tr[now].g=a[l];
	   	return;
	   }
	long long mid=(l+r)>>1;
	build(lc[now],l,mid),build(rc[now],mid+1,r);
    pushup(now);
}

void update(long long now,long long k)
{
	if(tr[now].l==tr[now].r)
	   {
	   	tr[now].g=a[k];
	   	return;
	   }
	long long mid=(tr[now].l+tr[now].r)>>1;
	if(k<=mid)update(lc[now],k);
	else if(k>=mid+1)update(rc[now],k);
	pushup(now);
}

struct matrix query(long long now,long long l,long long r)
{
	if(tr[now].l>=l&&tr[now].r<=r)return tr[now].g;
	long long mid=(tr[now].l+tr[now].r)>>1;
	if(l>mid)return query(rc[now],l,r);
	else if(r<mid+1)return query(lc[now],l,r);
	else return matrix_min(query(lc[now],l,r),query(rc[now],l,r));
}

void dfs1(long long x,long long f)
{
	long long mx=0,ans=0;
	dep[x]=dep[f]+1,fa[x]=f,siz[x]=1;
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=f)
		    {
		    	dfs1(e[i].v,x);
		    	if(siz[e[i].v]>mx)mx=siz[e[i].v],ans=e[i].v;
		    	siz[x]+=siz[e[i].v];
			}
	hs[x]=ans;
}

void dfs2(long long x,long long pr,long long tf)
{
	id[x]=++dfc,top[x]=tf,ed[tf]=max(ed[tf],id[x]);
	f[x][1]=c[x];
	a[id[x]].v[1][0]=c[x],a[id[x]].v[0][0]=1e16;
	if(hs[x]!=0)
	   {
	   dfs2(hs[x],x,tf);
	   f[x][0]+=f[hs[x]][1];
	   f[x][1]+=min(f[hs[x]][0],f[hs[x]][1]);
       }
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=pr&&e[i].v!=hs[x])
	       {
		   dfs2(e[i].v,x,e[i].v);
		   f[x][0]+=f[e[i].v][1];
		   f[x][1]+=min(f[e[i].v][0],f[e[i].v][1]);
		   a[id[x]].v[0][1]+=f[e[i].v][1];
		   a[id[x]].v[1][0]+=min(f[e[i].v][0],f[e[i].v][1]);
	       }
	a[id[x]].v[1][1]=a[id[x]].v[1][0];
}

void rupdate(long long x,long long y)
{
    while(x!=0)
      {
      	struct matrix pre=query(root,id[top[x]],ed[top[x]]);
      	update(root,id[x]);
      	struct matrix now=query(root,id[top[x]],ed[top[x]]);
      	x=fa[top[x]];
      	a[id[x]].v[0][1]=a[id[x]].v[0][1]-pre.v[1][1]+now.v[1][1];
      	a[id[x]].v[1][0]=a[id[x]].v[1][0]-min(pre.v[0][1],pre.v[1][1])+min(now.v[0][1],now.v[1][1]);
	    a[id[x]].v[1][1]=a[id[x]].v[1][0]; 
	  }
}

void change(long long x,long long p,long long y)
{
	a[id[x]].v[p^1][p]=y;
	a[id[x]].v[1][1]=a[id[x]].v[1][0];
	rupdate(x,y);
}

int main()
{
	scanf("%lld%lld%s",&n,&m,typ);
	for(int i=1;i<=n;i++)scanf("%lld",&c[i]);
	for(int i=1;i<=n-1;i++)
	    {
	    	scanf("%lld%lld",&x,&y);
	    	add_edge(x,y),add_edge(y,x);
		}
	dfs1(1,0),dfs2(1,0,1);
	build(root,1,n);
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%lld%lld%lld%lld",&x,&y,&z,&w);
	    	if(dep[x]<dep[z])swap(x,z),swap(y,w);
	    	if(y==0&&w==0)
	    	   {
	    	   	bool flag=0;
	    	   	for(int i=h[x];i;i=e[i].nxt)
	    	   	    if(e[i].v==z)flag=1;
	    	   	if(flag)
	    	   	    {
	    	   	   	printf("-1\n");
	    	   	   	continue;
					}
			   }
			long long px=a[id[x]].v[y^1][y],pz=a[id[z]].v[w^1][w];
			change(x,y,1e16);
			change(z,w,1e16);
	    	struct matrix ans=query(root,id[1],ed[1]);
	    	printf("%lld\n",min(ans.v[0][1],ans.v[1][1]));
	    	change(x,y,px);
			change(z,w,pz);
		}
	return 0;
}
```

### 后记

其实动态 DP 并不难，就是一个大缝合怪。但是网上讲解具体实现的资料很少，所以学起来很困难。话说回来，用到动态 DP 真的没几道题。

据说有办法通过 LCT 做到 $O(n\log n)$，显然我不会。

[动态 DP 好题解](https://www.luogu.com.cn/blog/Tweetuzki/solution-p4179)