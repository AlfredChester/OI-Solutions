# 浅谈LGV引理

### $LGV$引理

定义$w(P)$为有向路径$P$上**所有边权的乘积**，并定义$f(a,b)$表示$a\rightarrow b$的**所有有向路径边权乘积之和**，即：

$$
f(a,b)=\sum_{P:a\rightarrow b}w(P) 
$$

列出一个矩阵：

$$
M=\begin{bmatrix} f(a_1,b_1)&f(a_1,b_2)&\cdots&f(a_1,b_n)\\ f(a_2,b_1)&f(a_2,b_2)&\cdots&f(a_2,b_n)\\ \vdots&\vdots&\ddots&\vdots\\ f(a_n,b_1)&f(a_n,b_2)&\cdots&f(a_n,b_n) \end{bmatrix} 
$$

对于$A\rightarrow B$的一个**不交路径**集合$P_{1\sim n}$，其中$P_i$表示从$a_i$到$b_{\sigma(P)_i}$的一条路径（$\sigma(P)$是$P$对应的一个排列）。

然后对一个排列$\sigma$定义$sgn(\sigma)$，当$\sigma$中逆序对个数为偶数时$sgn(\sigma)=1$，为奇数时$sgn(\sigma)=-1$。

则满足：

$$
\det(M)=\sum_{P:A\rightarrow B}sgn(\sigma(P))\prod_{i=1}^nw(P_i) 
$$

### 路径不交问题

一种比较常见的题型，是求**从每个$a_i$到$b_i$，在满足路径不交的前提下，所有方案中路径边权乘积之和**。

则根据行列式的计算式发现：

$$
\det(M)=\sum_{\sigma}sgn(\sigma)\prod_{i=1}^nf(a_i,b_{\sigma(i)}) 
$$

考虑如果两个人的路径相交了，我们可以将相交后的部分取反，就相当于这两人分别走到了对方的终点去。

因此，我们可以把这当作一个容斥问题看待，即枚举一个排列$\sigma$表示每个人走到了谁的终点去。

然后发现容斥系数是$sgn(\sigma)$，也就是说这个$\det(M)$其实就是答案。

### [模板题](https://www.luogu.com.cn/problem/P6657)解法

-   给定一张$n\times m$的棋盘，每次只能向右或向下走一步。
-   有$m$个棋子，分别要从$(a_i,1)$走到$(b_i,n)$，求路径不交的方案数。
-   $n\le10^6,m\le100$

超级简化版。

相当于每条路径边权都是$1$，那么$f(a_i,b_j)$实际上就是从$(a_i,1)$走到$(b_j,n)$的方案数。

这应该算是个经典组合数问题，显然就是$C_{n-1+b_j-a_i}^{n-1}$。

### 代码：$O(n+m^3)$

```cpp
#include<bits/stdc++.h>
#define Tp template<typename Ty>
#define Ts template<typename Ty,typename... Ar>
#define Reg register
#define RI Reg int
#define Con const
#define CI Con int&
#define I inline
#define W while
#define N 1000000
#define M 100
#define X 998244353
#define C(x,y) (1LL*Fac[x]*IFac[y]%X*IFac[(x)-(y)]%X)//组合数
using namespace std;
int n,m,a[M+5],b[M+5];I int QP(RI x,RI y) {RI t=1;W(y) y&1&&(t=1LL*t*x%X),x=1LL*x*x%X,y>>=1;return t;}
int Fac[2*N+5],IFac[2*N+5];I void InitFac(CI S)//预处理阶乘和阶乘逆元
{
	RI i;for(Fac[0]=i=1;i<=S;++i) Fac[i]=1LL*Fac[i-1]*i%X;
	for(IFac[i=S]=QP(Fac[S],X-2);i;--i) IFac[i-1]=1LL*IFac[i]*i%X;
}
namespace G//高斯消元
{
	int a[M+5][M+5];I int Det(CI n)//求解行列式
	{
		RI i,j,k,t,s=1;for(i=1;i<=n;s=1LL*s*a[i][i]%X,++i)
		{
			if(!a[i][i]) {for(s=X-s,j=i+1;j<=n&&!a[j][i];++j);for(k=i;k<=n;++k) swap(a[i][k],a[j][k]);}
			for(j=i+1;j<=n;++j) for(t=1LL*(X-a[j][i])*QP(a[i][i],X-2)%X,k=i;k<=n;++k) a[j][k]=(a[j][k]+1LL*t*a[i][k])%X;
		}return s;
	}
}
int main()
{
	RI Tt,i,j;InitFac(N<<1),scanf("%d",&Tt);W(Tt--)
	{
		for(scanf("%d%d",&n,&m),i=1;i<=m;++i) scanf("%d%d",a+i,b+i);
		for(i=1;i<=m;++i) for(j=1;j<=m;++j) G::a[i][j]=a[i]<=b[j]?C(n-1+b[j]-a[i],n-1):0;//组合数计算路径数
		printf("%d\n",G::Det(m));
	}return 0;
}
```