# 【7】Tarjan学习笔记

### 前言

WFLS 暑假集训 Day 5 Day 6 Day 8 Day 9

[Tarjan](https://baike.baidu.com/item/tarjan/8970498?fr=ge_ala) 是个巨佬，快来膜拜他 orz。

**长文警告：本文一共 $1092$ 行，请合理安排阅读时间。**

### 强连通分量

强连通分量针对有向图，本篇目内图指有向图。

**定义**

**强连通**：如果一个图中任意两点可以相互到达，那么称这个图为强连通的。

**极大**：在满足条件的情况下包含点数**最多**的子图。

**强连通分量**：一个图中的**极大强连通子图**，叫做强连通分量。

换句话说，在强连通分量中，任意两点可以相互到达。只要到达其中任意一个点，其余的每一个点都可以到达。也就是说，在一个强连通分量里的点可以看作一个点，这就是后面会提到的，也就是 Tarjan 最主要的用处——**缩点**。

一个强连通分量（除一个点）中至少**包含**一个**环**。

**Tarjan 算法（核心）**

Tarjan 算法定义了两个数组：`dfn` 和 `low`。我们以深度优先搜索的方式遍历这个图，其中 `dfn[i]` 表示节点 $i$ 的**时间戳**，`low[i]` 表示点 $i$ 可以到达的**最小**的时间戳，也就是最小的 `dfn` 值。

首次访问到一个强连通分量里的某个点时，会发现这个点无法回溯到比其 `dfn` 值更小的节点。否则必然可以通过这个点到达其父节点，构成另一个强连通分量。而 `low[i]` 的值最小是 `dfn[i]`，因为一个点总是能回溯到自己。所以，当我们发现一个点的 `dfn` 和 `low` 值相等时，就意味着我们找到了一个强连通分量。

**Tarjan 算法（实现）**

由于缩点需要，仅仅求出有多少强连通分量是不够的，我们需要求出每个点属于哪个强连通分量，从而进行缩点。我们可以发现这样一个性质：首次访问到一个强连通分量里的某个点后，这个强连通分量里的每一个点，都是其在 DFS 搜索树上的**子节点**。我们可以用一个栈保存其子节点，发现满足条件直接将在这个后进栈，也就是其子节点全部出栈，并及时记录即可。

在整个实现过程中，我们采用 DFS 框架，通过 `dfn` 的值来反映这个点是否访问过。如果没有访问过，就递归计算这个子节点，回溯时注意更新 `low` 值，因为父节点可以通过这个子节点进行转移；如果访问过，那么直接更新 `low` 值即可。

```cpp
void tarjan(int now)
{
	dfn[now]=low[now]=++dfc,st[++top]=now,in[now]=1;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v])
	       {
	       	tarjan(e[i].v);
	       	low[now]=min(low[now],low[e[i].v]);
		   }
		else if(in[e[i].v])
		    low[now]=min(low[now],dfn[e[i].v]);
    if(dfn[now]==low[now])
       {
       sc++;
       while(st[top]!=now)scc[st[top]]=sc,in[st[top]]=0,top--;
	   scc[st[top]]=sc,in[st[top]]=0,top--;
       }
}
```

**缩点**

将每个点属于的强连通分量计算出来后，可以依据这个进行**缩点**。具体做法是，遍历每一条边，如果发现这条边连接的两个节点属于不同的强连通分量，那么证明这两个强连通分量有路径连接，在强连通分量编号中建边。

另外，缩点要求在一个强连通分量中的点**共享状态**。也就是其中之一发生变化，其余的每一个点都要发生同样的变化。

缩点之后的有向图有一个特点：**没有环**，是 **DAG**。因为如果有环，一定会是强连通分量，进而被缩成一个点。缩点之后，一些不能处理环的算法，例如拓扑排序，就可以正常使用了。

```cpp
void add_edge1(int u,int v)
{
	e[++cnt1].next=h[u];
	e[cnt1].v=v;
	h[u]=cnt1;
}

void add_edge2(int u,int v)
{
	s[++cnt2].next=d[u];
	s[cnt2].v=v;
	d[u]=cnt2;
}

for(int i=1;i<=n;i++)
	for(int j=h[i];j;j=e[j].next)
	   if(scc[i]!=scc[e[j].v])add_edge2(scc[i],scc[e[j].v]);
```

### 割边（点）

割边（点）针对无向图，本篇目内图指无向图。

**定义**(以下定义中图均为无向图)

**割点**：如果在一个连通图中，删去一个**点**，可以使整个图**不再连通**，那么这个点叫做**割点**。

**割边**：如果在一个连通图中，删去一条**边**，可以使整个图**不再连通**，那么这个点叫做**割边**。

有割点**一定**有割边，有割边**不一定**有割点。

例如这个图，就是有割边无有割点。

![](https://cdn.luogu.com.cn/upload/image_hosting/r7fjbvfc.png)

如果一个图中不存在**割点**，则任意两个点之间都至少有两条**点不重合**的路径；如果一个图中不存在**割边**，则任意两条边之间都至少有两条**边不重合**的路径。

**割点算法（核心）**

我们可以套用 Tarjan 算法的定义，在 Tarjan 算法的基础上设计割点/边算法。

如果存在一个点 $i$，满足这个点 DFS 树上的任一子节点的 `low` 值大于或等于 $dfn[i]$，那么这个点就是割点。因为如果这个点的子节点没有办法通过除 $i$ 以外的其他节点到达点 $i$ 之前的点，那么删去这个点，这个点的子节点和点 $i$ 之前的点就不能互相到达，图不再连通，符合割点的定义。

注意这个方法需要特判 DFS 树上的根节点，因为没有任何一个点能到达根节点之前的节点，但是根节点除至少有两个子节点才是割点。

**割点算法（实现）**

我们额外用一个变量记录当前节点 DFS 树上的儿子个数。遍历每一个儿子节点，如果出现任意一个子节点 `low` 值大于或等于 $dfn[i]$，且这个点不为根节点，直接记录为割点。如果是根节点，需要等记录儿子个数的变量大于 $1$ 时才能判定为割点。

```cpp
void tarjan(int now,int fa)
{
	int chi=0;
	dfn[now]=low[now]=++dfc,st[++top]=now,in[now]=1;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v])
	       {
	       	tarjan(e[i].v,now);
	       	low[now]=min(low[now],low[e[i].v]);
	       	if(low[e[i].v]>=dfn[now])
	       	   {
			   chi++;
	       	   if(now!=root||chi>1)cut[now]=1;
	           }
		   }
		else if(in[e[i].v]&&e[i].v!=fa)
		    low[now]=min(low[now],dfn[e[i].v]);
    if(dfn[now]==low[now])
       {
       while(st[top]!=now)in[st[top]]=0,top--;
	   in[st[top]]=0,top--;
       }
}
```

**割边算法（核心）**

总体和割边算法一致，但是判定割边的条件为出现任意一个子节点 `low` 值大于 $dfn[i]$，不能等于。因为如果等于，证明这个子节点可以越过这条边到达祖先，删去这条边对连通性造成影响。

**割边算法（实现）**

由于不用特判根节点，实现起来就比割点简单多了。不需要记录儿子数量，只要有一个子节点满足 `low` 值大于 $dfn[i]$，即可判定为割边。

```cpp
void tarjan(int now,int fa)
{
	dfn[now]=low[now]=++dfc,st[++top]=now,in[now]=1;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v]&&e[i].v!=fa)
	       {
	       	tarjan(e[i].v,now);
	       	low[now]=min(low[now],low[e[i].v]);
	       	if(low[e[i].v]>dfn[now])ans++;
		   }
		else if(in[e[i].v]&&e[i].v!=fa)
		    low[now]=min(low[now],dfn[e[i].v]);
    if(dfn[now]==low[now])
       {
       while(st[top]!=now)in[st[top]]=0,top--;
	   in[st[top]]=0,top--;
       }
}
```

### 双连通分量

双连通分量针对无向图，本篇目内图指无向图。

**定义**

**点双连通分量**：如果一个极大子图中不存在**割点**，那么这个子图为**点双连通分量**($v-DCC$)。

**边双连通分量**：如果一个极大子图中不存在**割边**，那么这个子图为**边双连通分量**($e-DCC$)。

像强连通分量在有向图中用于缩点一样，点（边）双连通分量也可以用于在无向图中**缩点**。缩点之后，图就是一棵**树**。

边双连通分量**一定**是点双连通分量，点双连通分量**不一定**是边双连通分量。

例如这个图，就是点双连通分量，但不是边双连通分量。

![](https://cdn.luogu.com.cn/upload/image_hosting/r7fjbvfc.png)

每个点只会属于一个边双连通分量，但可能属于多个点双连通分量。

**边双连通分量求法**

由于边双连通分量中不存在割边，所以只需要删去所有割边，剩下的图中就不存在割边，剩下的每个连通分量都是边双连通分量。

```cpp
void tarjan(int now,int fa)
{
	dfn[now]=low[now]=++dfc;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v])
	       {
	       	tarjan(e[i].v,i);
	       	low[now]=min(low[now],low[e[i].v]);
	       	if(low[e[i].v]>dfn[now])cut[i]=cut[i^1]=1;
		   }
		else if(i!=(fa^1))
		    low[now]=min(low[now],dfn[e[i].v]);
}

void dfs(int now)
{
	scc[now]=sc;
	scp[sc].push_back(now);
	for(int i=h[now];i;i=e[i].next)
	    {
	    if(scc[e[i].v]||cut[i])continue;
	    dfs(e[i].v);
	    }
}

for(int i=1;i<=n;i++)
   if(!scc[i])
     {
     sc++;
     dfs(i);
     }
```

**点双连通分量（核心）**

由于一个点可能会属于多个点双连通分量，所以上述求边双连通分量的方法不能使用了。

我们类比 Tarjan 求强连通分量的算法，用一个栈把每个点存起来。当我们发现一个点是割点时，不断弹栈直到上一次出栈的节点是造成这个点是割点的子节点，最后将这个点加入这个点双连通分量。因为如果一个点是割点，那么这个点会造成这个点到其子节点的下一个割点之间出现一个点双连通分量。按照递归的方式，其子节点的下一个割点之后出现的点已经弹完了，剩余的就是这个点双连通分量的节点了。

当然，这里判断上一次出栈的节点是否为造成这个点是割点的子节点，而不是栈顶节点是否为这个割点，是为了防止这个割点还有其他子树，由于存储结构是栈，这个割点的其他子树的节点也在这个割点之上，按照后者判定会把这些点也加入这个点双连通分量，这并不符合要求。

**点双连通分量（实现）**

参考 Tarjan 求割点的代码，在递归搜索完所有子节点后，每次判定为是割点时，不断弹栈直到上一次出栈的节点是造成这个点是割点的子节点。

注意，如果这样做，最后栈不为空时，需要把剩余的元素单独加入一个点双连通分量，因为这是根节点没有处理的第一个孩子。

```cpp
void tarjan(int now,int fa)
{
	int chi=0;
	dfn[now]=low[now]=++dfc,st[++top]=now;
	if(now==root&&h[now]==0)dcc[++dc].push_back(st[top]),top--;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v])
	       {
	       	tarjan(e[i].v,i);
	       	low[now]=min(low[now],low[e[i].v]);
	       	if(low[e[i].v]>=dfn[now])
	       	   {
			   chi++;
	       	   if(now!=root||chi>1)
	       	      {
	       	      	dc++;
                    while(st[top+1]!=e[i].v)dcc[dc].push_back(st[top]),top--;
	                dcc[dc].push_back(now);
				  }
	           }
		   }
		else if(i!=(fa^1))
		    low[now]=min(low[now],dfn[e[i].v]);
}
```

### 例题

例题 $1$ ：

[P3387 【模板】缩点](https://www.luogu.com.cn/problem/P3387)

缩点模板题，注意记录每个强连通分量的权值。缩完点之后，由于图是一个 DAG，所以直接拓扑排序跑一边最长路即可，不多赘述。

这里的代码实现不是很优美，可以参考其他例题的代码。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[300000],s[300000];
int n,m,u,v,a[300000],h[300000],sh[300000],dfn[300000],low[300000],st[300000],in[300000],scc[300000],q[300000],ind[300000],dis[300000],top=0,cnt=0,dfc=0,sc=0,ans=0;
void add_edge(int u,int v,struct edge e[],int h[])
{
	e[++cnt].next=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void tarjan(int now)
{
	dfn[now]=low[now]=++dfc,st[++top]=now,in[now]=1;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v])
	       {
	       	tarjan(e[i].v);
	       	low[now]=min(low[now],low[e[i].v]);
		   }
		else if(in[e[i].v])
		    low[now]=min(low[now],dfn[e[i].v]);
    if(dfn[now]==low[now])
       {
       sc++;
       while(st[top]!=now)
		 scc[st[top]]=sc,q[sc]+=a[st[top]],in[st[top]]=0,top--;
	   scc[st[top]]=sc,q[sc]+=a[st[top]],in[st[top]]=0,top--;
       }
}

void topo_sort()
{
	int h=1,t=0,que[300000];
	for(int i=1;i<=sc;i++)
	    if(ind[i]==0)dis[i]=q[i],que[++t]=i;
	while(h<=t)
	   {
	   	int now=que[h];
	   	for(int i=sh[now];i;i=s[i].next)
	   	    {
	   	        dis[s[i].v]=max(dis[s[i].v],dis[now]+q[s[i].v]);
	   	        ind[s[i].v]--;
	   	        if(ind[s[i].v]==0)que[++t]=s[i].v;
			}
		h++;
	   }
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++)scanf("%d",&a[i]);
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%d%d",&u,&v);
	    	add_edge(u,v,e,h);
		}
	for(int i=1;i<=n;i++)
	    if(!dfn[i])tarjan(i);
	cnt=0;
	for(int i=1;i<=n;i++)
	    for(int j=h[i];j;j=e[j].next)
	        if(scc[i]!=scc[e[j].v])ind[scc[e[j].v]]++,add_edge(scc[i],scc[e[j].v],s,sh);
	topo_sort();
	for(int i=1;i<=sc;i++)
	    ans=max(ans,dis[i]);
	printf("%d",ans);
	return 0;
}
```

例题 $2$ ：

[P2341 \[USACO03FALL / HAOI2006\] 受欢迎的牛 G](https://www.luogu.com.cn/problem/P2341)

由于图中可能存在环，先缩点，记录每个强连通分量的点数，之后建图。在一个强连通分量中，所有奶牛互相爱慕，只要其中一个发生变化，另外所有的都会发生变化，满足缩点的条件。

如果缩点后图不连通，那么没有任何奶牛能成为明星奶牛。如果一个强连通分量不存在出边，由于图必须连通，所以这个强连通分量只存在入边。如果不存在其他的强连通分量不存在出边，那么所有入边最终都是指向这一个强连通分量，这个强连通分量中的每一头奶牛都是明星。如果存在多于一个强连通分量不存在出边，则这些强连通分量中必然互不指向，没有一头奶牛能成为明星。

~~总结：让别人喜欢你的方法，就是不喜欢别人。~~（划掉

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[1100000],s[1100000];
int n,m,u,v,h[300000],sh[300000],dfn[300000],low[300000],st[300000],in[300000],scc[300000],q[300000],oud[300000],top=0,cnt=0,dfc=0,sc=0,ans=0;
void add_edge(int u,int v,struct edge e[],int h[])
{
	e[++cnt].next=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void tarjan(int now)
{
	dfn[now]=low[now]=++dfc,st[++top]=now,in[now]=1;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v])
	       {
	       	tarjan(e[i].v);
	       	low[now]=min(low[now],low[e[i].v]);
		   }
		else if(in[e[i].v])
		    low[now]=min(low[now],dfn[e[i].v]);
    if(dfn[now]==low[now])
       {
       sc++;
       while(st[top]!=now)
		 scc[st[top]]=sc,q[sc]++,in[st[top]]=0,top--;
	   scc[st[top]]=sc,q[sc]++,in[st[top]]=0,top--;
       }
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%d%d",&u,&v);
	    	add_edge(u,v,e,h);
		}
	for(int i=1;i<=n;i++)
	    if(!dfn[i])tarjan(i);
	cnt=0;
	for(int i=1;i<=n;i++)
	    for(int j=h[i];j;j=e[j].next)
	        if(scc[i]!=scc[e[j].v])oud[scc[i]]++,add_edge(scc[i],scc[e[j].v],s,sh);
	for(int i=1;i<=sc;i++)
	    if(oud[i]==0&&ans==0)ans=q[i];
	    else if(oud[i]==0)ans=0;
	printf("%d",ans);
	return 0;
}
```

例题 $3$ ：

[P2812 校园网络【\[USACO\]Network of Schools加强版】](https://www.luogu.com.cn/problem/P2812)

由于图中可能有环，且一个强连通分量内的点只要到达一个，其余的就也能到达，所以先缩点。

缩点之后，第一问易得结果为入度为 $0$ 的强连通分量的数量。因为入度为 $0$ 的强连通分量不可能接受到其他强连通分量的传递，必须自己拥有才行。由于图是个 DAG，所以所有入度为 $0$ 的强连通分量拥有之后，可以下传到任意一个强连通分量。

第二问，结果为入度为 $0$ 的强连通分量的数量和出度为 $0$ 的强连通分量的数量的较大值。根据题目要求，补充完整之后缩点后的整个图是一个强连通分量。让出度为 $0$ 的强连通分量连向入度为 $0$ 的强连通分量，刚好构成一个环。每一对这样连接，最终的图一定是由若干个环嵌套而成的。当然，对于每一个成对连接后多出来的入度或出度等于 $0$ 的强连通分量，需要额外连一条边到一个环中。站在两者之间较多的角度来看，就是每一个点连一条边，就相当于求两者之间的较大值。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[300000];
int n,u,h[300000],dfn[300000],low[300000],st[300000],in[300000],scc[300000],ind[300000],oud[300000],top=0,cnt=0,dfc=0,sc=0,ans=0,ic=0,oc=0;
unordered_map<long long,bool>b;
void add_edge(int u,int v)
{
	e[++cnt].next=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void tarjan(int now)
{
	dfn[now]=low[now]=++dfc,st[++top]=now,in[now]=1;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v])
	       {
	       	tarjan(e[i].v);
	       	low[now]=min(low[now],low[e[i].v]);
		   }
		else if(in[e[i].v])
		    low[now]=min(low[now],dfn[e[i].v]);
    if(dfn[now]==low[now])
       {
       sc++;
       while(st[top]!=now)
		 scc[st[top]]=sc,in[st[top]]=0,top--;
	   scc[st[top]]=sc,in[st[top]]=0,top--;
       }
}

int main()
{
	scanf("%d",&n);
	for(int i=1;i<=n;i++)
	    {
	    	u=-1;
	    	while(u!=0)
		    	{
		    	scanf("%d",&u);
		    	if(u==0)break;
		    	add_edge(i,u);
		        }
		}
	for(int i=1;i<=n;i++)
	    if(!dfn[i])tarjan(i);
	for(int i=1;i<=n;i++)
	    for(int j=h[i];j;j=e[j].next)
	        if(scc[i]!=scc[e[j].v]&&!b[(long long)scc[i]*20000+e[j].v])
			   b[(long long)scc[i]*20000+e[j].v]=1,oud[scc[i]]++,ind[scc[e[j].v]]++;
	for(int i=1;i<=sc;i++)
	    if(ind[i]==0)ans++;
	for(int i=1;i<=sc;i++)
	    {
	    if(ind[i]==0)ic++;
	    if(oud[i]==0)oc++;
	    }
	if(sc==1)ic=0,oc=0;
	printf("%d\n%d",ans,max(ic,oc));
	return 0;
}
```

例题 $4$ ：

[P2403 \[SDOI2010\] 所驼门王的宝藏](https://www.luogu.com.cn/problem/P2403)

对于没有宝藏的宫室，完全没有去的必要，不需要考虑。这样，点的数量就降低到了 $N$ 个。

考虑点与点之间连边。对于任意门，每次最多只会连 $8$ 条边，直接哈希之后暴力连边。对于横天门，我们在每一行中选出一个横天门“代表门”，同一行中的其他横天门与这个“代表门”之间连双向边，其他非横天门与这个“代表门”之间连“代表门”到其他门的单向边。纵寰门也是同理。

注意这里的连边必须时读入完所有点之后在进行，否则可能会出现部分点没有连边的情况。

连完边之后，考虑缩点，之后图就是一个 DAG，直接用拓扑排序求最长路即可（就是例题 $1$）。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[300000],s[300000];
int n,r,c,u[300000],v[300000],t[300000],h[300000],sh[300000],dfn[300000],low[300000],st[300000],in[300000],scc[300000],q[300000],ind[300000],dis[300000],dh[300000],dl[300000],top=0,cnt=0,dfc=0,sc=0,ans=0;
int dx[8]={0,0,1,-1,1,1,-1,-1};
int dy[8]={1,-1,0,0,1,-1,1,-1};
unordered_map<long long,int>hou;
void add_edge(int u,int v,struct edge e[],int h[])
{
	e[++cnt].next=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void tarjan(int now)
{
	dfn[now]=low[now]=++dfc,st[++top]=now,in[now]=1;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v])
	       {
	       	tarjan(e[i].v);
	       	low[now]=min(low[now],low[e[i].v]);
		   }
		else if(in[e[i].v])
		    low[now]=min(low[now],dfn[e[i].v]);
    if(dfn[now]==low[now])
       {
       sc++;
       while(st[top]!=now)
		 scc[st[top]]=sc,q[sc]++,in[st[top]]=0,top--;
	   scc[st[top]]=sc,q[sc]++,in[st[top]]=0,top--;
       }
}

void topo_sort()
{
	int h=1,t=0,que[300000];
	for(int i=1;i<=sc;i++)
	    if(ind[i]==0)dis[i]=q[i],que[++t]=i;
	while(h<=t)
	   {
	   	int now=que[h];
	   	for(int i=sh[now];i;i=s[i].next)
	   	    {
	   	        dis[s[i].v]=max(dis[s[i].v],dis[now]+q[s[i].v]);
	   	        ind[s[i].v]--;
	   	        if(ind[s[i].v]==0)que[++t]=s[i].v;
			}
		h++;
	   }
}

long long makepair(int x,int y)
{
	return (long long)x*200000+y;
}

int main()
{
	scanf("%d%d%d",&n,&r,&c);
	for(int i=1;i<=n;i++)
	    {
	    	scanf("%d%d%d",&u[i],&v[i],&t[i]);
	    	if(t[i]==1&&dh[u[i]]==0)dh[u[i]]=i;
	    	if(t[i]==1&&dh[u[i]]!=0)add_edge(i,dh[u[i]],e,h);
	    	if(t[i]==2&&dl[v[i]]==0)dl[v[i]]=i;
	    	if(t[i]==2&&dl[v[i]]!=0)add_edge(i,dl[v[i]],e,h);
	    	if(t[i]==3)hou[makepair(u[i],v[i])]=i;
		}
	for(int i=1;i<=n;i++)
	    {
	    if(dh[u[i]]!=0)add_edge(dh[u[i]],i,e,h);
	    if(dl[v[i]]!=0)add_edge(dl[v[i]],i,e,h);
	    for(int j=0;j<8;j++)
	    	if(hou[makepair(u[i]+dx[j],v[i]+dy[j])]!=0)add_edge(hou[makepair(u[i]+dx[j],v[i]+dy[j])],i,e,h);	    
		}
	for(int i=1;i<=n;i++)
	    if(!dfn[i])tarjan(i);
	cnt=0;
	for(int i=1;i<=n;i++)
	    for(int j=h[i];j;j=e[j].next)
	        if(scc[i]!=scc[e[j].v])ind[scc[e[j].v]]++,add_edge(scc[i],scc[e[j].v],s,sh);
	topo_sort();
	for(int i=1;i<=sc;i++)
	    ans=max(ans,dis[i]);
	printf("%d",ans);
	return 0;
}
```

例题 $5$ ：

[\[USACO15JAN\] Grass Cownoisseur G](https://www.luogu.com.cn/problem/P3119)

为了避免环的影响，先缩点。然后在缩点后的图进行记忆化搜索。还需要反向存一次边，方便计算逆行时的结果。设计状态 $f[i][j][k]$，表示从编号为 $j$ 的点搜索到编号为 $i$ 的点，是否逆行过（$k=0$ 表示没有，$k=1$ 表示有）走到的最多的点数。

转移时，类似普通遍历图，记录当前节点 $i$ 和前驱节点 $j$，递归搜索 $i$ 除 $j$ 以外的每一个邻接点，取状态最大值，加上这个强连通分量的点的数量。如果一个状态的 $k$ 值为 $0$，还需要额外处理逆行时的情况，同样不能走回 $j$ 点，取最大值。

需要注意的是，这样处理逆行是不会导致某个强连通分量重复计算的。假设某个点通过走回了一个计算过的点，那么在计算这个计算过的点时，就会直接搜索这个逆行点。故这个点必然是逆行点的前驱，而逆行转移是不会走回前驱点的，假设不成立。当然，在图中存在环时，也可能会有种情况。但是缩点之后图是一个 DAG，不存在环，依旧不成立。

状态中的 $i,j$ 分别记录可能爆空间，仔细分析下来发现 $i,j$ 其实是边上的一对点，相当于一条边。如果把 $(i,j)$ 作为一个数对哈希出来，状态数量其实就只与边数有关，不会爆空间。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next,dis;
}e[300000],s[300000];
int n,m,u,v,h[300000],sh[300000],dfn[300000],low[300000],st[300000],in[300000],scc[300000],q[300000],top=0,cnt=0,dfc=0,sc=0;
unordered_map<long long,int>f[2];
void add_edge(int u,int v,int dis,struct edge e[],int h[])
{
	e[++cnt].next=h[u];
	e[cnt].v=v;
	e[cnt].dis=dis;
	h[u]=cnt;
}

void tarjan(int now)
{
	dfn[now]=low[now]=++dfc,st[++top]=now,in[now]=1;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v])
	       {
	       	tarjan(e[i].v);
	       	low[now]=min(low[now],low[e[i].v]);
		   }
		else if(in[e[i].v])
		    low[now]=min(low[now],dfn[e[i].v]);
    if(dfn[now]==low[now])
       {
       sc++;
       while(st[top]!=now)
		 scc[st[top]]=sc,q[sc]++,in[st[top]]=0,top--;
	   scc[st[top]]=sc,q[sc]++,in[st[top]]=0,top--;
       }
}

long long makepair(int x,int y)
{
	return (long long)x*200000+y;
}

int dfs(int now,int pre,int used)
{
	int mx=-99999999;
	if(now==scc[1]&&pre!=0)return 0;
	if(f[used][makepair(now,pre)])return f[used][makepair(now,pre)];
	for(int i=sh[now];i;i=s[i].next)
	    if(s[i].v!=pre)
	       {
		   if(s[i].dis==0)mx=max(mx,dfs(s[i].v,now,used));
		   else if(s[i].dis==1&&used==0)mx=max(mx,dfs(s[i].v,now,1));
	       }
	f[used][makepair(now,pre)]=mx+q[now];
	return mx+q[now];
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%d%d",&u,&v);
	    	add_edge(u,v,0,e,h);
		}
	for(int i=1;i<=n;i++)
	    if(!dfn[i])tarjan(i);
	cnt=0;
	for(int i=1;i<=n;i++)
	    for(int j=h[i];j;j=e[j].next)
	        if(scc[i]!=scc[e[j].v])add_edge(scc[i],scc[e[j].v],0,s,sh),add_edge(scc[e[j].v],scc[i],1,s,sh);
	printf("%d",dfs(scc[1],0,0));
	return 0;
}
```

例题 $6$ ：

[P3388 【模板】割点（割顶）](https://www.luogu.com.cn/problem/P3388)

割点模板题，不多赘述。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[300000];
int n,m,u,v,root=0,h[300000],dfn[300000],low[300000],st[300000],in[300000],cut[300000],top=0,cnt=0,dfc=0,ans=0;
void add_edge(int u,int v)
{
	e[++cnt].next=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void tarjan(int now,int fa)
{
	int chi=0;
	dfn[now]=low[now]=++dfc,st[++top]=now,in[now]=1;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v])
	       {
	       	tarjan(e[i].v,now);
	       	low[now]=min(low[now],low[e[i].v]);
	       	if(low[e[i].v]>=dfn[now])
	       	   {
			   chi++;
	       	   if(now!=root||chi>1)cut[now]=1;
	           }
		   }
		else if(in[e[i].v]&&e[i].v!=fa)
		    low[now]=min(low[now],dfn[e[i].v]);
    if(dfn[now]==low[now])
       {
       while(st[top]!=now)in[st[top]]=0,top--;
	   in[st[top]]=0,top--;
       }
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%d%d",&u,&v);
	    	add_edge(u,v);add_edge(v,u);
		}
	for(int i=1;i<=n;i++)
        if(!dfn[i])root=i,tarjan(i,0);
	for(int i=1;i<=n;i++)
		if(cut[i])ans++;
	printf("%d\n",ans);
	for(int i=1;i<=n;i++)
		if(cut[i])printf("%d ",i);
	return 0;
}
```

例题 $7$ ：

[T103481 【模板】割边](https://www.luogu.com.cn/problem/T103481)

割边模板题，不多赘述。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[750000];
int n,m,u,v,root=0,h[300000],dfn[300000],low[300000],st[300000],in[300000],top=0,cnt=0,dfc=0,ans=0;
void add_edge(int u,int v)
{
	e[++cnt].next=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void tarjan(int now,int fa)
{
	dfn[now]=low[now]=++dfc,st[++top]=now,in[now]=1;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v]&&e[i].v!=fa)
	       {
	       	tarjan(e[i].v,now);
	       	low[now]=min(low[now],low[e[i].v]);
	       	if(low[e[i].v]>dfn[now])ans++;
		   }
		else if(in[e[i].v]&&e[i].v!=fa)
		    low[now]=min(low[now],dfn[e[i].v]);
    if(dfn[now]==low[now])
       {
       while(st[top]!=now)in[st[top]]=0,top--;
	   in[st[top]]=0,top--;
       }
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%d%d",&u,&v);
	    	add_edge(u,v);add_edge(v,u);
		}
	for(int i=1;i<=n;i++)
        if(!dfn[i])root=i,tarjan(i,0);
	printf("%d\n",ans);
	return 0;
}
```

例题 $8$ ：

[P8436 【模板】边双连通分量](https://www.luogu.com.cn/problem/P8436)

边双连通分量模板题，不多赘述。注意重边的处理。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[20040000];
int n,m,u,v,h[2000010],dfn[2000010],low[2000010],cut[20040000],scc[2000010],top=0,cnt=1,dfc=0,sc=0;
vector<int>scp[2000010];
void add_edge(int u,int v)
{
	e[++cnt].next=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void tarjan(int now,int fa)
{
	dfn[now]=low[now]=++dfc;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v])
	       {
	       	tarjan(e[i].v,i);
	       	low[now]=min(low[now],low[e[i].v]);
	       	if(low[e[i].v]>dfn[now])cut[i]=cut[i^1]=1;
		   }
		else if(i!=(fa^1))
		    low[now]=min(low[now],dfn[e[i].v]);
}

void dfs(int now)
{
	scc[now]=sc;
	scp[sc].push_back(now);
	for(int i=h[now];i;i=e[i].next)
	    {
	    if(scc[e[i].v]||cut[i])continue;
	    dfs(e[i].v);
	    }
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%d%d",&u,&v);
	    	add_edge(u,v);add_edge(v,u);
		}
	for(int i=1;i<=n;i++)
        if(!dfn[i])tarjan(i,0);
    for(int i=1;i<=n;i++)
        if(!scc[i])
           {
           sc++;
           dfs(i);
           }
	printf("%d\n",sc);
	for(int i=1;i<=sc;i++)
		{
			int l=scp[i].size();
			printf("%d ",l);
			for(int j=0;j<l;j++)
			    printf("%d ",scp[i][j]);
			printf("\n");
	    }
	return 0;
}
```

例题 $9$ ：

[P8435 【模板】点双连通分量](https://www.luogu.com.cn/problem/P8435)

点双连通分量模板题，不多赘述。注意重边的处理。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[20040000];
int n,m,u,v,root=0,h[2000010],dfn[2000010],low[2000010],st[2000010],cut[2000010],top=0,cnt=1,dfc=0,ans=0,dc=0;
vector<int>dcc[2000010];
void add_edge(int u,int v)
{
	e[++cnt].next=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void tarjan(int now,int fa)
{
	int chi=0;
	dfn[now]=low[now]=++dfc,st[++top]=now;
	if(now==root&&h[now]==0)dcc[++dc].push_back(st[top]),top--;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v])
	       {
	       	tarjan(e[i].v,i);
	       	low[now]=min(low[now],low[e[i].v]);
	       	if(low[e[i].v]>=dfn[now])
	       	   {
			   chi++;
	       	   if(now!=root||chi>1)
	       	      {
	       	      	dc++;
                    while(st[top+1]!=e[i].v)dcc[dc].push_back(st[top]),top--;
	                dcc[dc].push_back(now);
				  }
	           }
		   }
		else if(i!=(fa^1))
		    low[now]=min(low[now],dfn[e[i].v]);
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%d%d",&u,&v);
	    	if(u==v)continue;
	    	add_edge(u,v);add_edge(v,u);
		}
	for(int i=1;i<=n;i++)
        if(!dfn[i])
           {
		   root=i,tarjan(i,0);
		   if(top!=0)
		       {
		       dc++;
		       while(top>0)dcc[dc].push_back(st[top]),top--;
		       }
	       }
	printf("%d\n",dc);
	for(int i=1;i<=dc;i++)
		{
			int l=dcc[i].size();
			printf("%d ",l);
			for(int j=0;j<l;j++)
			    printf("%d ",dcc[i][j]);
			printf("\n");
	    }
	return 0;
}
```

例题 $10$ ：

[P3225 \[HNOI2012\] 矿场搭建](https://www.luogu.com.cn/problem/P3225)

任意一个点坍塌，其余任意一个点都有逃生的路口，自然联想到割点。在一个点双连通分量中，只有删去割点才会影响图的连通性。

如果一个点双连通分量里存在两个割点，那么其中一个割点坍塌后，图中所有的可以通过另一个割点逃往其他的点双连通分量。如果一个点双连通分量里只存在一个割点，那么这个割点坍塌后，这个点双连通分量就与整个图不再连通，这个点双连通分量内需要建一个逃生口。如果一个点双连通分量里一个割点都没有，那么这个点双连通分量需要两个逃生口，以免其中一个逃生口坍塌。

至于方案数的计算，根据乘法原理，每存在一个只存在一个割点的点双连通分量，把方案数累乘这个点双连通分量除这个割点外的点数。每存在一个没有割点的点双连通分量，那么就把方案数累乘在这个点双连通分量中选择两个不重复点的方案数。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[24000],s[24000];
int t,n,m,u,v,h[20010],d[20010],dfn[20010],low[20010],st[20010],book[20010],b[20010],g[20010],cut[20010],top=0,cnt1=1,cnt2=1,dfc=0,dc=0;
vector<int>dcc[20010],dcd[20010];
void add_edge1(int u,int v)
{
	e[++cnt1].next=h[u];
	e[cnt1].v=v;
	h[u]=cnt1;
}

void add_edge2(int u,int v)
{
	s[++cnt2].next=d[u];
	s[cnt2].v=v;
	d[u]=cnt2;
	b[u]++;b[v]++;
}

void init()
{
	memset(h,0,sizeof(h));memset(d,0,sizeof(d));memset(dfn,0,sizeof(dfn));memset(low,0,sizeof(low));
	memset(book,0,sizeof(book));memset(b,0,sizeof(b));memset(g,0,sizeof(g));memset(cut,0,sizeof(cut));memset(st,0,sizeof(st));
	cnt1=1,cnt2=1,dfc=0,n=0,dc=0,top=0;
}

void tarjan1(int now,int fa)
{
	int chi=0;
	dfn[now]=low[now]=++dfc,st[++top]=now;
	for(int i=h[now];i;i=e[i].next)
	    if(!dfn[e[i].v])
	       {
	       	tarjan1(e[i].v,i);
	       	low[now]=min(low[now],low[e[i].v]);
	       	if(low[e[i].v]>=dfn[now])
	       	   {
			   chi++;
	       	   if(now!=1||chi>1)
	       	      {
	       	      	dc++;
	       	      	cut[now]=1;
                    while(st[top+1]!=e[i].v)dcc[st[top]].push_back(dc),dcd[dc].push_back(st[top]),top--;
	                dcc[now].push_back(dc),dcd[dc].push_back(now);
				  }
	           }
		   }
		else if(i!=(fa^1))
		    low[now]=min(low[now],dfn[e[i].v]);
}

void dfs(int now)
{
	int l=dcc[now].size();
    book[now]=1;
	for(int i=0;i<l-1;i++)add_edge2(dcc[now][l-1],dcc[now][i]),add_edge2(dcc[now][i],dcc[now][l-1]);
	for(int i=h[now];i;i=e[i].next)
	    if(!book[e[i].v])dfs(e[i].v);
}

int main()
{
	while(scanf("%d",&m))
		{
		if(m==0)break;
		t++;
		init();
		long long ans1=0,ans2=1;
		for(int i=1;i<=m;i++)
		    {
		    	scanf("%d%d",&u,&v);
		    	if(u==v)continue;
		    	n=max(n,max(u,v));
		    	add_edge1(u,v);add_edge1(v,u);
			}
		tarjan1(1,0);
		if(top!=0)
		   {
		   dc++;
	       while(top>0)dcc[st[top]].push_back(dc),dcd[dc].push_back(st[top]),top--;
		   }
		dfs(1);
		for(int i=1;i<=n;i++)
		    if(cut[i])
		       {
		       	int l=dcc[i].size();
		       	for(int j=0;j<l;j++)g[dcc[i][j]]++;
			   }
		for(int i=1;i<=dc;i++)
		    if(b[i]==2||g[i]==1)
		       {
		       	long long cnt=0;
		       	ans1++;
		       	int l=dcd[i].size();
		       	for(int j=0;j<l;j++)
		       	    if(!cut[dcd[i][j]])cnt++;
		       	ans2*=cnt;
			   }
		if(ans1==0)ans1=2,ans2=(long long)n*(n-1)/2;
		printf("Case %d: %lld %lld\n",t,ans1,ans2);
		for(int i=1;i<=n;i++)dcc[i].clear();
		for(int i=1;i<=dc;i++)dcd[i].clear();
	    }
	return 0;
}
```

### 后记

这篇成功取代 [【6】树状数组学习笔记](https://www.cnblogs.com/w9095/p/18701660)，共 $1092$ 行，成为最长的学习笔记。

upd on $2023.12.7$ 被 [【7】同余学习笔记](https://www.cnblogs.com/w9095/p/18704166) 取代。