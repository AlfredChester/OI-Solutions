# 【6】树形DP学习笔记

### 前言

教练说过，树形 DP 是一个抽象的东西，很多状态比较难以理解，后面具体的学习方法，忘了。

UPD on $2024.11.21$：修复了例题 $5$ 的假做法和假代码。

### 普通树形 DP

树形 DP 是一类在树上的动态规划，通常以**节点位置**作为**阶段**，树中的**父子关系**为**转移**，**边界状态**是**叶子节点**，**目标**是**根节点**。记 $son(x)$ 为 $x$ 的子节点的集合，$op(x,y)$ 为 $x\to y$ 转移的贡献，则树形 DP 的状态转移方程类似这两种形式：

$$
f_x=\max_{y\in son(x)}/\min_{y\in son(x)}\{f_{x},f_y+op(x,y)\}
$$

$$
f_x=\sum_{y\in son(x)}f_y+op(x,y) 
$$

由于跨层转移难以处理，所以我们一般设计可以父子转移的状态。

例题 $1$：

[P1352 没有上司的舞会](https://www.luogu.com.cn/problem/P1352)

令 $1$ 为根节点。设状态 $f_{x,0}$ 表示点 $x$ 不去的情况，$f_{x,1}$ 表示点 $x$ 去的情况。

如果点 $x$ 不去，则它的儿子节点可以去或者不去，在 $f_{y,0}$ 和 $f_{y,1}$ 中取较小值。因此，有以下转移方程：

$$
f_{x,0}=\sum_{y\in son(x)}\min(f_{y,0},f_{y,1}) 
$$

如果点 $x$ 去，则它的儿子节点不可以去，取 $f_{y,0}$。因此，有以下转移方程：

$$
f_{x,0}=\sum_{y\in son(x)}f_{y,0} 
$$

边界情况是叶节点 $x$，$f_{x,0}=0,f_{x,1}=a_x$，目标是是根节点，可以去或不去，取 $f_{1,0}$ 和 $f_{1,1}$ 的较小值。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,nxt;
}e[400000];
int n,m,u,v,a[400000],h[400000],f[400000][2],cnt=0;
void add_edge(long long u,long long v)
{
	e[++cnt].nxt=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void dfs(long long x,long long fa)
{
	f[x][1]=a[x];
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=fa)
	       {
		   dfs(e[i].v,x);
		   f[x][0]+=max(f[e[i].v][0],f[e[i].v][1]);
		   f[x][1]+=f[e[i].v][0];
	       }
}

int main()
{
	scanf("%d",&n);
	for(int i=1;i<=n;i++)scanf("%d",&a[i]);
	for(int i=1;i<=n-1;i++)
	    {
	    	scanf("%d%d",&u,&v);
	    	add_edge(u,v),add_edge(v,u);
		}
	dfs(1,0);
	printf("%lld",max(f[1][0],f[1][1]));
	return 0;
}
```

例题 $2$：

[CF1929D Sasha and a Walk in the City](https://www.luogu.com.cn/problem/CF1929D)

我们把选取到的点称为黑点，由题意得，一个合法的点集能使树中任意一条简单路径上的黑点数量不超过两个。也就是说，如果黑点数量多于 $2$，对于任意两个黑点，它们如果在同一个节点的子树内，必然是兄弟关系。否则，一旦存在祖先关系，由于黑点数量多于 $2$，必然有一个黑点可以与这两个点组成一条黑点数量超过两个的简单路径。

设状态 $f_{i,j}$ 表示以 $i$ 为根的子树内，从根到叶子节点最多经过 $j$ 个黑点。显然，只有 $j$ 等于 $0,1$ 或 $2$ 时，状态是合法的。

考虑如何转移，对于 $f_{i,0}$，显然等于 $1$。

对于 $f_{i,1}$，由于根到叶子节点最多经过黑点数最大值为 $1$，最多就是两条到根的路径合并，经过 $1+1=2$ 个黑点，所以每一个儿子内可以任意选择。另外，每个子树内还可以从没有黑点涂子树的根上的黑点变成有 $1$ 个黑点，所以还需要加入没有黑点的方法数。根据乘法原理，可以推出如下转移式：

$$
f_{i,1}=\prod_{j\in son(i)}(f_{j,0}+f_{j,1})=\prod_{j\in son(i)}(1+f_{j,1}) 
$$

对于 $f_{i,2}$，由于根到叶子节点最多经过黑点数最大值为 $2$，所以只能有一个子树内的 $f$ 值可以转移来，否则必然可以构造一条黑点数量超过两个的简单路径。另外，每个子树内还可以从 $1$ 个黑点涂子树的根上的黑点变成有 $2$ 个黑点，所以还需要加入 $1$ 个黑点的方法数。根据加法原理，可以推出如下转移式：

$$
f_{i,2}=\sum_{j\in son(i)}(f_{j,1}+f_{j,2}) 
$$

以 $1$ 为整棵树的根，最后的答案为 $f_{1,0}+f_{1,1}+f_{1,2}$，也就是 $1+f_{1,1}+f_{1,2}$。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	long long v,nxt;
}e[800000];
long long t,u,v,n,mod=998244353,h[800000],f[800000][3],cnt=0;
void add_edge(long long u,long long v)
{
	e[++cnt].nxt=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}
 
void init()
{
	for(int i=1;i<=n;i++)h[i]=0,f[i][1]=1,f[i][2]=0;
	cnt=0;
}
 
void dfs(long long x,long long fa)
{
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=fa)
	       {
		   dfs(e[i].v,x);
		   f[x][1]=(f[x][1]*(f[e[i].v][1]+1)%mod)%mod; 
		   f[x][2]=(f[x][2]+f[e[i].v][1]+f[e[i].v][2])%mod;
	       }
}
 
int main()
{
	scanf("%lld",&t);
	while(t--)
	   {
	   	scanf("%lld",&n);
	   	init();
	   	for(int i=1;i<=n-1;i++)
	   	    {
	   	    scanf("%lld%lld",&u,&v);
	   	    add_edge(u,v),add_edge(v,u);
			}
		dfs(1,0);
		printf("%lld\n",(f[1][1]+f[1][2]+1)%mod);
	   }
	return 0;
}
```

例题 $3$：

[P2279 \[HNOI2003\] 消防局的设立](https://www.luogu.com.cn/problem/P2279)

主要参考 [这篇题解](https://www.luogu.com.cn/article/4gqtwpgc)。

对于每个点 $x$，有五个状态：

$f_{x,0}$ 表示覆盖到 $x$ 向上 $2$ 层的最少消防局个数。

$f_{x,1}$ 表示覆盖到 $x$ 向上 $1$ 层的最少消防局个数。

$f_{x,2}$ 表示覆盖到 $x$ 这一层的最少消防局个数。

$f_{x,3}$ 表示覆盖到 $x$ 向下 $1$ 层的最少消防局个数。

$f_{x,4}$ 表示覆盖到 $x$ 向下 $2$ 层的最少消防局个数。

显然，状态之间具有包含关系，即 $f_{x,0}\ge f_{x,1}\ge f_{x,2}\ge f_{x,3}\ge f_{x,4}$，在求解时需要注意与更大的取最小值。

$$
f_{x,0}=1+\sum_{y\in son(x)}f_{y,4} 
$$

因为 $x$ 可以覆盖到向上 $2$ 层，所以它自己必须是消防站。此时，它可以覆盖到所有儿子和孙子，因此儿子的状态可以是 $f_{y,0\sim4}$ 中的任意一种情况。但因为我们需要使得消防站个数最少，所以取 $f_{y,4}$。

$$
f_{x,1}=\min_{y\in son(x)}(f_{y,0}+\sum_{z\in son(y)}f_{z,3}) 
$$

覆盖到向上 $1$ 层，那么 $x$ 的至少一个儿子是消防站，可以覆盖到向上 $2$ 层，故取 $f_{y,0}$。其它儿子的状态可以是 $f_{z,0\sim3}$ 中的任意一种。同样，因为要取消防站个数最小值，取 $f_{z,3}$。

$$
f_{x,2}=\min_{y\in son(x)}(f_{y,1}+\sum_{z\in son(y)}f_{z,2}) 
$$

覆盖到这一层，那么 $x$ 的至少一个儿子的下一层是消防站，可以覆盖到向上 $1$ 层，故取 $f_{y,1}$。其它儿子的状态可以是 $f_{z,0\sim2}$ 中的任意一种。同样，因为要取消防站个数最小值，取 $f_{z,2}$。

$$
f_{x,3}=\sum_{y\in son(x)}f_{x,2} 
$$

覆盖到向下 $1$ 层，即所有儿子被覆盖就可以，取 $f_{y,2}$。

$$
f_{x,4}=\sum_{y\in son(x)}f_{y,3} 
$$

覆盖到向下 $1$ 层，即所有儿子的下一层被覆盖就可以，取 $f_{x,3}$。

注意要依次取最小值，以 $1$ 号节点为根，最终目标是 $f_{1,2}$。叶子节点 $x$ 边界情况，$f_{x,0}=f_{x,1}=f_{x,2}=1,f_{x,3}=f_{x,4}=0$。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	int v,nxt;
}e[5000];
int n,a,h[5000],f[5000][5],cnt=0;
void add_edge(int u,int v)
{
	e[++cnt].nxt=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void dfs(int now,int fa)
{
	f[now][0]=1;
	for(int i=h[now];i;i=e[i].nxt)
	    if(e[i].v!=fa)
	       {
		   dfs(e[i].v,now);
		   f[now][0]+=f[e[i].v][4];
		   f[now][3]+=f[e[i].v][2];
		   f[now][4]+=f[e[i].v][3];
	       }
	if(e[h[now]].v==fa&&e[h[now]].nxt==0)f[now][1]=f[now][2]=1;
	else
	   {
	   	f[now][1]=f[now][2]=1e5;
	   	for(int i=h[now];i;i=e[i].nxt)
	   	    if(e[i].v!=fa)
		   	    {
		   	    int f1=f[e[i].v][0],f2=f[e[i].v][1];
		   	    for(int j=h[now];j;j=e[j].nxt)
				    if(e[j].v!=fa&&e[j].v!=e[i].v)f1+=f[e[j].v][3],f2+=f[e[j].v][2];
				f[now][1]=min(f[now][1],f1);
				f[now][2]=min(f[now][2],f2);	
				}
	   }
	for(int i=1;i<=4;i++)f[now][i]=min(f[now][i],f[now][i-1]);
}

int main()
{
	scanf("%d",&n);
	for(int i=1;i<=n-1;i++)
	    {
	    	scanf("%d",&a);
	    	add_edge(a,i+1),add_edge(i+1,a);
		}
	dfs(1,0);
	printf("%d\n",f[1][2]);
	return 0;
}
```

### 树上背包

树上背包是一类经典的树形 DP 问题,是**树形 DP** 和**背包 DP** 的结合。

树上背包问题的转移方程式至少拥有 $2$ 个维度，一个是来自树形 DP 的**节点维度**，另一个是来自背包 DP 的**容量维度**。

转移时，我们一般以节点维度为**阶段**。在单个节点进行转移时，根据题目要求，枚举每一个**子节点**，对于这个子节点的每一种选法，当作**分组背包**处理。

树上背包问题的复杂度一般为 $O(n^3)$，但是有的可以使用特殊方法优化到 $O(n^2)$。

例题 $4$ ：

[P2014 \[CTSC1997\] 选课](https://www.luogu.com.cn/problem/P2014)

树上背包模板题，定义状态 $f[x][k]$ 为在以节点 $x$ 为根的子树中，学习 $k$ 门可以获得的最大学分。根据背包类 DP 的过程，不难得出以下转移方程：($d$ 为枚举子树 $u$ 所占的空间)

$$
f[x][k]=\max\{f[x][k],f[x][k-d]+f[u][d]\}(u\in son(x),d\le k) 
$$

采用分组背包的转移方式，每个子树的各个 $d$ 的状态分为一组。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	long long v,nxt;
}e[400000];
long long n,m,u,h[400000],f[400][400],cnt=0;
void add_edge(long long u,long long v)
{
	e[++cnt].nxt=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void dfs(long long x,long long fa)
{
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=fa)
	       {
	       	dfs(e[i].v,x);
	       	for(int j=m;j>0;j--)
	       		for(int k=0;k<j;k++)
	       	        f[x][j]=max(f[x][j],f[x][j-k]+f[e[i].v][k]);
		   }
}

int main()
{
	scanf("%lld%lld",&n,&m);
	m++;
	for(int i=1;i<=n;i++)
	    {
	    	scanf("%lld%lld",&u,&f[i][1]);
	    	add_edge(i,u),add_edge(u,i);
		}
	dfs(0,-1);
	printf("%lld",f[0][m]);
	return 0;
}
```

例题 $5$ ：

[P1273 有线电视网](https://www.luogu.com.cn/problem/P1273)

直接以价值作为第二维显然会超时，那么就换思路，定义状态 $f[x][i]$ 为子树 $x$ 中使 $i$ 个人可以看到比赛的最大收益，则最终结果为满足 $f[1][i]\ge0$ 的最大的 $i$ 的值(题目已经说明 $1$ 为整棵树的树根)。

记 $x\to y$ 的边权为 $c(x,y)$，参照树上背包的转移方式，容易得出以下转移方程：

$$
f[x][i]=\max\{f[x][i-j]+f[y][j]-c(x,y)\}(j\le i,y\in son(x)) 
$$

初始值 $f[x][0]=1$，$f[l][1]=a[l]$，其中 $l$ 为叶子节点，其余为负无穷。

这样做时间复杂度为 $O(n^3)$，会超时，我们使用一个优化。我们发现，每一个节点 $x$ 使 $i$ 个人观看，则 $i$ 最多为 $x$ 的子树大小。利用这一点进行优化，时间复杂度为 $O(n^2)$。以下是证明：[子树合并背包类型的dp的复杂度证明](https://blog.csdn.net/lyd_7_29/article/details/79854245)

注意此时合并时相较普通的背包 DP，此时需要枚举的是 $j-k$ 和 $k$，因为这样对于每一棵枚举量是已枚举的子树的大小之和加 $1$ 乘以这一棵子树的大小和。这才是上述证明中提到的复杂度。如果还是枚举 $j$ 和 $k$，其实一条链就被卡掉了，我在这里假了一年，谢罪。

事实上，所有子树合并类树上背包都可以这样优化。若第二维为 $m$，则复杂度从 $O(nm^2)\to O(nm)$。

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m,k,a,c,f[3010][3010],g[3010],siz[3010],ans=-1e9;
vector<int>t[3010],s[3010];
void dfs(int x)
{
	siz[x]=1;
	int l=t[x].size();
	for(int i=0;i<l;i++)
		{
		dfs(t[x][i]);
		for(int j=0;j<=n;j++)g[j]=f[x][j];
		for(int j=0;j<=siz[x];j++)
		    for(int k=0;k<=siz[t[x][i]];k++)
		    	f[x][j+k]=max(f[x][j+k],g[j]+f[t[x][i]][k]-s[x][i]);
		siz[x]+=siz[t[x][i]];
	    }
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n-m;i++)
	    {
	    	scanf("%d",&k);
	    	for(int j=1;j<=k;j++)
	    	    {
	    	    	scanf("%d%d",&a,&c);
	    	    	t[i].push_back(a),s[i].push_back(c);
				}
		}
	for(int i=1;i<=n;i++)
	    for(int j=1;j<=n;j++)
	        f[i][j]=-1e9;
	for(int i=1;i<=m;i++)scanf("%d",&f[n-m+i][1]);
	dfs(1);
	for(int i=0;i<=n;i++)
	    if(f[1][i]>=0)ans=max(ans,i);
	printf("%d",ans);
	return 0;
}
```

例题 $6$ ：

[P4037 \[JSOI2008\] 魔兽地图](https://www.luogu.com.cn/problem/P4037)

题目中装备有购买限制，所以二维树上背包状态肯定无法表示。又由于每件装备的合成只与其子节点的合成数量有关，所以需要一维表示这一个装备合成多少个，这样刚好可以进行父子之间的转移。

设状态 $f[x][y][z]$ 表示第 $x$ 件装备合成 $z$ 个，使用 $y$ 个金币可以达到的最大价值。

初始时，$f[x][y][z]$ 为负无穷。对于叶子节点，直接枚举购买数量，计算需要的金币，记录状态。

我们枚举 $z$，经过手推发现直接转移是不行的，所以考虑记录辅助转移数组 $g[i][j]$ 表示第 $x$ 件装备合成 $z$ 个时，考虑到第 $i$ 棵子树，使用了 $j$ 个金币。

我们发现，每一棵子树都必须达到可以合成 $z$ 个第 $x$ 件装备的数量。也就是说，第 $i$ 棵子树的装备至少合成 $w_{x,i}\times z$ 个，且不能不选，其中 $w_{x,i}$ 为合成 $x$ 需要的 $i$ 的数量。由于不能不选，所以 $g[i][j]$ 的初值为负无穷，$g[0][0]=0$。

接下来，我们使用类似分组背包的转移方式。对于第 $i$ 棵子树，在合成数量大于 $w_{x,i}\times z$ 的情况下任意选择，也就是枚举这个子树使用的金币 $k$，合成的数量 $l$。易得如下转移方程：($s_{x,i}$ 为第 $i$ 棵子树对应到的编号)

$$
g[i][j]=\max\{g[i-1][j],g[i][j-k]+f[s_{x,i}][j][l]\}(0\le k\le j,w_{x,i}\times z\le l \le 100) 
$$

上述过程可以使用滚动数组优化空间。转移结束后，令 $f[x][y][z]=g[c_x][y]$，其中 $c_x$ 为 $x$ 的子树数量，并计算合成的贡献。

这样做复杂度较高，为 $O(100^2nm^2)$。我们注意到如果倒序枚举 $z$，那么对于确定的 $i,j$，$f[s_{x,i}][j][l]$ 组成的集合元素数量是单增不降的。我们可以使用一个变量来维护，就不需要枚举 $l$ 了，时间复杂度为 $O(100nm^2)$。

这样还是会超时。我们发现其实对于一些高级装备，它们最多被合成的数量其实不大。我们可以把这个数量预处理出来，记为 $y_x$，$z$ 就只需要从 $y_x$ 枚举到 $0$ 即可。

经过上述优化，代码成功通过此题。

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m,c,x,ind[60],a[60],b[60],t[60],y[60],s[60][60],w[60][60],f[60][2001][101],g[2][2001],mx[60][2001],ans=-1e9;
char op;
void prework(int x)
{
	if(t[x]==0)return;
	y[x]=1e9;
	for(int i=1;i<=t[x];i++)
	    {
	    prework(s[x][i]);
	    y[x]=min(y[x],y[s[x][i]]/w[x][i]);
	    }
}

void dfs(int x)
{
	int now=0;
	if(t[x]==0)return;
	for(int i=1;i<=t[x];i++)dfs(s[x][i]);
	for(int i=1;i<=t[x];i++)
		for(int j=0;j<=m;j++)
		    mx[s[x][i]][j]=-1e9;
	for(int i=1;i<=t[x];i++)
		for(int j=0;j<=m;j++)
		    for(int p=(y[x]+1)*w[x][i];p<=100;p++)
		        mx[s[x][i]][j]=max(mx[s[x][i]][j],f[s[x][i]][j][p]);
	for(int k=y[x];k>=0;k--)
		{
		for(int i=1;i<=t[x];i++)
		    for(int j=0;j<=m;j++)
		        if(k*w[x][i]<=100)
		           for(int p=k*w[x][i];p<=min((k+1)*w[x][i],100);p++)
		               mx[s[x][i]][j]=max(mx[s[x][i]][j],f[s[x][i]][j][p]);
		for(int j=0;j<=m;j++)g[now][j]=g[now^1][j]=-1e9;
		g[now][0]=0;
		for(int i=1;i<=t[x];i++)
		    {
		    for(int j=0;j<=m;j++)g[now^1][j]=-1e9;
		    for(int j=0;j<=m;j++)
		        for(int p=0;p<=j;p++)
		            g[now^1][j]=max(g[now^1][j],g[now][j-p]+mx[s[x][i]][p]);
		    now^=1;
		    }
		for(int i=0;i<=m;i++)
		    if(g[now][i]!=-1e9)f[x][i][k]=g[now][i]+b[x]*k;
	    }
}

int main()
{
	cin>>n>>m;
	for(int i=1;i<=n;i++)
	    for(int j=0;j<=m;j++)
	        for(int k=0;k<=100;k++)
	            f[i][j][k]=-1e9;
	for(int i=1;i<=n;i++)
	    {
	    	cin>>a[i]>>op;
	    	if(op=='A')
	    	    {
	    	    	b[i]=a[i];
	    	    	cin>>t[i];
	    	    	for(int j=1;j<=t[i];j++)cin>>s[i][j],cin>>w[i][j],ind[s[i][j]]++;
				}
			else if(op=='B')
			    {
			    	cin>>c>>x;
			    	y[i]=x;
			    	for(int j=0;j<=x;j++)
					    if(c*j<=m)f[i][c*j][j]=a[i]*j;
				}
		}
    for(int i=1;i<=n;i++)
        if(t[i])
           for(int j=1;j<=t[i];j++)b[i]-=a[s[i][j]]*w[i][j];
    for(int i=1;i<=n;i++)
	    if(ind[i]==0)prework(i);
	for(int i=1;i<=n;i++)
	    if(ind[i]==0)dfs(i);
	for(int i=1;i<=n;i++)
	    if(ind[i]==0)
	       {
	       	for(int j=0;j<=m;j++)
	       	    for(int k=0;k<=100;k++)
	       	        ans=max(ans,f[i][j][k]);
	       	cout<<ans;
		   }
	return 0;
}
```

### 换根 DP

换根 DP 是一类特殊的树形 DP，其特点是每个节点的贡献**不仅**与子树内有关，也与**子树外**有关。这种情况，我们就需要先做一遍**普通树形 DP**，求出**根节点**对应的值。然后，**从上往下**进行**换根**，每次计算从**父亲**到**儿子**的变化，直到遍历完整棵树。

这种问题一般需要两个转移方程，一个是树形 DP 转移方程，另一个是换根转移方程。

例题 $7$ ：

[P3478 \[POI2008\] STA-Station](https://www.luogu.com.cn/problem/P3478)

首先，先不考虑子树外的点的贡献。设状态 $f[x]$ 表示点 $x$ 的子树内的点到点 $x$ 的深度和。易得如下转移方程：(记 $s[x]$ 为 $x$ 的子树大小)

$$
f[x]=\sum_{y\in son(x)}f[y]+s[y] 
$$

因为所有点都需要深度到 $y$ 之后再往上增加 $1$，总共就增加了 $s[y]$。

设状态 $g[x]$ 表示点 $i$ 为根时所有点的深度和。记树根为 $rt$，我们发现，$g[rt]=f[rt]$，因为这个节点子树外没有点。我们以这个点为初始根，进行换根。易得如下转移方程：

$$
g[x]=g[y]+n-2\times s[y](x\in son(y)) 
$$

因为当根从 $x$ 换到 $y$ 时，$y$ 子树内的 $s[y]$ 个节点深度减 $1$，$y$ 子树外的 $n-s[y]$ 个节点深度加 $1$，总变量就是 $-s[y]+n-s[y]=n-2\times s[y]$。

最后遍历一遍，找出 $g[x]$ 的最大值即可。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	long long v,nxt;
}e[2000000];
long long n,u,v,siz[2000000],f[2000000],g[2000000],h[2000000],cnt=0,ans=0,pl=0;
void add_edge(long long u,long long v)
{
	e[++cnt].nxt=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void dfs1(long long x,long long fa)
{
	siz[x]=1;
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=fa)
	       {
	       	dfs1(e[i].v,x);
	       	siz[x]+=siz[e[i].v],f[x]+=(f[e[i].v]+siz[e[i].v]);
		   }
}

void dfs2(long long x,long long fa)
{
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=fa)
	       {
	       	g[e[i].v]=g[x]+n-2*siz[e[i].v];
	       	dfs2(e[i].v,x);
		   }
}

int main()
{
	scanf("%lld",&n);
	for(int i=1;i<=n-1;i++)
	    {
	    	scanf("%lld%lld",&u,&v);
	    	add_edge(u,v),add_edge(v,u);
		}
	dfs1(1,0);
	g[1]=f[1];
	dfs2(1,0);
	for(int i=1;i<=n;i++)
	    if(g[i]>=ans)ans=g[i],pl=i;
	printf("%lld\n",pl);
	return 0;
}
```

例题 $8$ ：

[P3047 \[USACO12FEB\] Nearby Cows G](https://www.luogu.com.cn/problem/P3047)

由于 $k$ 很小，所以考虑把 $k$ 设进状态。设状态 $f[x][i]$ 表示点 $x$ 的子树内到 $x$ 距离为 $i$ 的点的权值和，易得如下转移方程：

$$
f[x][i]=\sum_{y\in son(x)}f[y][i-1] 
$$

因为儿子节点 $y$ 子树内到 $x$ 距离为 $i$ 的点，到儿子节点 $y$ 的距离为 $i-1$，即 $f[y][i-1]$，直接累加即可。初始时，$f[x][0]=a[x]$。

设状态 $g[x][i]$ 表示所有到 $x$ 距离为 $i$ 的点的权值和，同例题 $7$ 得 $g[rt][i]=f[rt][i]$。进行换根，有如下转移方程：

$$
g[x][i]=g[x][i]+g[y][i-1]-f[x][i-2](x\in son(y)) 
$$

$g[x][i]$ 的初值为 $f[x][i]$，因为所有到 $x$ 距离为 $i$ 的点的权值和包含点 $x$ 的子树内到 $x$ 距离为 $i$ 的点的权值和。点 $x$ 的子树外到点 $x$ 的父亲 $y$ 距离为 $i-1$ 的点，即 $g[y][i-1]$，到 $x$ 距离为 $i$，累加即可。但是这样就会误算 $x$ 子树内的点，这样的点不满足要求。所以，我们要减去子树内距离 $x$ 距离为 $i-2$ 的点，即 $f[x][i-2]$，因为这些点距离 $x$ 的父亲 $y$ 为 $i-1$，刚好是被误算的那些点。

对于每个节点，求出 $\sum_{i=0}^kg[x][i]$ 输出即可。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	long long v,nxt;
}e[400000];
long long n,k,u,v,a[400000],f[400000][21],g[400000][21],h[400000],cnt=0;
void add_edge(long long u,long long v)
{
	e[++cnt].nxt=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void dfs1(long long x,long long fa)
{
	f[x][0]=a[x];
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=fa)
	       {
	       	dfs1(e[i].v,x);
	       	for(int j=1;j<=k;j++)f[x][j]+=f[e[i].v][j-1];
		   }
}

void dfs2(long long x,long long fa)
{
	for(int i=0;i<=k;i++)g[x][i]+=f[x][i];
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=fa)
	       {
	        g[e[i].v][1]+=g[x][0];
	       	for(int j=2;j<=k;j++)g[e[i].v][j]+=(g[x][j-1]-f[e[i].v][j-2]);
	       	dfs2(e[i].v,x);
		   }
}

int main()
{
	scanf("%lld%lld",&n,&k);
	for(int i=1;i<=n-1;i++)
	    {
	    	scanf("%lld%lld",&u,&v);
	    	add_edge(u,v),add_edge(v,u);
		}
	for(int i=1;i<=n;i++)scanf("%lld",&a[i]);
	dfs1(1,0);
	dfs2(1,0);
	for(int i=1;i<=n;i++)
	    {
	    	long long sum=0;
	    	for(int j=0;j<=k;j++)sum+=g[i][j];
	    	printf("%lld\n",sum);
	    }
	return 0;
}
```

例题 $9$ ：

[CF1324F Maximum White Subtree](https://www.luogu.com.cn/problem/CF1324F)

记黑点为 $-1$，白点为 $1$，$cnt_1-cnt_2$ 等价于连通块内权值之和。

设状态 $f[x]$ 表示点 $x$ 的子树内的连通块可以取到的最大权值和。易得如下转移方程：

$$
f[x]=\sum_{y\in son(x)}\max(f[y],0) 
$$

对于每一个子树的连通块，可以取或者不取。如果取，就是 $f[y]$，如果不取，就是 $0$。两者取最大值即可。$f[x]$ 初始值为 $a[x]$。

设状态 $g[x]$ 表示包含点 $x$ 的连通块可以取到的最大权值和，同例题 $7$ 得 $g[rt]=f[rt]$。进行换根，有如下转移方程：

$$
g[x]=\max(g[y],f[x])(x\in son(y),f[x]\ge0) 
$$

因为 $f[x]\ge0$，所以在 $g[y]$ 的连通块中肯定包含了 $f[x]$ 对应的选法，也就是包含 $x$，可以直接取用 $g[y]$ 这个连通块。当然，也可以不取用这个连通块，那相当于只在子树内选点，就是 $f[x]$。取最大即可。

$$
g[x]=\max(g[y]+f[x],f[x])(x\in son(y),f[x]\lt0) 
$$

因为 $f[x]\lt0$，所以在 $g[y]$ 的连通块中肯定不包含 $f[x]$ 对应的选法，也就是不包含 $x$，要想取用，就必须增加 $f[x]$ 来包含点 $x$。当然，也可以不取用这个连通块，那相当于只在子树内选点，就是 $f[x]$。取最大即可。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	long long v,nxt;
}e[400000];
long long n,k,u,v,a[400000],f[400000],g[400000],h[400000],cnt=0;
void add_edge(long long u,long long v)
{
	e[++cnt].nxt=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void dfs1(long long x,long long fa)
{
	f[x]=a[x];
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=fa)
	       {
	       	dfs1(e[i].v,x);
	       	f[x]+=max(f[e[i].v],0ll);
		   }
}

void dfs2(long long x,long long fa)
{
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=fa)
	       {
	        if(f[e[i].v]>=0)g[e[i].v]=max(f[e[i].v],g[x]);
	        else g[e[i].v]=max(g[x]+f[e[i].v],f[e[i].v]);
	       	dfs2(e[i].v,x);
		   }
}

int main()
{
	scanf("%lld",&n);
	for(int i=1;i<=n;i++)
	    {
	    scanf("%lld",&a[i]);
	    if(a[i]==0)a[i]=-1; 
	    }
	for(int i=1;i<=n-1;i++)
	    {
	    	scanf("%lld%lld",&u,&v);
	    	add_edge(u,v),add_edge(v,u);
		}
	dfs1(1,0);
	g[1]=f[1];
	dfs2(1,0);
	for(int i=1;i<=n;i++)printf("%lld ",g[i]);
	return 0;
}
```

例题 $10$ ：

[CF708C Centroids](https://www.luogu.com.cn/problem/CF708C)

分析性质，我们发现，对于每个节点，想要改造成为重心，我们需要知道其子节点子树大小大于 $\lfloor\frac{n}{2}\rfloor$ 的子树内最大可以删去的子树大小。由于删掉之后还要连回来，所以删掉的子树大小不能超过 $\lfloor\frac{n}{2}\rfloor$。

以 $1$ 为根，记 $siz[x]$ 为子树 $x$ 的节点数。我们发现这个是可以动态规划的，设状态 $f[x]$ 表示子树 $x$ 内可以删去的不超过 $\lfloor\frac{n}{2}\rfloor$ 的最大子树大小。有如下转移方程：

$$
f[x]=\max(siz[y],f[x])(y\in son(x),siz[y]\le\lfloor\frac{n}{2}\rfloor) 
$$

$$
f[x]=\max(f[y],f[x])(y\in son(x),siz[y]\gt\lfloor\frac{n}{2}\rfloor) 
$$

第一个式子表示子树 $y$ 大小不超过 $\lfloor\frac{n}{2}\rfloor$，可以全部删去。第二个式子表示子树 $y$ 大小超过 $\lfloor\frac{n}{2}\rfloor$，不可以直接删去，但是可以删去这个子树中可以删去的最大值。根据 $f$ 数组的定义，$f[y]$ 一定不超过 $\lfloor\frac{n}{2}\rfloor$。

接下来，考虑子树外的贡献。设 $g[x]$ 表示子树 $x$ 外可以删去的不超过 $\lfloor\frac{n}{2}\rfloor$ 的最大子树大小。

考虑进行转移。如果 $y\to x$，那么 $x$ 子树外可以删去的不超过 $\lfloor\frac{n}{2}\rfloor$ 的最大子树可以是 $f[y]$，因为 $y$ 以及其子树现在在点 $x$ 外。但是如果 $f[y]$ 是由 $f[x]$ 转移过来的，就只能取 $f[y]$ 转移时的次大值。因此，对于每一个 $f[y]$，我们还需要记录次大值 $f[y][1]$，最大值记为 $f[y][0]$。

如果 $f[y]$ 是由 $f[x]$ 转移来的，则有 $g[x]=f[x][1]$，否则 $g[x]=f[x][0]$，这是子树 $y$ 之内的转移。另外，我们还需要考虑子树 $y$ 之外的转移。所以如果 $n-siz[y]\le\lfloor\frac{n}{2}\rfloor$，则 $g[x]=\max(n-siz[y],g[x])$，否则 $g[x]=\max(g[y],g[x])$。

最后，直接计算是否可以。对于点 $x$，如果最大的子树 $y$ 大小超过了 $\lfloor\frac{n}{2}\rfloor$，那么比较 $siz[y]-f[y][0]$ 和 $\lfloor\frac{n}{2}\rfloor$ 的值。如果子树 $x$ 外的子树大小超过了 $\lfloor\frac{n}{2}\rfloor$，那么比较 $n-siz[x]-g[x]$ 和 $\lfloor\frac{n}{2}\rfloor$ 的大小。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	long long v,nxt;
}e[800000];
long long n,k,u,v,siz[800000],mx[800000],b[800000],p[800000],f[800000][2],g[800000],h[800000],cnt=0;
void add_edge(long long u,long long v)
{
	e[++cnt].nxt=h[u];
	e[cnt].v=v;
	h[u]=cnt;
}

void dfs1(long long x,long long fa)
{
	siz[x]=1,p[x]=fa;
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=fa)
	       {
	       	dfs1(e[i].v,x);
            siz[x]+=siz[e[i].v];
            if(siz[e[i].v]>siz[mx[x]])mx[x]=e[i].v;
            long long v=0;
            if(siz[e[i].v]<=n/2)v=siz[e[i].v];
            else v=f[e[i].v][0];
	       	if(v>=f[x][0])f[x][1]=f[x][0],f[x][0]=v,b[x]=e[i].v;
            else if(v>=f[x][1])f[x][1]=v;
		   }
}

void dfs2(long long x,long long fa)
{
	for(int i=h[x];i;i=e[i].nxt)
	    if(e[i].v!=fa)
	       {
	        long long v;
            if(n-siz[x]>n/2)v=g[x];
            else v=n-siz[x];
            g[e[i].v]=max(g[e[i].v],v);
            if(e[i].v!=b[x])g[e[i].v]=max(g[e[i].v],f[x][0]);
            else g[e[i].v]=max(g[e[i].v],f[x][1]);
            dfs2(e[i].v,x);
		   }
}

long long check(long long x)
{
    if(siz[mx[x]]>n/2)return siz[mx[x]]-f[mx[x]][0]<=n/2;
    if(n-siz[x]>n/2)return n-siz[x]-g[x]<=n/2;
    return 1;
}

int main()
{
	scanf("%lld",&n);
	for(int i=1;i<=n-1;i++)
	    {
	    	scanf("%lld%lld",&u,&v);
	    	add_edge(u,v),add_edge(v,u);
		}
	dfs1(1,0);
    g[1]=f[1][0];
	dfs2(1,0);
	for(int i=1;i<=n;i++)printf("%lld ",check(i));
	return 0;
}
```

### 后记

忽然发现这次例题的设置很像数学试卷，绿题(基础题)占 $70\%$，蓝题(中档题)占 $20\%$，紫题(拔高题)占 $10\%$。

不过其实树形 DP 类题目理解起来并不难，还挺容易的。