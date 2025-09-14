# 【8】2-SAT学习笔记

### 前言

WFLS 暑假集训 Day10

2-SAT 是一个比较难的算法，属于省选算法，当时学的不好，可能讲的不是很透彻。

### 2-SAT

---

有 $n$ 个布尔变量 $x_1\sim x_n$，另有 $m$ 个需要满足的条件，每个条件的形式都是 「$x_i$ 为 `true` / `false` 或 $x_j$ 为 `true` / `false`」。比如 「$x_1$ 为真或 $x_3$ 为假」、「$x_7$ 为假或 $x_2$ 为假」。

2-SAT 问题的目标是给每个变量赋值使得所有条件得到满足。

---

由于每个点都需要维护一个额外信息 `true` 或 `false`，先拆点，令点 $i$ 为变量 $x_i$ 取值为 `false`，令点 $i+n$ 为变量 $x_i$ 取值为 `true`，则问题就转变为了在每一对点 $(i,i+n)$ 中取一个点，并满足所有限制条件。

限制条件形如 $x_i$ 为 $a(a\in \{0,1\})$ 或 $x_j$ 为 $b(b\in \{0,1\})$，所以，如果 $x_i$ 为 $a$，则 $x_j$ 不能为 $b$。又因为变量取值只有 $0$ 和 $1$，所以 $x_j$ 必须为非 $b$。如果 $x_j$ 为 $b$ 也是同理。

我们可以连边 $i\to j$，表示选择了 $i$ 就必须选择 $j$。这样，就可以把这些点的关系在图上表示出来。

每一对点 $(i,i+n)$ 中只能取一个点，所以如果能够直接或间接地 $i\to i+n$ 且 $i+n\to i$，表示这两个点中选取任意一个点，都必须要选取另一个点，很明显，这种情况是无解的。在这种情况下，这两个点在同一个强连通分量中。

由于我们选择一个点，就必然会影响这个点指向的点。所以如果一个点拓扑序越大，其会影响的点数就越少，取这个点的成功率就越高。所以，在每一对 $(i,i+n)$ 中，我们选取拓扑序较大的点对应的取法。

在我们 Tarjan 求强连通分量的过程中，其实已经求出了反拓扑序。仔细观察 Tarjan 的过程，我们发现 Tarjan 的节点是后进先出，相当于求出了拓扑序的反序。Tarjan 序（强连通分量编号）越小，拓扑序越大，对应的选择越优。

代码实现的话看下面例题。

### 例题

例题 $1$ ：

[P4782 【模板】2-SAT 问题](https://www.luogu.com.cn/problem/P4782)

2-SAT 模板题，不多赘述。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[2000010];
int n,m,x1,x2,a,b,h[2000010],y[2000010],dfn[2000010],low[2000010],st[2000010],in[2000010],scc[2000010],dfc=0,top=0,sc=0,cnt=0;
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
       while(st[top]!=now)scc[st[top]]=sc,in[st[top]]=0,top--;
	   scc[st[top]]=sc,in[st[top]]=0,top--;
       }
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++)
	    y[i]=i+n,y[i+n]=i;
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%d%d%d%d",&x1,&a,&x2,&b);
	    	if(a)x1=y[x1];
	    	if(b)x2=y[x2];
	    	add_edge(x1,y[x2]);add_edge(x2,y[x1]);
		}
	for(int i=1;i<=n*2;i++)
	    if(!dfn[i])tarjan(i);
	for(int i=1;i<=n;i++)
	    if(scc[i]==scc[y[i]])
	       {
	       	printf("IMPOSSIBLE");
	       	return 0;
		   }
	printf("POSSIBLE\n");
	for(int i=1;i<=n;i++)
	    if(scc[i]<scc[y[i]])printf("1 ");
	    else printf("0 ");
	return 0;
}
```

例题 $2$ ：

[P4171 \[JSOI2010\] 满汉全席](https://www.luogu.com.cn/problem/P4171)

我们发现，每一道菜都只有满式和汉式两种做法，而每个评委的喜好相当于一次限定。自然联想到 2-SAT。

拆点，点 $i$ 表示第 $i$ 个点做满式，$i+n$ 第 $i$ 个点做汉式。每次评委的限制相当于两个点的这两个状态二选一，相当于 2-SAT 模板的 「$x_i$ 为 `true` / `false` 或 $x_j$ 为 `true` / `false`」。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[2000010];
int t,n,m,x1,x2,no,h[2000010],y[2000010],dfn[2000010],low[2000010],st[2000010],in[2000010],scc[2000010],dfc=0,top=0,sc=0,cnt=0;
char c1,c2;
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
       while(st[top]!=now)scc[st[top]]=sc,in[st[top]]=0,top--;
	   scc[st[top]]=sc,in[st[top]]=0,top--;
       }
}

int main()
{
	scanf("%d",&t);
	while(t--)
		{
		bool flag=0;
		scanf("%d%d",&n,&m);
		for(int i=1;i<=n;i++)
		    y[i]=i+n,y[i+n]=i;
		for(int i=1;i<=n*2;i++)
		    dfn[i]=low[i]=h[i]=scc[i]=0;
		dfc=cnt=sc=0;
		scanf("%c",&no);
		for(int i=1;i<=m;i++)
		    {
		    	scanf("%c%d%c%c%d",&c1,&x1,&no,&c2,&x2);
		    	scanf("%c",&no);
		    	if(c1=='h')x1=y[x1];
		    	if(c2=='h')x2=y[x2];
		    	add_edge(x1,y[x2]);add_edge(x2,y[x1]);
		    }
		for(int i=1;i<=n*2;i++)
		    if(!dfn[i])tarjan(i);
		for(int i=1;i<=n;i++)
		    if(scc[i]==scc[y[i]])
		       flag=1;
		if(flag)printf("BAD\n");
		else printf("GOOD\n");
	    }
	return 0;
}
```

例题 $3$ ：

[P5782 \[POI2001\] 和平委员会](https://www.luogu.com.cn/problem/P5782)

由于每个党派是两个代表二选一，同时也有类似 「$x_i$ 为 `true` / `false` 或 $x_j$ 为 `true` / `false`」 的限制条件，联想到 2-SAT。

由于题目已经规定了每个代表的编号，所以不用自己拆点。对于互相厌恶的 $x,y$，则建边 $x\to y'$，$y\to x'$。最后直接按 2-SAT 的方式处理即可。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[2000010];
int n,m,x1,x2,h[2000010],y[2000010],dfn[2000010],low[2000010],st[2000010],in[2000010],scc[2000010],dfc=0,top=0,sc=0,cnt=0;
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
       while(st[top]!=now)scc[st[top]]=sc,in[st[top]]=0,top--;
	   scc[st[top]]=sc,in[st[top]]=0,top--;
       }
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++)
	    y[i*2]=i*2-1,y[i*2-1]=i*2;
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%d%d",&x1,&x2);
	    	add_edge(x1,y[x2]);add_edge(x2,y[x1]);
		}
	for(int i=1;i<=n*2;i++)
	    if(!dfn[i])tarjan(i);
	for(int i=1;i<=n;i++)
	    if(scc[i]==scc[y[i]])
	       {
	       	printf("NIE\n");
	       	return 0;
		   }
	for(int i=1;i<=n*2;i+=2)
	    if(scc[i]<scc[y[i]])printf("%d\n",i);
	    else printf("%d\n",y[i]);
	return 0;
}
```

例题 $4$ ：

[P3825 \[NOI2017\] 游戏](https://www.luogu.com.cn/problem/P3825)

比较难的 2-SAT 题目。

不算地图为 $x$ 的情况，由于每种地图必然排除一种赛车，所以实际上每种地图只能选择两种赛车。自然联想到 2-SAT。

注意到 $d$ 很小，考虑暴搜每个 $x$ 的状态($x\in{a,b,c}$)，将 $x$ 类型的地图也转化为可以使用 2-SAT 求解。

记 $x'$ 为点 $x$ 另一个状态的对应点，则对于一个限制条件选点 $x$ 必须选点 $y$，首先建边 $x\to y$。然后考虑如果不选 $y$，选 $y'$，则选 $x$ 的话只能选择 $y'$，与限制条件冲突，所以只能选 $x'$，于是建边 $y'\to x'$。

注意，可能会出现限制条件中的点因为地图限制不能被选择的情况。如果是起点不能被选择，那么可以直接忽略这个限制。如果终点不能被选择，那么选择起点就必须选择一个不能被选择的点，则起点不能被选择。可以直接让起点连边到其对应点，由 2-SAT 的构造方案的方式，我们知道这相当于标记这个点不能选。

跑一边 2-SAT，如果存在一组方案，可以直接输出，并直接结束程序。如果不存在，继续搜索下一种情况。

另外，注意对于 $x$ 类型的地图，只需要枚举这个地图被看作 $a$ 或 $b$。因为 $a$ 包含了 $bc$ 的情况，$b$ 包含了 $ac$ 的情况，合在一起已经包含了所有情况。

实现比较复杂，对应关系要写清楚。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,next;
}e[300010];
int n,m,d,f[100010],t[100010],h[100010],y[100010],dfn[100010],low[100010],st[100010],in[100010],scc[100010],dfc=0,top=0,sc=0,cnt=0,flag=0;
char map1[100010],s[100010],l[100010];
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
       while(st[top]!=now)scc[st[top]]=sc,in[st[top]]=0,top--;
	   scc[st[top]]=sc,in[st[top]]=0,top--;
       }
}

void init()
{
	memset(h,0,sizeof(h));memset(dfn,0,sizeof(dfn));memset(scc,0,sizeof(scc));
	dfc=0,top=0,sc=0,cnt=0;
}

int get_pid(int now,char p)
{
	int ban=map1[now]-'a'+'A';
	if(ban=='A')return (p=='B')?0:1;
	if(ban=='B')return (p=='A')?0:1;
	if(ban=='C')return (p=='A')?0:1;
}

void check()
{
	if(flag==1)return;
	init();
	for(int i=1;i<=m;i++)
	    {
	    if(s[i]==map1[f[i]]-'a'+'A')continue;
	    if(l[i]==map1[t[i]]-'a'+'A')
	       {
	       	int pi=get_pid(f[i],s[i]);
	       	add_edge(f[i]+pi*n,y[f[i]+pi*n]);
		   }
		add_edge(f[i]+get_pid(f[i],s[i])*n,t[i]+get_pid(t[i],l[i])*n);
		add_edge(y[t[i]+get_pid(t[i],l[i])*n],y[f[i]+get_pid(f[i],s[i])*n]);
	    }
	for(int i=1;i<=n*2;i++)
		if(!dfn[i])tarjan(i);
	for(int i=1;i<=n;i++)
		if(scc[i]==scc[y[i]])return;
	flag=1;
	for(int i=1;i<=n;i++)
		if(scc[i]<scc[y[i]])
		   {
		   if(map1[i]=='a')printf("B");
		   else printf("A");
		   }
		else
		   {
		   if(map1[i]=='c')printf("B");
		   else printf("C");
		   }
}

void dfs(int now)
{
	if(flag==1)return;
	if(now==n+1)
	   {
	   	check();
	   	return;
	   }
	if(map1[now]=='x')
	   for(int i=0;i<2;i++)
	       {
	       	map1[now]='a'+i;
	       	dfs(now+1);
	       	map1[now]='x';
		   }
	else dfs(now+1);
}

int main()
{
	scanf("%d%d",&n,&d);
	scanf("%s",map1+1);
	scanf("%d",&m);
	for(int i=1;i<=m;i++)
	    scanf("%d %c %d %c",&f[i],&s[i],&t[i],&l[i]);
	for(int i=1;i<=n;i++)
		y[i]=i+n,y[i+n]=i;
	dfs(1);
	if(flag==0)printf("-1");
	return 0;
}
```

### 后记

由于是多校联训~~上课时~~期间写的，所以比较仓促，很多地方写的不够清楚，有时间的话会回来修一修。

~~但是一般没时间~~