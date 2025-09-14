# 【6】LCA学习笔记

### 前言

WFLS 2023 暑假集训 Day 2 Day 3

做完一部分老师的 LCA 题单后，我觉得人都要升华了。教练说前几年正是考这个玩意的时候，所以出的题目弯弯绕绕。我的评价：不如字符串。

### LCA

LCA 的**定义**与**性质**

**最近公共祖先**(LCA)：树上任意两点 $i,j$ 最近的公共祖先，记作 $lca(i,j)$。

LCA 的一些小性质：

$1$：记点 $i$ 到点 $j$ 的路径为 $dist(i,j)$，则 $dist(i,j)$ 可以拆分为 $dist(i,lca(i,j))$ 和 $dist(lca(i,j),j)$。

推论 $1$：令 $dis(i,j)$ 为 $i$ 到 $j$ 的路径权值之和，$d[i]$ 为点 $i$ 到根节点的权值之和，则有 $dis(i,j)=d[i]+d[j]-2\times d[lca(i,j)]$。

推论 $2$：点 $d$ 在路径 $i,j$ 上的条件为 $d$ 是 $i$ 或 $j$ 的父节点，且 $lca(i,j)$ 是 $d$ 的父节点。

推论 $3$：如果路径 $i,j$ 和路径 $k,l$ 相交，则路径 $i,j$ 经过 $lca(k,l)$ 或路径 $k,l$ 经过 $lca(i,j)$。

$2$：$lca(a_1,a_2\dots a_n)=lca(lca(a_1,a_2\dots a_{n-1}),a_n)=\dots=lca(lca\dots(lca(a_1,a_2)\dots),a_n)$

由于在树中两个点的路径中必定存在这两个点的 LCA，而这两个点的 LCA 又可以把路径分为两个部分，分别维护，所以一般涉及两点之间的路径的题目，一般都需要用到 LCA。

LCA 的**求法**

**预处理**

采用倍增思想，记 $fa[x][i]$ 为点 $x$ 向上（规定根节点方向为上）跳 $2^i$ 个节点到达的节点，则有：

$$
fa[x][i]=fa[fa[x][i-1]][i-1] 
$$

原因很显然，根据定义，$fa[x][i-1]$ 表示点 $x$ 向上（规定根节点方向为上）跳 $2^{i-1}$ 个节点到达的节点，此时在向上跳 $2^{i-1}$ 个节点，就相当于从 $x$ 开始向上跳了 $2^{i-1}+2^{i-1}=2^i$ 个节点，也就是 $f[x][i]$。

在实际实现的过程中，可以使用 DFS 递归求解，保证在求解某个点时，其所有的父节点已经求过了。每次转移时直接求出 $fa[x][0]$，然后根据上述式子求出其他值。在 DFS 的过程中，可以顺便求出每个点的深度，方便查询时使用。

```cpp
void dfs(int now)
{
	for(int i=1;i<30;i++)
	    if(fa[now][i-1])fa[now][i]=fa[fa[now][i-1]][i-1];
	    else break;
	for(int i=h[now];i;i=e[i].next)
	    if(e[i].v!=fa[now][0])
	    {
	    	fa[e[i].v][0]=now;
	    	dep[e[i].v]=dep[now]+1;
	    	dfs(e[i].v);
		}
}
```

时间复杂度：$O(n\log n)$

**回答询问**

我们首先要使询问的两个点深度相同，这样才能后期计算时一起向上跳。我们可以先按深度排序两个点，求出层数差，然后对这个差值二进制拆分（使用位运算），使层数深的点能每次向上跳 $2$ 的整数次幂个节点，最终拼出差值，统一深度。

注意，深度统一之后需要特判一下两个点是否已经重合，否则会影响后面的计算。

深度统一之后，我们可以让两个点每次尽量跳多一点。如果两个点重合，那么不能跳，因为这有可能不是最近的公共祖先。所以，我们从大到小枚举跳到步数，不相等就两个点一起跳。无法跳之后，其中任意一个点的父节点就是 LCA。

```cpp
int lca(int x,int y)
{
	if(dep[x]>dep[y])swap(x,y);
	int c=dep[y]-dep[x];
	for(int i=0;i<30;i++)
	    if((1<<i)&c)y=fa[y][i];
	if(x==y)return x;
	for(int i=29;i>=0;i--)
	    if(fa[x][i]!=fa[y][i])x=fa[x][i],y=fa[y][i];
	return fa[x][0];
}
```

时间复杂度：$O(\log n)$

我们可以发现，求 LCA 的过程 [【6】ST表学习笔记](https://www.luogu.com.cn/blog/w9095/st-biao-xue-xi-bi-ji) 有些相似。综合这两篇笔记，我们可以总结出倍增类题目的一般规律：先**倍增预处理**，然后**拆分回答询问**。

另外，这个求 $fa$ 数组的模板，也可以用来维护一些其他的东西，我们将在例题里面见到。

### 树上差分

---

给定一棵有 $n$ 个节点的树，每个点有权值。先有 $m$ 次修改操作，每次形如 $i,j,k$，表示将路径 $i,j$ 上每个点的权值增加 $k$。然后有 $q$ 次询问操作，每次形如 $i,j$，表示路径 $i,j$ 上每个点的权值之和。

$n\le10^5,m\le10^5,q\le10^5,1\le i,j\le n,k\le10^9$

---

对于这种**静态修改操作**，我们可以采用**树上差分**。对于每一次修改操作，都对差分数组进行修改，最后一次性上传标记。

具体来说，记 $q_x$ 为点 $x$ 的差分权值，$f_x$ 为点 $x$ 的父节点，对于每一个 $i,j,k$ 的修改操作，在树中进行如下修改：

$$
q_i+k,q_j+k,q_{lca(i,j)}-k,q_{f_{lca(i,j)}}-k 
$$

所有操作结束后，通过 DFS 一次性上传每个节点的差分权值。记 $s_{x_{1,2\dots n}}$ 为点 $x$ 的子节点，则对于每一个节点，上传每个节点的差分权值时有：

$$
q_x+\sum_{i=1}^{n}s_{x_{i}} 
$$

由于 $lca$ 可以把路径分为两个部分，所以如果 $q_i+k,q_j+k$，那么增加的 $k$ 最终一定会上传累加到 $lca(i,j)$，且上传的过程中一定经过了路径中的每一个点，使每一个点权值加 $k$。此时，$q_{lca(i,j)}$ 由于 $i,j$ 两个子节点累加，会变为 $q_{lca(i,j)}+2k$，需要减去一个 $k$，所以需要 $q_{lca(i,j)}-k$。由于 $lca(i,j)$ 之上的部分不再属于这条路径，所以需要彻底排除 $+k$ 的影响。由于 $lca(i,j)$ 处已经减去了一个 $k$，在 $f_{lca(i,j)}$ 处再减去一个 $k$ 即可，所以需要 $q_{f_{lca(i,j)}}-k$。

```cpp
void query(int now)
{
	for(int i=h[now];i;i=e[i].next)
	    if(e[i].v!=fa[now][0])
		    {
		    	query(e[i].v);
		    	q[now]+=q[e[i].v];
			}
}

void add(int i,int j,int k)
{
    q[i]+=k,q[j]+=k,q[lca(i,j)]-=k,q[fa[lca(i,j)][0]]-=k;
}
```

时间复杂度：$O(n\log n)$

### 例题

例题 $1$ ：

[P3379 【模板】最近公共祖先（LCA）](https://www.luogu.com.cn/problem/P3379)

LCA 模板题，不多赘述。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[1200000];
int n,m,s,x,y,h[600000],fa[600000][30],dep[600000],cnt=0;
void add_edge(int u,int v)
{
	e[++cnt].next=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void dfs(int now)
{
	for(int i=1;i<30;i++)
	    if(fa[now][i-1])fa[now][i]=fa[fa[now][i-1]][i-1];
	    else break;
	for(int i=h[now];i;i=e[i].next)
	    if(e[i].v!=fa[now][0])
	    {
	    	fa[e[i].v][0]=now;
	    	dep[e[i].v]=dep[now]+1;
	    	dfs(e[i].v);
		}
}

int lca(int x,int y)
{
	if(dep[x]>dep[y])swap(x,y);
	int c=dep[y]-dep[x];
	for(int i=0;i<30;i++)
	    if((1<<i)&c)y=fa[y][i];
	if(x==y)return x;
	for(int i=29;i>=0;i--)
	    if(fa[x][i]!=fa[y][i])x=fa[x][i],y=fa[y][i];
	return fa[x][0];
}

int main()
{
	scanf("%d%d%d",&n,&m,&s);
	for(int i=1;i<=n-1;i++)
	    {
	    	scanf("%d%d",&x,&y);
	    	add_edge(x,y);add_edge(y,x);
		}
	dfs(s);
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%d%d",&x,&y);
	    	printf("%d\n",lca(x,y));
		}
	return 0;
}
```

例题 $2$ ：

[P3258 \[JLOI2014\] 松鼠的新家](https://www.luogu.com.cn/problem/P3258)

每走过一个房间，就需要在房间内放一颗糖。实际上就是将某条路径内的点权值 $+1$，可以直接使用树上差分，最后一次性上传标记。

注意，记 $f_x$ 为点 $x$ 的父节点每次修改的路径应为 $a_{i-1},f_{a_i}$。因为如果修改 $a_{i-1},a_i$，则在下次 $a_i,a_{i+1}$ 的修改中，$a_i$ 被重复计算了一次。同时，题目中也说了，参观指南上最后一个房间在最后一次参观中不需要将权值 $+1$，刚好符合这个修改方式。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[1200000];
int n,x,y,a[600000],h[600000],q[600000],fa[600000][30],dep[600000],cnt=0;
void add_edge(int u,int v)
{
	e[++cnt].next=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void dfs(int now)
{
	for(int i=1;i<30;i++)
	    if(fa[now][i-1])fa[now][i]=fa[fa[now][i-1]][i-1];
	    else break;
	for(int i=h[now];i;i=e[i].next)
	    if(e[i].v!=fa[now][0])
	    {
	    	fa[e[i].v][0]=now;
	    	dep[e[i].v]=dep[now]+1;
	    	dfs(e[i].v);
		}
}

int lca(int x,int y)
{
	if(dep[x]>dep[y])swap(x,y);
	int c=dep[y]-dep[x];
	for(int i=0;i<30;i++)
	    if((1<<i)&c)y=fa[y][i];
	if(x==y)return x;
	for(int i=29;i>=0;i--)
	    if(fa[x][i]!=fa[y][i])x=fa[x][i],y=fa[y][i];
	return fa[x][0];
}

void query(int now)
{
	for(int i=h[now];i;i=e[i].next)
	    if(e[i].v!=fa[now][0])
		    {
		    	query(e[i].v);
		    	q[now]+=q[e[i].v];
			}
}

int main()
{
	scanf("%d",&n);
	for(int i=1;i<=n;i++)scanf("%d",&a[i]);
	for(int i=1;i<=n-1;i++)
	    {
	    	scanf("%d%d",&x,&y);
	    	add_edge(x,y);add_edge(y,x);
		}
	dfs(1);
	for(int i=2;i<=n;i++)
	    {
	    	int f=lca(a[i],a[i-1]);
	    	q[fa[f][0]]--,q[f]--,q[fa[a[i]][0]]++,q[a[i-1]]++;
		}
	query(1);
	for(int i=1;i<=n;i++)
	    printf("%d\n",q[i]);
	return 0;
}
```

例题 $3$ ：

[P3398 仓鼠找 sugar](https://www.luogu.com.cn/problem/P3398)

直接套用 LCA 的性质 $1$ 推论 $3$，如果路径 $i,j$ 和路径 $k,l$ 相交，则路径 $i,j$ 经过 $lca(k,l)$ 或路径 $k,l$ 经过 $lca(i,j)$。

下面给出详细解释。

由 LCA 的定义以及求法，得知 $lca(i,j)$ 是路径 $i,j$ 中每一个点祖先。由于这是一棵树，所以如果想到达某个点，那么必须先到达其祖先。如果另一条路径与这条路径交于任意一点，则另一条路径一定经过 $lca(i,j)$。所以，只需要判断另一条路径与 $lca(i,j)$ 的关系就可以了。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[1200000];
int n,m,x,y,a,b,h[600000],fa[600000][30],dep[600000],cnt=0;
void add_edge(int u,int v)
{
	e[++cnt].next=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void dfs(int now)
{
	for(int i=1;i<30;i++)
	    if(fa[now][i-1])fa[now][i]=fa[fa[now][i-1]][i-1];
	    else break;
	for(int i=h[now];i;i=e[i].next)
	    if(e[i].v!=fa[now][0])
	    {
	    	fa[e[i].v][0]=now;
	    	dep[e[i].v]=dep[now]+1;
	    	dfs(e[i].v);
		}
}

int lca(int x,int y)
{
	if(dep[x]>dep[y])swap(x,y);
	int c=dep[y]-dep[x];
	for(int i=0;i<30;i++)
	    if((1<<i)&c)y=fa[y][i];
	if(x==y)return x;
	for(int i=29;i>=0;i--)
	    if(fa[x][i]!=fa[y][i])x=fa[x][i],y=fa[y][i];
	return fa[x][0];
}

bool judge(int x,int y)
{
	if(dep[x]>dep[y])return 0;
	int c=dep[y]-dep[x];
	for(int i=0;i<30;i++)
	    if((1<<i)&c)y=fa[y][i];
	if(x==y)return 1;
    else return 0;
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n-1;i++)
	    {
	    	scanf("%d%d",&x,&y);
	    	add_edge(x,y);add_edge(y,x);
		}
	dfs(1);
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%d%d%d%d",&x,&y,&a,&b);
	    	int f1=lca(x,y),f2=lca(a,b);
            if((judge(f2,f1)&&(judge(f1,a)||judge(f1,b)))||(judge(f1,f2)&&(judge(f2,x)||judge(f2,y))))printf("Y\n");
            else printf("N\n");
		}
	return 0;
}
```

例题 $4$ ：

[P1967 \[NOIP2013 提高组\] 货车运输](https://www.luogu.com.cn/problem/P1967)

需要将图转化为树。

首先，有一个显而易见的贪心：尽量走较长的边。这样就能保证最小值最大，满足要求。

进一步想，如果点 $i,j$ 能通过多条较长边联通，那么一条 $i,j$ 直达的最短边是无用的，可以在图中删去。如果一直删除这样无用的边，就会发现最后剩下的边刚好构成一棵树。因为每两个点都只会有一条最长的路径联通，自然就是一棵树。所以，我们可以直接在图中求出最大生成树，在树上解决这个问题。

由于把货物从 $x$ 运往 $y$ 是一条树上路径，自然想到用 LCA 来处理，把路径分为两个部分。每次询问的回答是路径上的边权的最小值，所以我们需要额外维护一个记录数组 $mi$，仿照 $fa$ 数组，我们采用倍增思想，记 $mi[x][i]$ 为点 $x$ 向上（规定根节点方向为上）跳 $2^i$ 个节点路径上的边权的最小值，则易得：

$$
mi[x][i]=\min(mi[x][i-1],mi[fa[x][i-1][i-1]) 
$$

这个可以在求 $fa$ 的时候顺便求出来，根据这个数组，在查询时求出跳过的每一步的最小值就行了。

注意，由于图不一定联通，所以在转化为树之后要每个连通块预处理一次。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next,dis;
}e[120000];
struct value
{
	int u,v,dis;
}cun[120000];
int n,m,x,y,z,q,f[60000],h[60000],fa[60000][30],mi[60000][30],dep[60000],cnt=0;
void add_edge(int u,int v,int dis)
{
	e[++cnt].next=h[u];
	e[cnt].v=v;
	e[cnt].dis=dis;
	h[u]=cnt;
}

bool cmp(struct value a,struct value b)
{
	return a.dis>b.dis;
}

int getf(int x)
{
	if(f[x]==x)return x;
	else return f[x]=getf(f[x]);
}

void merge(int x,int y)
{
	int p=getf(x),q=getf(y);
	if(p!=q)f[q]=p;
}

bool judge(int x,int y)
{
	return getf(x)==getf(y);
}

void dfs(int now)
{
	for(int i=1;i<30;i++)
	    if(fa[now][i-1])fa[now][i]=fa[fa[now][i-1]][i-1],mi[now][i]=min(mi[now][i-1],mi[fa[now][i-1]][i-1]);
	    else break;
	for(int i=h[now];i;i=e[i].next)
	    if(e[i].v!=fa[now][0])
	    {
	    	fa[e[i].v][0]=now;
	    	mi[e[i].v][0]=e[i].dis;
	    	dep[e[i].v]=dep[now]+1;
	    	dfs(e[i].v);
		}
}

int lca(int x,int y)
{
	int ans=99999999;
	if(dep[x]>dep[y])swap(x,y);
	int c=dep[y]-dep[x];
	for(int i=0;i<30;i++)
	    if((1<<i)&c)ans=min(ans,mi[y][i]),y=fa[y][i];
	if(x==y)return ans;
	for(int i=29;i>=0;i--)
	    if(fa[x][i]!=fa[y][i])
	       {
	       ans=min(ans,mi[x][i]),ans=min(ans,mi[y][i]);
		   x=fa[x][i],y=fa[y][i];
	       }
	return min(ans,min(mi[x][0],mi[y][0]));
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++)
	    for(int j=0;j<29;j++)
	        mi[i][j]=99999999;
	for(int i=1;i<=n;i++)f[i]=i;
	for(int i=1;i<=m;i++)scanf("%d%d%d",&cun[i].u,&cun[i].v,&cun[i].dis);
	sort(cun+1,cun+m+1,cmp);
	for(int i=1;i<=m;i++)
	    if(!judge(cun[i].u,cun[i].v))
		    {
		    	merge(cun[i].u,cun[i].v);
		    	add_edge(cun[i].u,cun[i].v,cun[i].dis);
				add_edge(cun[i].v,cun[i].u,cun[i].dis);
			}
	for(int i=1;i<=n;i++)	
	    if(getf(i)==i)dfs(i);
	scanf("%d",&q);
	for(int i=1;i<=q;i++)
	    {
	    	scanf("%d%d",&x,&y);
	    	if(!judge(x,y))printf("-1\n");
	    	else printf("%d\n",lca(x,y));
		}
	return 0;
}
```

例题 $5$ ：

[P2597 \[ZJOI2012\] 灾难](https://www.luogu.com.cn/problem/P2597)

需要将图转化为树。

本题需要使用一个新东西：**支配树**。我们定义，如果点 $i$ 的灭绝会直接或间接地导致点 $j$ 的灭绝，则称点 $i$ **支配**点 $j$，并在支配树上建立一条 $i\to j$ 的有向边。问题就转化为了支配树上的每个点有多少个子节点。

我们发现，一个点一定能被其所有父节点在支配树上的 LCA 支配。原因显然，根据支配树的定义，这个点的所有父节点都能被这个 LCA 节点支配。如果这个 LCA 节点灭绝，就会导致这个点的所有父节点灭绝，从而导致这个点灭绝。

由于图不一定联通，所以我们需要建立一个超级源点，并以此作为支配树的树根。接下来，由于图中不存在环，所以这个图是一个 DAG，为了使计算每一个节点时其父节点已经计算过，我们可以对这个 DAG 进行拓扑排序。每次有一个新的点出队列时，就立即计算其 $fa[now][i]$，并且通过 LCA 的性质 $2$，求出其所有父节点在支配树上的 LCA。

最后，在支配树上搜索回溯就可以求出支配树上的每个点有多少个子节点了。

```cpp
#include <bits/stdc++.h>
using namespace std;
long long n,a,ind[100000],dep[100000],ans[100000],fa[100000][30];
vector<long long>s[100000],f[100000],zt[100000];
long long lca(long long x,long long y)
{
	if(dep[x]>dep[y])swap(x,y);
	long long c=dep[y]-dep[x];
	for(long long i=0;i<30;i++)
	    if((1<<i)&c)y=fa[y][i];
	if(x==y)return x;
	for(long long i=29;i>=0;i--)
	    if(fa[x][i]!=fa[y][i])x=fa[x][i],y=fa[y][i];
	return fa[x][0];
}

void topo_sort()
{
	long long h=1,t=0,q[100000];
	q[++t]=0;
	while(h<=t)
	   {
	   	long long now=q[h];
	   	long long l1=f[now].size(),l2=s[now].size(),z=0;
	   	if(l1>1)z=f[now][0];
	   	for(long long i=1;i<l1-1;i++)z=lca(z,f[now][i]);
	   	zt[z].push_back(now),fa[now][0]=z,dep[now]=dep[z]+1;;
	   	for(long long i=1;i<30;i++)
		    if(fa[now][i-1])fa[now][i]=fa[fa[now][i-1]][i-1];
		    else break;
	   	for(long long i=0;i<l2;i++)
	   	    {
		   	ind[s[now][i]]--;
			if(ind[s[now][i]]==0)q[++t]=s[now][i];
		    }
	   	h++;
	   }
}

long long count(long long now)
{
	long long cnt=1,l=zt[now].size();
	for(long long i=0;i<l;i++)
	    if(zt[now][i]!=0)cnt+=count(zt[now][i]);
	ans[now]=cnt;
	return cnt;
}

int main()
{
	scanf("%lld",&n);
	for(long long i=1;i<=n;i++)
	    {
	    a=-1; 
	    while(a!=0)
	       {
	       	scanf("%lld",&a);
	       	if(a==0)break;
	       	s[a].push_back(i),f[i].push_back(a),ind[i]++;
		   }
	    }
	for(long long i=1;i<=n;i++)
	    s[0].push_back(i),f[i].push_back(0),ind[i]++;
	topo_sort();
	count(0);
	for(long long i=1;i<=n;i++)
	    printf("%lld\n",ans[i]-1);
	return 0;
}

```

### 后记

树链剖分是个好东西，可惜我不会用。