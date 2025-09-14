# 【7】状态压缩DP学习笔记

### 前言

状态压缩 DP 是一类常用的 DP 方式，思维难度不是很大，但需要一点卡常和实现技巧。比较容易掌握，是一个骗分的好东西。

### 状态压缩DP

状态压缩 DP 通常用来处理 DP 过程中与**具体状态**相关的问题。这种情况下，我们通过**二进制压缩**使**状态**可以成为 DP 的一个维度，从而进行动态规划。

因此，状态压缩 DP 的时间复杂度大体为 $O(2^n)$。故状态压缩 DP 数据范围一般**不大**，$n\le25$ 左右。

一些具体的压缩方式，可以参见例题。

[位运算基础知识](https://blog.csdn.net/china_chk/article/details/137394881)

### 例题

例题 $1$ ：

[P1879 \[USACO06NOV\] Corn Fields G](https://www.luogu.com.cn/problem/P1879)

由于数据范围很小，考虑状态压缩 DP。不难发现，在当前这一行某一个位置是否可以种草，取决于这块地是否适合种草以及上一行这个位置是否种草。而当前这一行某一个位置是否可以种草可以直接得出，而上一行这个位置是否种草是转移时需要的信息。因此，我们把上一行每个位置是否种草压缩成状态，成为 DP 数组的维度。

具体的，设状态 $f[x][y]$ 表示当前处于第 $x$ 行，上一行状态为 $y$ 的方案数。若 $y$ 的第 $i$ 位为 $0$，则上一行第 $i$ 个位置没有种草；若 $y$ 的第 $i$ 位为 $1$，则上一行第 $i$ 个位置种了草。

考虑枚举这一行的状态 $z$，$z$ 的定义与 $y$ 相同。首先，判定 $z$ 中所有 $1$ 的位置是否能种草以及是否有 $1$ 相邻。然后，判定 $z$ 中所有 $1$ 的位置是否在 $y$ 中也为 $1$。如果都为 $1$，根据状态定义，我们发现出现了有公共边的两块草地，不满足题意。

最后，对于每一个合法状态 $z$，显然有如下转移方程：

$$
f[x+1][z]=f[x+1][z]+f[x][y] 
$$

最后的答案为 $\sum_{i=0}^{2^n-1}f[m+1][i]$，时间复杂度为 $O(m2^n)$。代码中使用了记忆化搜索的实现方法。

```cpp
#include <bits/stdc++.h>
using namespace std;
int m,n,map1[20][20],f[20][5000],mod=100000000;
int dfs(int now,int pre)
{
    int ans=0;
    if(f[now][pre])return f[now][pre];
	if(now==m+1)
	   {
	   	ans=(ans+1)%mod;
	   	return ans;
	   }
	int t=(1<<n);
	for(int i=0;i<t;i++)
	    {
	    int flag=1;
	    for(int j=0;j<n;j++)
		    if(((i>>j)&1)&&((pre>>j)&1||(i>>(j-1)&1)||(i>>(j+1)&1)||(!map1[now][j+1])))flag=0;
		if(flag)ans=(ans+dfs(now+1,i))%mod;
	    }
	f[now][pre]=ans;
	return ans;
}

int main()
{
	scanf("%d%d",&m,&n);
	for(int i=1;i<=m;i++)
	   for(int j=1;j<=n;j++)
	      scanf("%d",&map1[i][j]);
	printf("%d\n",dfs(1,0));
	return 0;
}
```

例题 $2$ ：

[P2704 \[NOI2001\] 炮兵阵地](https://www.luogu.com.cn/problem/P2704)

与上一道题目类似，但是需要状态压缩上两行的数据。判断转移也与上面基本一样，但需要判定两行。设状态 $f[x][y][z]$ 表示目前是第 $x$ 行，上一行状态为 $y$，上两行状态为 $z$，若可以由状态 $f[x-1][z][w]$，则有转移方程：(令 $p[x]$ 为状态 $x$ 的二进制位为 $1$ 的位置数量)

$$
f[x][y][z]=\max(f[x][y][z],f[x-1][z][w]+p[y]) 
$$

使用滚动数组优化空间即可通过。

```cpp
#include <bits/stdc++.h>
using namespace std;
int m,n,f[2][3000][3000],p[3000],c[200],ans=0,now=0;
char map1[200][200];
int lowbit(int x)
{
	return x&(-x);
}

int popcnt(int x)
{
	int ans=0;
	while(x>0)ans++,x-=lowbit(x);
	return ans;
}

int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++)
	   {
	   scanf("%s",map1[i]);
	   for(int j=0;j<m;j++)
	       if(map1[i][j]=='P')c[i]+=(1<<j);
       }
    for(int i=1;i<=(1<<m)-1;i++)p[i]=popcnt(i);
	for(int i=0;i<=(1<<m)-1;i++)
	    for(int j=0;j<=(1<<m)-1;j++)
	        f[now][i][j]=-1e5;
	f[now][0][0]=0;
	for(int i=0;i<n;i++)
	    {
	    for(int j=0;j<=(1<<m)-1;j++)
	        for(int k=0;k<=(1<<m)-1;k++)
		        f[now^1][j][k]=-1e5;
	    for(int j=0;j<=(1<<m)-1;j++)
	        for(int k=0;k<=(1<<m)-1;k++)
	            {
	            if((j&(j<<1))||(j&(j<<2))||(k&(k<<1))||(k&(k<<2))||((c[i]&j)!=j))continue;
	            for(int l=0;l<=(1<<m)-1;l++)
	                {
	                	if((l&(l<<1))||(l&(l<<2))||(j&l)||(k&l)||((c[i+1]&l)!=l))continue;
	                	f[now^1][l][j]=max(f[now^1][l][j],f[now][j][k]+p[l]);
					}
				}
	    now^=1;
	    }
	for(int i=0;i<=(1<<m)-1;i++)
	    for(int j=0;j<=(1<<m)-1;j++)
	        ans=max(ans,f[now][i][j]);
	printf("%d\n",ans);
	return 0;
}
```

例题 $3$ ：

[P3959 \[NOIP2017 提高组\] 宝藏](https://www.luogu.com.cn/problem/P3959)

不难发现最后的路径一定会构成一棵树。由于每条路的贡献与与其深度有关，故考虑设计与深度有关的状态。由于数据范围很小，考虑状态压缩 DP，每次扩展一层。

设状态 $f[i][j]$ 表示目前扩展到第 $i$ 层，所有节点的状态为 $j$。若第 $k$ 位为 $1$，则表示节点 $k$ 已经被打通；若第 $k$ 位为 $0$，则表示节点 $k$ 未被打通。记状态 $k$ 转移到状态 $j$ 经过的边权之和为 $cost(k,j)$，不难得到如下转移方程：

$$
f[i][j]=\min(f[i][j],f[i-1][k]+cost(k,j)\times(i-1)) 
$$

接下来，考虑预处理出两个数组 $cost(k,j)$ 与 $pos(k,j)$，表示状态 $k$ 转移到状态 $j$ 经过的边权之和为 $cost(k,j)$ 与可行性。再预处理一个数组 $ex(i)$，表示状态 $i$ 可以扩展的所有点(包括原有点)的状态集合。若第 $k$ 位为 $1$，则表示节点 $k$ 可以被扩展；若第 $k$ 位为 $0$，则表示节点 $k$ 不可以被扩展。

$ex(i)$ 并不难预处理，只需要枚举已有节点的每一条边，能扩展到的标记为 $1$ 即可。有了 $ex(i)$ 后，我们发现 $pos(k,j)$ 也不难预处理。当且仅当 $k$ 是 $j$ 的子集且 $j$ 是 $ex(k)$ 的子集时，$pos(k,j)=1$。判断子集可以用位运算来实现，若 $j\&k=k$，则 $k$ 是 $j$ 的子集。

接下来，考虑求出满足 $pos(k,j)$ 的 $cost(k,j)$。考虑枚举状态 $j$ 中未扩展的点，枚举从 $k$ 中的点到这一个点的边，取边权最小值为贡献。最后，将每一个未扩展的点的贡献加和，即为 $cost(k,j)$。预处理之后，转移也比较显然。

时间复杂度为 $O(m2^{n}+n3^n)$，其中 $3^n$ 是枚举子集的时间复杂度。

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m,u,v,d,h[30000],ex[5000],cst[5000][5000],f[20][5000],ans=1e9;
vector<int>to[20],dis[20];
bool pos[5000][5000];
int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%d%d%d",&u,&v,&d);
	    	to[u].push_back(v),dis[u].push_back(d);
	    	to[v].push_back(u),dis[v].push_back(d);
		}
	for(int i=0;i<=(1<<n)-1;i++)
	    {
	    	ex[i]=i;
	    	for(int j=0;j<n;j++)
	    	    if(i&(1<<j))
	    	       {
	    	       int s=to[j+1].size();
	    	       for(int k=0;k<s;k++)ex[i]|=(1<<(to[j+1][k]-1));
	    	       }
		}
	for(int i=0;i<=(1<<n)-1;i++)
	    for(int j=0;j<=(1<<n)-1;j++)
	        if((j|ex[i])==ex[i]&&(i|j)==j&&i!=j)
	           {
	           	pos[i][j]=1;
	           	for(int k=0;k<n;k++)
	           	    if(!(i&(1<<k))&&(j&(1<<k)))
		           	    {
		    	        int mi=1e9,s=to[k+1].size();
		    	        for(int l=0;l<s;l++)
						    if((i&(1<<(to[k+1][l]-1))))mi=min(mi,dis[k+1][l]);
						cst[i][j]+=mi;
						}
			   }
	for(int i=0;i<=n;i++)
	    for(int j=0;j<=(1<<n)-1;j++)
	        f[i][j]=1e9;
	for(int i=1;i<=n;i++)
	    f[0][(1<<(i-1))]=0;
	for(int i=1;i<=n;i++)
	    for(int j=0;j<=(1<<n)-1;j++)
	        for(int k=0;k<=(1<<n)-1;k++)
	            if(pos[k][j]&&f[i-1][k]!=1e9)f[i][j]=min(f[i][j],f[i-1][k]+cst[k][j]*i);
	for(int i=0;i<=n;i++)ans=min(ans,f[i][(1<<n)-1]);
	printf("%d\n",ans);
	return 0;
}
```

### 后记

> 蒹葭苍苍，白露为霜。所谓伊人，在水一方。溯洄从之，道阻且长。溯游从之，宛在水中央。