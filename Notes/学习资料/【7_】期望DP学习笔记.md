# 【7*】期望DP学习笔记

### 前言

由于马上就要把同学的《概率论与数理统计》还回去了，所以赶快看一点，并做一点笔记。

感觉网上很多文章讲期望 DP 都讲得不够透彻啊，就写一些自己的理解造福后人吧，自我感觉讲得很透彻。

**此类知识点大纲中并未直接涉及，所以【7】是我自己的估计，后带星号表示估计，仅供参考。**

### 基础定义

在概率论中，一个**随机试验**具有如下三个特征。

$1$：可以在**相同**的条件下**重复**地进行。

$2$：每一次试验的结果**不止一个**。并且能实现**验明**试验**所有**可能的结果。

$3$：进行试验前**不能确定**哪一个结果会出现。

我们把一个随机试验 $E$ 的**所有**可能的结果组成的**集合**称为随机试验 $E$ 的**样本空间**，记作 $S$。样本空间的每一个**元素**，称为**样本点**。

一般的，我们称随机试验 $E$ 的样本空间 $S$ 的**子集**称为 $E$ 的**随机事件**，简称**事件**。特别的，对于只有**一个**样本点的事件，称为基本事件。

事件运算即为基本的集合运算，特别的，对于两个事件 $A$ 和 $B$，我们把 $A\cap B$ 简记为 $AB$，其意义为 $A$ 和 $B$ 一起发生。

### 概率

**概率的基本定义**

设 $E$ 是随机试验，$S$ 是它的样本空间，对于 $E$ 的每一事件赋予一个实数 $P(A)$，称为 $A$ 的**概率**。这个函数满足如下条件。

$1$：$P(A)\ge 0$

$2$：$P(S)=1$

$3$：设 $A_1,A_2\dots$ 是一系列互不相容的事件，即对于 $A_iA_j=\varnothing,i\ne j,i,j\in\{1,2\dots\}$，有：

$$
P(A_1\cup A_2\cup\dots)=P(A_1)+P(A_2)+\dots 
$$

下面是一些常用的推论。

$1$：$P(\varnothing)=0,P(A)\le 1$

$2$：(**有限可加性**)设 $A_1,A_2\dots A_n$ 是一系列互不相容的事件，即对于 $A_iA_j=\varnothing,i\ne j,i,j\in\{1,2\dots n\}$，有：

$$
P(A_1\cup A_2\cup\dots\cup A_n)=P(A_1)+P(A_2)+\dots+P(A_n) 
$$

$3$：(**逆事件的概率**)$P(\bar A)=1-P(A)$

$4$：(**加法公式**)类似容斥原理。

$$
P(A_1\cup A_2\cup\dots\cup A_n)=\sum_{i=1}^nP(A_i)-\sum_{1\le i\lt j\le n}P(A_iA_j)+\sum_{1\le i\lt j\lt k\le n}P(A_iA_jA_k)+\dots+(-1)^{n-1}P(A_1A_2\dots A_n) 
$$

样本空间中具有**有限种**元素，且每一种事件发生**概率相同**的概率模型称为**古典概型**。在此概率模型下，可以得到概率的计算式。

$$
P(A)=\frac{\text{A包含的基本事件数}}{S中基本事件的总数} 
$$

**条件概率**

事件 $A$ **已发生**的情况下事件 $B$ 发生的概率，称为**条件概率**，记作 $P(B\mid A)$。

在古典概型下，设试验的基本事件总数为 $n$，$A$ 所包含的基本事件数为 $m(m\gt0)$，$AB$ 所包含的基本事件数为 $k(k\gt0)$，不难推出条件概率的计算公式。

$$
P(B\mid A)=\frac{k}{m}=\frac{\frac{k}{n}}{\frac{m}{n}}=\frac{P(AB)}{P(A)} 
$$

不难发现，$P(A)$ 其实等价于 $P(A\mid S)$。

注意到条件概率 $P(X\mid A)$ 满足概率的基本定义，其中 $X$ 为 $A$ 的任意子集。于是，概率 $P(X)$ 的推论对条件概率 $P(X\mid A)$ 也适用。以加法原理的特例举个例子。

$$
P(B_1\cup B_2\mid A)=P(B_1\mid A)+P(B_2\mid A)-P(B_1B_2\mid A) 
$$

逆用条件概率的计算式，可以得到概率论中的**乘法定理**。

$$
P(AB)=P(B\mid A)P(A) 
$$

这个式子很容易推广到多个事件的情况。

$$
P(ABC)=P(C\mid AB)P(B\mid A)P(A) 
$$

$$
P(A_1A_2\dots A_n)=P(A_n\mid A_1A_2\dots A_{n-1})=P(A_{n-1}\mid A_1A_2\dots A_{n-2})\dots P(A_2\mid A_1)P(A_1) 
$$

上面几个公式可以实现条件概率与与积事件概率之间的互化。

特别的，如果 $P(B\mid A)=P(B)$，这时乘法公式可以改写为如下形式。

$$
P(AB)=P(B\mid A)P(A)=P(B)P(A) 
$$

如果 $A$ 和 $B$ 满足上面式子，那么我们称 $A$ 与 $B$ 是**独立**的。这其实就是我们平常认为的概率计算公式。

**划分**

设 $S$ 为试验 $E$ 的样本空间，$B_1,B_2\dots B_n$ 为 $E$ 的一组事件。若 $B_1,B_2\dots B_n$ 满足如下限制，则称 $B_1,B_2\dots B_n$ 是 $S$ 的一组**划分**。

$1$：$B_iB_j=\varnothing,i\ne j,i,j\in\{1,2,\dots,n\}$

$2$：$B_1\cup B_2\cup\dots\cup B_n=S$

对于一个划分，我们有**全概率公式**。

$$
P(A)=P(A\mid B_1)P(B_1)+P(A\mid B_2)P(B_2)+\dots+P(A\mid B_n)P(B_n) 
$$

下面给出证明。

---

由划分的定义，不难得出如下式子。

$$
A=AS=A(B_1\cup B_2\cup\dots\cup B_n)=AB_1\cup AB_2\cup\dots\cup AB_n 
$$

套进有限可加性的式子，再使用乘法公式展开，就证完了。

$$
\begin{aligned} P(A)&=P(AB_1)+P(AB_2)+\dots+P(AB_n) \\ &=P(A\mid B_1)P(B_1)+P(A\mid B_2)P(B_2)+\dots+P(A\mid B_n)P(B_n) \end{aligned}
$$

---

事实上，在实际问题中，$P(A)$ 不能直接求得，但却能容易找到 $S$ 一个划分 $B_1,B_2\dots B_n$，使得 $P(B_i)$ 和 $P(A\mid B_i)$ 都容易求得。这时我们就可以通过全概率公式求出 $P(A)$。

特别的，如果一个划分 $n=2$，记 $B_1$ 为 $B$，则 $B_2$ 就是 $\bar B$，此时有全概率公式的特殊形式。

$$
P(A)=P(A\mid B)P(B)+P(A\mid\bar B)P(\bar B) 
$$

对于一个划分，我们还有一个重要的公式，**贝叶斯公式**。

$$
P(B_i\mid A)=\frac{P(A\mid B_i)P(B_i)}{\sum_{j=1}^nP(A\mid B_j)P(B_j)}(i\in1,2,\dots n) 
$$

下面给出证明。

---

我们使用条件概率的定义展开。

$$
P(B_i\mid A)=\frac{P(A\mid B_i)}{P(A)}(i\in1,2\dots n) 
$$

把上面使用乘法定理展开，下面用全概率公式展开，就得到了贝叶斯公式。

$$
P(B_i\mid A)=\frac{P(A\mid B_i)P(B_i)}{\sum_{j=1}^nP(A\mid B_j)P(B_j)}(i\in1,2,\dots n) 
$$

---

特别的，如果一个划分 $n=2$，记 $B_1$ 为 $B$，则 $B_2$ 就是 $\bar B$，此时也有贝叶斯公式的特殊形式。

$$
P(B\mid A)=\frac{P(A\mid B)P(B)}{P(A\mid B)P(B)+P(A\mid\bar B)P(\bar B)} 
$$

### 期望

对于一个随机试验 $E$，我们可以将样本空间 $S$ 中的每一个时间 $e$ 对应到一个实数 $X(e)$，我们称 $X=X(e)$ 为**随机变量**。

设离散型随机变量的分布为 $P(X=x_k)=p_k,k=1,2\dots$，我们将如下式子定义为随机变量 $X$ 的**期望**。

$$
\sum_{k=1}^{\infin}x_kp_k 
$$

实际上，如果上面是式子不是绝对收敛，这个式子不能被称为 $X$ 的期望。但是 OI 中的式子一般都是绝对收敛，所以我们不考虑这种情况。

现在，假设我们知道了 $P(X=x_k)=p_k,k=1,2\dots$ 的期望，我们想知道 $P(X'=x_k+1)=p_k,k=1,2\dots$ 的期望，我们考虑如何求解。

$$
\sum_{k=1}^{\infin}(x_k+1)p_k=\sum_{k=1}^{\infin}x_kp_k+\sum_{k=1}^{\infin}p_k=\sum_{k=1}^{\infin}x_kp_k+1 
$$

由此，我们引出**期望的线性性**。这对于期望 DP 来说是重要的基本功。现在，我们记 $E(X)$ 表示随机变量 $X$ 的期望，那么我们有如下式子。

$1$：$c$ 为常数，$E(c)=c$

$2$：$E(X+Y)=E(X)+E(Y)$

$3$：若 $X,Y$ 相互独立，$E(XY)=E(X)E(Y)$

$4$：$a,b$ 为常数，$E(aX+b)=E(aX)+b$

上述式子均只需要展开期望的定义即可证明，在此不做赘述。

在期望 DP 中，状态 $f[x]$ 表示状态 $x$ 下随机变量的期望。也就是说，在期望 DP 中，$f[x]$ 实际上是一个随机变量 $X$ 的期望。有了这一点认识，我们就可以利用期望的展开式和期望的线性性来推式子。

另外，期望 DP 一般使用**逆推**的形式，也就是记录 $f[x]$ 表示到终末状态的期望。因为如果顺推，从初始状态 $A$ 到达当前状态 $B$ 的概率并不是 $P(AB)$，而是 $P(B\mid A)$。而我们要求的期望是在 $P(AB)$ 意义下的。此时如果逆推，注意到终末状态 $A$ 的必然的，于是就有 $P(AB)=P(B\mid A)P(A)=P(B\mid A)$，不会产生矛盾。

因此，我们不难得知，如果终止状态确定(即所有终止状态都被包含在状态中)执行逆推，起始状态确定执行顺推。

期望 DP 有如下经典模型，这里以逆推为例。

$$
f[i]=\sum_{j\in\text{nxt}(i)}p(i,j)\times(f[j]+w(i,j)) 
$$

其中 $\text{nxt}(i)$ 表示 $i$ 的后继状态，$w(i,j)$ 表示从 $i$ 到 $j$ 对答案的贡献，$p(i,j)$ 表示从 $i$ 转移到 $j$ 的概率。

这个模型是容易证明的。我们只需要展开期望的定义式就能证明。

$$
\begin{aligned} f[i]&=\sum_{x}p_xx\\ &=\sum_{j\in\text{nxt}(i)}\sum_{x'}p'_{x'}\times p(i,j)\times (x'+w(i,j))\\ &=\sum_{j\in\text{nxt}(i)}p(i,j)(\sum_{x'} p'_{x'}x'+ \sum_{x'}p'_{x'}w(i,j))\\ &=\sum_{j\in\text{nxt}(i)}p(i,j)\times(f[j]+w(i,j)) \end{aligned}
$$

### 例题

例题 $1$ ：

[CF453A Little Pony and Expected Maximum](https://www.luogu.com.cn/problem/CF453A)

求期望，直接展开期望定义式。

$$
E(\max\{x_i\})=\sum_{k=1}^mP(\max\{x_i\}=k)k 
$$

考虑求 $P(\max\{x_i\}=k)$，使用前缀和技巧。

$$
\begin{aligned} P(\max\{x_i\}=k)&=P(\max\{x_i\}\le k)-P(\max\{x_i\}\le k-1) \\ &=(\frac{k}{m})^n-(\frac{k-1}{m})^n \end{aligned}
$$

带回原式，不难发现是一个类似分数裂项的形式，进一步展开得到最终的计算式。

$$
\begin{aligned} E(\max\{x_i\})&=\sum_{k=1}^m(\frac{k}{m})^nk-(\frac{k-1}{m})^nk \\ &=m-\sum_{k=1}^{m-1}(\frac{k}{m})^n \end{aligned}
$$

```cpp
#include <bits/stdc++.h>
using namespace std;
long long m,n;
long double power(long double a,long long p)
{
	long double x=a,ans=1;
	while(p)
	   {
	   	if(p&1)ans=ans*x;
	   	p>>=1;
	   	x=x*x;
	   }
	return ans;
}

int main()
{
	scanf("%lld%lld",&m,&n);
	long double ans=m;
	for(int i=1;i<=m-1;i++)ans-=power((long double)i/m,n);
	printf("%Lf\n",ans);
	return 0;
}
```

例题 $2$ ：

[P1297 \[国家集训队\] 单选错位](https://www.luogu.com.cn/problem/P1297)

方案的总数是容易计算的，为 $\prod_{i=1}^na_i$，那期望就是统计每一种方案的贡献和再除掉。

转化贡献体。考虑每一种方案显然不可做，考虑每一个位置会贡献 $i$ 多少次。首先位置 $i-1$ 和 $i$ 的答案必须相同，有 $\min(a_{i-1},a_i)$ 种选法。其余位置随便选，有 $\frac{\prod_{i=1}^na_i}{a_{i-1}a_i}$ 种选法。把除方案总数的 $\prod_{i=1}^na_i$ 除进来，得到每个位置的贡献为 $\frac{\min(a_{i-1},a_i)}{a_{i-1}a_i}$，累加即可。

注意题中是一个环，需要特判。

```cpp
#include <bits/stdc++.h>
using namespace std;
long long n,x,y,z,a[20000000];
double ans=0;
int main()
{
	scanf("%lld%lld%lld%lld%lld",&n,&x,&y,&z,&a[1]);
    for(int i=2;i<=n;i++)a[i]=(a[i-1]*x+y)%100000001;
    for(int i=1;i<=n;i++)a[i]=a[i]%z+1;
    for(int i=2;i<=n;i++)ans+=(double)min(a[i],a[i-1])/a[i]/a[i-1];
    ans+=(double)min(a[1],a[n])/a[1]/a[n];
    printf("%.3lf",ans);
	return 0;
}
```

例题 $3$ ：

[P4316 绿豆蛙的归宿](https://www.luogu.com.cn/problem/P4316)

由于终点状态是确定的，考虑设计状态 $f[i]$ 表示从 $i$ 走到 $n$ 期望需要多少步。记 $d[i]$ 为点 $i$ 的出度，套用经典模型得到转移式。其中 $\text{dis}$ 为边权。

$$
f[i]=\sum_{j\in\text{nxt}(i)}\frac{f[j]+\text{dis}}{d[i]} 
$$

拓扑排序直接转移即可。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct edge
{
	long long v,d,nxt;
}e[300000];
long long n,m,u,v,w,h[300000],d[300000],sd[300000],q[300000],l=1,r=0,cnt=0;
double f[300000];
void add_edge(long long u,long long v,long long w)
{
	e[++cnt].nxt=h[u];
	e[cnt].v=v;
	e[cnt].d=w;
	h[u]=cnt;
	d[v]++;
}

void topo_sort(long long s)
{
	l=1,r=0,q[++r]=s;
	while(l<=r)
	   {
	   	long long x=q[l];
	   	for(int i=h[x];i;i=e[i].nxt)
	   	    {
	   	    f[e[i].v]+=(f[x]+e[i].d)/sd[e[i].v],d[e[i].v]--;
	   	    if(d[e[i].v]==0)q[++r]=e[i].v;
			}
		l++;
	   }
}

int main()
{
	scanf("%lld%lld",&n,&m);
	for(int i=1;i<=m;i++)scanf("%lld%lld%lld",&u,&v,&w),sd[u]++,add_edge(v,u,w);
	topo_sort(n);
    printf("%.2lf",f[1]);
	return 0;
}
```

例题 $4$ ：

[AT\_abc263\_e \[ABC263E\] Sugoroku 3](https://www.luogu.com.cn/problem/AT_abc263_e)

记 $f[x]$ 表示从 $x$ 开始到 $n$ 的期望步数，先考虑不会跳到自己的情况。套用经典模型不难得到如下式子。

$$
f[x]=\frac{1}{a_i}\sum_{i=x+1}^{x+a_i}f[i]+1 
$$

现在我们考虑能转移到自己，还是一样计算。

$$
f[x]=\frac{1}{a_i+1}\sum_{i=x}^{x+a_i}f[i]+1 
$$

把 $f[x]$ 移到左边。

$$
\frac{a_i}{a_i+1}f[x]=\frac{1}{a_i+1}\sum_{i=x+1}^{x+a_i}f[i]+1 
$$

化系数为 $1$，就得到递推式。

$$
f[x]=\frac{\sum_{i=x+1}^{x+a_i}f[i]+a_i+1}{a_i} 
$$

后缀和优化一下，时间复杂度 $O(n\log V)$。

```cpp
#include <bits/stdc++.h>
using namespace std;
long long n,a[300000],f[300000],h[300000];
const long long mod=998244353;
long long power(long long a,long long p)
{
	long long x=a,ans=1;
	while(p)
	   {
	   	if(p&1)ans=ans*x%mod;
	   	p>>=1;
	   	x=x*x%mod;
	   }
	return ans;
}

int main()
{
	scanf("%lld",&n);
	for(int i=1;i<=n-1;i++)scanf("%lld",&a[i]);
	f[n]=h[n]=h[n+1]=0;
	for(int i=n-1;i>=1;i--)f[i]=(h[i+1]-h[min(n+1,i+a[i]+1)]+a[i]+1+mod)%mod*power(a[i],mod-2)%mod,h[i]=(h[i+1]+f[i])%mod;
	printf("%lld\n",f[1]);
	return 0;
}
```

例题 $5$ ：

[P1850 \[NOIP 2016 提高组\] 换教室](https://www.luogu.com.cn/problem/P1850)

如果知道这一次和下一次的教室的位置，为了最小化消耗的体力值，肯定会选择走最短路，使用 Floyd 预处理出任意两点之间的最短路。

由于起始状态必然是最开始两间教室之一，结束状态也必然是最后两间教室之一，都是确定的，所以可以随便选择 DP 顺序。我们发现知道相邻两次有没有都提出换教室就能得到相邻两次每种情况的概率，考虑把这个设进状态。

记状态 $f[i][j][0/1]$ 表示现在在考虑第 $i$ 间教室，已经提出了 $j$ 次申请，上一次有没有换教室。记 $\text{dist}(x,y)$ 为点 $x$ 和点 $y$ 之间的最短路。

先考虑 $f[i][j][0]$，这个状态有两个前驱状态，$f[i-1][j][0]$ 和 $f[i-1][j][1]$，我们显然会选择期望体力值少的转移。

$$
f[i][j][0]=\min(f[i-1][j][0]+\text{dist}(c_{i-1},c_i),f[i-1][j][1]+k_{i-1}\text{dist}(d_{i-1},c_i)+(1-k_{i-1})\text{dist}(c_{i-1},c_i)) 
$$

取 $\min$ 内第一项是两次都没有换教室，显然贡献为 $\text{dist}(c_{i-1},c_i)$。第二项把经典模型的公式展开了，$f[i-1][j][1]$ 的系数是所有概率累加为 $1$，后面是每一种情况的概率乘上贡献，即经典模型中的 $p(i,j)\times w(i,j)$。

类似的，我们写出 $f[i][j][1]$ 的转移式，直接展开，考虑每一项的概率及其贡献。

$$
f[i][j][1]=\min(f[i-1][j-1][0]+k_{i}\text{dist}(c_{i-1},d_i)+(1-k_{i})\text{dist}(c_{i-1},c_i),f[i-1][j-1][1]+k_{i-1}k_i\text{dist}(d_{i-1},d_i)+(1-k_{i-1})k_i\text{dist}(c_{i-1},d_i))+k_{i-1}(1-k_i)\text{dist}(d_{i-1},c_i)+(1-k_{i-1})(1-k_i)\text{dist}(c_{i-1},c_i) 
$$

边界是 $f[1][0][0]=f[1][1][1]=0$，显然消耗的体力值为 $0$。最后的答案就是 $f[n][i][0/1]$ 中的最小值，其中 $1\le i\le m$。

```cpp
#include <bits/stdc++.h>
using namespace std;
long long n,m,v,l,a,b,w,c[3000],d[3000],e[400][400];
double k[3000],f[3000][3000][2];
int main()
{
	scanf("%lld%lld%lld%lld",&n,&m,&v,&l);
	for(int i=1;i<=n;i++)scanf("%lld",&c[i]);
	for(int i=1;i<=n;i++)scanf("%lld",&d[i]);
	for(int i=1;i<=n;i++)scanf("%lf",&k[i]);
	for(int i=1;i<=v;i++)
	    for(int j=1;j<=v;j++)
	        if(i!=j)e[i][j]=1e18;
	        else e[i][j]=0;
    for(int i=1;i<=l;i++)scanf("%lld%lld%lld",&a,&b,&w),e[a][b]=e[b][a]=min(e[a][b],w);
    for(int k=1;k<=v;k++)
        for(int i=1;i<=v;i++)
            for(int j=1;j<=v;j++)
                e[i][j]=min(e[i][j],e[i][k]+e[k][j]);
    for(int i=1;i<=n;i++)
        for(int j=0;j<=m;j++)
            f[i][j][0]=f[i][j][1]=1e18;
    f[1][0][0]=f[1][1][1]=0;
    for(int i=2;i<=n;i++)
        for(int j=0;j<=m;j++)
            {
            	f[i][j][0]=min(f[i][j][0],f[i-1][j][0]+e[c[i-1]][c[i]]);
            	f[i][j][0]=min(f[i][j][0],f[i-1][j][1]+k[i-1]*e[d[i-1]][c[i]]+(1-k[i-1])*e[c[i-1]][c[i]]);
            	if(j)f[i][j][1]=min(f[i][j][1],f[i-1][j-1][0]+k[i]*e[c[i-1]][d[i]]+(1-k[i])*e[c[i-1]][c[i]]);
            	if(j)f[i][j][1]=min(f[i][j][1],f[i-1][j-1][1]+k[i-1]*k[i]*e[d[i-1]][d[i]]+k[i-1]*(1-k[i])*e[d[i-1]][c[i]]+(1-k[i-1])*k[i]*e[c[i-1]][d[i]]+(1-k[i-1])*(1-k[i])*e[c[i-1]][c[i]]);
			}
	double mi=1e18;
	for(int i=0;i<=m;i++)
	    for(int j=0;j<2;j++)
	        mi=min(mi,f[n][i][j]);
	printf("%.2lf",mi);
	return 0;
}
```

例题 $6$ ：

[P1654 OSU!](https://www.luogu.com.cn/problem/P1654)

期望具有线性性，但不具有可乘性。也就是说，$X^3$ 的期望不等于 $X$ 的期望的三次方。因此，原先的期望 DP 模型失效，很难套用模型进行递推，我们需要通过期望的定义重新推一遍。

记 $f_w[x]$ 为第 $x$ 个位置产生的 $X^w$ 的期望，位置 $x$ 出现 $1$ 的概率为 $p[x]$，位置 $x$ 前(包括位置 $x$)出现连续 $k$ 个 $1$ 的概率为 $p_x[k]$，展开定义式得位置 $x$ 的期望。

$$
\begin{aligned} f_2[x]&=p[x]\sum_{k}p_{x-1}[k](k+1)^2\\ &=p[x]\sum_{k}p_{x-1}[k](k^2+2k+1)\\ &=p[x](\sum_{k}p_{x-1}[k]k^2+2\sum_{k}p_{x-1}[k]k+\sum_{k}p_{x-1}[k])\\ &=p[x](f_2[x-1]+2f_1[x-1]+1) \end{aligned}
$$

$f_1[x]$ 也是同理，过程就不写了。

$$
f_1[x]=p[x](f_1[x-1]+1) 
$$

记 $f[x]$ 为前 $x$ 个位置 $X^3$ 的和的期望，我们还是展开期望的定义式。

$$
\begin{aligned} f[x]&=p[x]\sum_{k}p_{x-1}[k](f[x-1]+(k+1)^3-k^3)+(1-p[x])f[x-1]\\ &=p[x]\sum_{k}p_{x-1}[k](f[x-1]+3k^2+3k+1)+(1-p[x])f[x-1]\\ &=p[x](\sum_{k}p_{x-1}[k]f[x-1]+3\sum_{k}p_{x-1}[k]k^2+3\sum_{k}p_{x-1}[k]k+1)+(1-p[x])f[x-1]\\ &=p[x](f[x-1]+3f_2[x-1]+3f_1[x-1]+1)+(1-p[x])f[x-1]\\ &=f[x-1]+p[x](3f_2[x-1]+3f_1[x-1]+1)\\ \end{aligned}
$$

这个方法可以扩展到更高次。这启示我们，如果期望无法直接递推，我们考虑展开定义式。

```cpp
#include <bits/stdc++.h>
using namespace std;
long long n;
double x,g1[200000],g2[200000],f[200000];
int main()
{
	scanf("%lld",&n);
	for(int i=1;i<=n;i++)
	    {
	    	scanf("%lf",&x);
	    	g1[i]=(g1[i-1]+1)*x;
	    	g2[i]=(g2[i-1]+2*g1[i-1]+1)*x;
	    	f[i]=f[i-1]+(3*g2[i-1]+3*g1[i-1]+1)*x;
		}
	printf("%.1lf",f[n]);
	return 0;
}
```

例题 $7$ ：

[P3232 \[HNOI2013\] 游走](https://www.luogu.com.cn/problem/P3232)

不管怎样安排边权，每条边被经过的期望次数是一样的，所以我们可以把每条边被经过的期望次数求出来，然后对于期望经过次数少的分配大边权，经过次数多的分配小边权。

但是求边的经过次数无法转移，我们考虑求点的经过次数。我们发现，一条边的经过次数就是两端点的经过次数除以对应的度数之和。注意走到点 $n$ 就直接结束了，所以不需要计算点 $n$。

考虑如何求一个点的期望经过次数 $f[x]$。我们直接套用经典模型。$\text{nxt}(x)$ 表示 $x$ 相邻的节点，$\text{deg}[x]$ 表示点 $x$ 的度数。

$$
f[x]=\sum_{v\in\text{nxt}(x)}\frac{f[v]}{\text{deg}[v]} 
$$

高斯消元求解即可，参见 [【6】矩阵学习笔记](https://www.cnblogs.com/w9095/p/18704170)。

```cpp
#include <bits/stdc++.h>
using namespace std;
long long n,m,u,v,d[1200],f[1200][1200],book[1200],cnt=0;
double a[1200][1200],b[1200],ans[1200],q[240000],sum=0,eps=1e-10; 
void gauss()
{
	long long now=1;
	double div=0;
	for(int i=1;i<=n;i++)
	    {
	    long long mx=now;
	    for(int j=now+1;j<=n;j++)
	        if(fabs(a[j][i])>fabs(a[mx][i]))mx=j;
	    if(fabs(a[mx][i])<eps)continue; 
	    if(now!=mx)
	       {
	       for(int j=1;j<=n;j++)swap(a[now][j],a[mx][j]);
	       swap(b[now],b[mx]);
	       }
	    div=a[now][i],b[now]/=div;
	    for(int j=1;j<=n;j++)a[now][j]/=div;
	    for(int j=now+1;j<=n;j++)
	        {
	        	div=a[j][i],b[j]-=b[now]*div;
	        	for(int k=i;k<=n;k++)a[j][k]-=a[now][k]*div;
			}
		now++;
	    }
	ans[n]=b[n];
	for(int i=n-1;i>=1;i--)
		{
		ans[i]=b[i];
		for(int j=n;j>i;j--)ans[i]-=a[i][j]*ans[j];
		}
}

int main()
{
	scanf("%lld%lld",&n,&m);
	for(int i=1;i<=m;i++)
	    {
	    	scanf("%lld%lld",&u,&v);
	    	f[u][v]=1,f[v][u]=1,d[u]++,d[v]++;
		}
	for(int i=1;i<=n;i++)
	    {
	    if(i==1)b[i]=1;
	    for(int j=1;j<=n-1;j++)
	        if(f[j][i])a[i][j]=-1.0/d[j];
	    a[i][i]=1;
	    }
	gauss();
	for(int i=1;i<=n-1;i++)
	    for(int j=i+1;j<=n;j++)
	        if(f[i][j])
	           {
			   q[++cnt]=ans[i]/d[i];
			   if(j!=n)q[cnt]+=ans[j]/d[j];
		       }
	sort(q+1,q+cnt+1);
	for(int i=1;i<=cnt;i++)sum+=q[i]*(cnt-i+1);
	printf("%.3lf",sum);
	return 0;
}
```

### 后记

学习期望 DP 的过程中，我觉得独立思考是很重要的。通过独立思考，我排除了网上许多不够透彻的文章的干扰，形成了自己的见解，回头看来，有一种轻舟已过万重山的快感。

希望自己能继续保持。

[数学期望 DP](https://www.cnblogs.com/Zeardoe/p/16559432.html)

> 一夜长安冷雨 几声马蹄彷徨
> 
> 烽火长燃撞破天光
> 
> 只拂一拂袖 挥别了当年模样
> 
> 千里江山如烟 谁不志在四方
> 
> 行道水穷不曾回望
> 
> 吟鞭断流水 也能断千肠
> 
> 缱绻往事 何必思量