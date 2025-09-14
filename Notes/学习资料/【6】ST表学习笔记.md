# 【6】ST表学习笔记

## 前言

学习ST表，主要是倍增思想，可以理解为倍增优化后的DP。写在这里，一方面方便自己以后复习，另一方面给其他人参考。

UPD on 2023/3/21 ：修改了格式，使格式与其他的学习笔记统一。

## 倍增

[倍增引入](https://blog.csdn.net/jarjingx/article/details/8180560)

与其说倍增是一种算法，不如说倍增是一种思想。

倍增的时间复杂度和二分是一样的，都是 $O(\log n)$ 。唯一的区别是倍增与二分的方向是相反的。倍增思想常用来**优化算法的时空复杂度**（ $O(n)\to O(\log n)$ ），和其他算法搭配使用。

我们进行递推时，如果状态空间很大，通常线性递推无法满足时空复杂度的要求，那可以通过成倍增加，**以 $2$ 的整数次幂为代表**。如 $13$，可以表示为

$$
13=2^3+2^2+2^0 
$$

对于 $2$ 的 $n$ 次幂，可以通过**位运算**快速求出。

```cpp
i=(1<<(n-1))
```

注意：由于位运算优先级分布不均匀，使用时一般**搭配括号**。

倍增的经典应用：快速幂，ST表，LCA，后缀数组。

## ST表

ST表可以用来解决RMQ（区间最值询问）问题，是一个**离线算法**。这里求解的是区间最小值问题。当询问数量较多，达到 $10^6$ 甚至更多且没有修改操作时，考虑使用**ST表**。

### 主要思想

#### 预处理

设状态 $dp[i][j]$ 表示区间 $[i,i+2^j-1]$ 的最小值。

易得 $dp[i][0]=a[i]$

状态转移方程如下：

$$
dp[i][j]=\min\{dp[i][j-1],dp[i+2^{j-1}][j-1]\} 
$$

由状态定义得，其中 $dp[i][j-1]$ 表示区间 $[i,i+2^{j-1}-1]$ 的最小值， $dp[i+2^{j-1}][j-1]$ 表示下面区间的最小值：

$$
[i+2^{j-1},i+2^{j-1}+2^{j-1}-1]=[i+2^{j-1},i+2^j-1] 
$$

由于相交或相接的区间可以合并，故把 $dp[i][j]$ 分为两段，分别求出最小值，再取最小值合并。

注意：由于 $dp[i][j]$ 是由 $dp[i][j-1]$ 和 $dp[i+2^{j-1}][j-1]$ 转移来的，所以**对于变量 $j$ 的循环要在外层**。

预处理完整代码如下：

```cpp
for(int i=1;i<=n;i++)
	f[i][0]=a[i];
for(int j=1;j<=MAXM;j++)
	for(int i=1;i+(1<<(j-1))<=n;i++)
		f[i][j]=max(f[i][j-1],f[i+(1<<(j-1))][j-1]);
```

时间复杂度：$O(n\log n)$

#### 询问

对于询问区间 $[s,t]$ ，首先取

$$
k=(int)log2(t-s+1) 
$$

然后可得

$$
RMQ[s,t]=\min\{dp[s][k],dp[t-2^k+1][k]\} 
$$

由状态定义得，其中 $dp[s][k]$ 表示区间 $[s,s+2^k-1]$ 的最小值， $dp[t-2^k+1][k]$ 表示下面区间的最小值：

$$
[t-2^k+1,t-2^k+1+2^k-1]=[t-2^k+1,t] 
$$

很显然，这两个区间是相交或相接的，故可以合并最小值，求出答案。

回答询问完整代码如下：

```cpp
int k=(int)log2(t-s+1);
printf("%d\n",max(f[s][k],f[t-(1<<k)+1][k]));
```

时间复杂度：$O(1)$

### ST表例题

例题 $1$ ：

[P3865 【模板】ST 表](https://www.luogu.com.cn/problem/P3865)

ST表模板题，不多赘述。

```cpp
#include <bits/stdc++.h>
#define MAX 600000
using namespace std;
int n,m,s,t,a[2000000],f[100050][21];
inline int read()
{
	int x=0,f=1;char ch=getchar();
	while (ch<'0'||ch>'9'){if (ch=='-') f=-1;ch=getchar();}
	while (ch>='0'&&ch<='9'){x=x*10+ch-48;ch=getchar();}
	return x*f;
}

int main()
{
	n=read();m=read();
	for(int i=1;i<=n;i++)
	    a[i]=read();
	for(int i=1;i<=n;i++)
	    f[i][0]=a[i];
	for(int j=1;j<=20;j++)
		for(int i=1;i+(1<<(j-1))<=n;i++)
		        f[i][j]=max(f[i][j-1],f[i+(1<<(j-1))][j-1]);
	for(int i=1;i<=m;i++)
	    {
	    	s=read();t=read();
	    	int k=(int)log2(t-s+1);
	    	printf("%d\n",max(f[s][k],f[t-(1<<k)+1][k]));
		}
	return 0;
}
```

例题 $2$ ：

[P2880 \[USACO07JAN\] Balanced Lineup G](https://www.luogu.com.cn/problem/P2880)

很显然，只需要改变 $\min$ 和 $\max$ ，就可以从求最小值转化为求最大值。

$$
dp[i][j]=\max\{dp[i][j-1],dp[i+2^{j-1}][j-1]\} 
$$

$$
k=(int)log2(t-s+1) 
$$

$$
RMQ[s,t]=\max\{dp[s][k],dp[t-2^k+1][k]\} 
$$

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m,s,t,a[2000000],f[100050][21],f2[100050][21];
inline int read()
{
    int x=0,f=1;char ch=getchar();
    while (ch<'0'||ch>'9'){if (ch=='-') f=-1;ch=getchar();}
    while (ch>='0'&&ch<='9'){x=x*10+ch-48;ch=getchar();}
    return x*f;
}
  
int main()
{
    n=read();m=read();
    for(int i=1;i<=n;i++)
        a[i]=read();
    for(int i=1;i<=n;i++)
        f[i][0]=a[i],f2[i][0]=a[i];
    for(int j=1;j<=20;j++)
        for(int i=1;i+(1<<(j-1))<=n;i++)
                {
                f[i][j]=max(f[i][j-1],f[i+(1<<(j-1))][j-1]);
                f2[i][j]=min(f2[i][j-1],f2[i+(1<<(j-1))][j-1]);
                }
    for(int i=1;i<=m;i++)
        {
            s=read();t=read();
            int k=(int)log2(t-s+1);
            printf("%d\n",max(f[s][k],f[t-(1<<k)+1][k])-min(f2[s][k],f2[t-(1<<k)+1][k]));
        }
    return 0;
}
```

## 后记

RMQ问题还可以用线段树来解决，这里不多赘述。

教练推荐的一篇博客：

[倍增与ST算法 --算法竞赛专题解析（28）](https://blog.csdn.net/weixin_43914593/article/details/109500135?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_paycolumn_v3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_paycolumn_v3&utm_relevant_index=2)