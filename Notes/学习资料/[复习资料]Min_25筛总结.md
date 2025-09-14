# [复习资料]Min_25筛总结



<div class="toc"><div class="toc-container-header">目录</div><ul><li><a href="#min_25筛总结" rel="noopener nofollow">Min_25筛总结</a><ul><li><a href="#问题" rel="noopener nofollow">问题</a></li><li><a href="#dp" rel="noopener nofollow">dp？</a><ul><li><a href="#求质数" rel="noopener nofollow">求质数</a></li><li><a href="#求合数" rel="noopener nofollow">求合数</a></li><li><a href="#求答案" rel="noopener nofollow">求答案</a></li></ul></li><li><a href="#套上整除分块" rel="noopener nofollow">套上整除分块</a></li><li><a href="#例题" rel="noopener nofollow">例题</a><ul><li><a href="#洛谷p5325-模板min_25筛" rel="noopener nofollow">洛谷P5325 【模板】Min_25筛</a><ul><li><a href="#分析" rel="noopener nofollow">分析</a></li><li><a href="#参考代码" rel="noopener nofollow">参考代码</a></li></ul></li><li><a href="#loj572-libreoj-round-11misaka-network-与求和" rel="noopener nofollow">LOJ572 「LibreOJ Round #11」Misaka Network 与求和</a><ul><li><a href="#分析-1" rel="noopener nofollow">分析</a></li><li><a href="#参考代码-1" rel="noopener nofollow">参考代码</a></li></ul></li></ul></li></ul></li></ul></div>


<h1 id="min_25筛总结">Min_25筛总结</h1>

Min_25筛，据说是一种可以以 $\mathcal O(\frac{n^{\frac{3}{4}}}{\log_2n})$ 的时间复杂度求积性函数 $f(x)$ 前缀和的筛法（网上写的时间复杂度都是这个，我也不知道是不是，我也不会证明）。

可能算是前置知识？：


$$
\lfloor\frac{\lfloor\frac{n}{x}\rfloor}{y}\rfloor=\lfloor\frac{n}{xy}\rfloor

$$

这保证了 $n$ 除以任意数下取整得到的不同的值的个数是 $\sqrt{n}$ 级别的。

<h2 id="问题">问题</h2>

给定积性函数 $f(x)$ ，其中 $f(x)$ 满足：

<ol>
<li>
$f(p)$ 是一个关于 $p$ 的简单多项式。
</li>
<li>
$f(p^k)$ 可以比较快地求出来。
</li>
</ol>

给定 $n(1\le n\le 10^{10})$ ，求：


$$
\sum_{i=1}^nf(i)

$$

<h2 id="dp">dp？</h2>

先用线性筛求出 $\sqrt{n}$ 以内的所有质数 $P_1,\dots,P_m$ ，其中从小到大的第 $i$ 个质数为 $P_i$ 。

将质数和合数分开求解。

<h3 id="求质数">求质数</h3>

求：


$$
\sum_{i=1}^nf(i)[i\text{是质数}]

$$

由于只需要求质数位置的值的和，所以<strong>用一个质数位置的值和 $f(i)$ 质数位置的值相等的函数 $f_0(x)$ 来替换 $f(i)$ 求解质数位置的所有值，并且 $f_0(x)$ 为完全积性函数， $f_0(x)$ 的前缀和特别好求</strong>，这样的 $f_0(x)$ 可能不太好构造，但是注意到问题中 $f(p)$ 是一个关于 $p$ 的简单多项式，所以可以<strong>把质数位置的值拆成好几个函数，使得每个函数质数位置的值都是一个幂函数（ $f(i)=i^\alpha$ ）</strong>，这样完全积性函数和前缀和好求的要求就都满足了。

现在我们要求完全积性函数并且前缀和特别好求的函数 $f_0(x)$ 质数位置值的和，考虑用所有位置的值减去合数位置的值，设：


$$
g(n,j)=\sum_{i=1}^nf_0(i)[i\text{是质数或者}i\text{的最小质因子大于}P_j]

$$

其中 $g()$ 的初值：


$$
g(n,0)=\sum_{i=2}^nf_0(i)

$$

如何求解 $g(n,j)$ ？考虑 $g(n,j)$ 比 $g(n,j-1)$ 少算了什么（需要减去什么）。<strong>少算了最小质因子恰好等于 $P_j$ 的所有合数。</strong>所以我们把这部分减掉就行了：


$$
g(n,j)=g(n,j-1)-f_0(P_j)\times (g(\lfloor\frac{n}{P_j}\rfloor,j-1)-\sum_{i=1}^{j-1}f_0(P_i)[P_i\le\lfloor\frac{n}{P_j}\rfloor])

$$

（从这个转移方程可以看出求函数 $g()$ 的时候第一维只需要求 $n$ 除以任意数下取整的 $\sqrt{n}$ 级别个不同的数这些位置的值即可）

解释一下上面转移方程后半部分的含义，注意到 $f_0()$ 是完全积性函数，所以对于所有最小质因子等于 $P_j$ 的合数，可以把 $f_0(P_j)$ 给提出来放到括号外面，这样剩下来的需要求的就是 <strong>$1\sim \lfloor\frac{n}{P_j}\rfloor$ 中最小质因子大于 $P_{j-1}$ 的所有质数或者合数位置的值</strong>，也就是 $g(\lfloor\frac{n}{P_j}\rfloor,j-1)$ 再减去它额外算的小于等于 $P_{j-1}$ 的所有质数位置的值 $\sum_{i=1}^{j-1}f_0(P_i)[P_i\le \lfloor\frac{n}{P_j}\rfloor]$ 。

当 $P_j^2>n$ 时， $g(n,j)=g(n,j-1)$ （ $1\sim n$ 中不存在最小质因子恰好等于 $P_j$ 的合数），而当 $P_j^2\le n$ 时，后面的 $[P_i\le \lfloor\frac{n}{P_j}\rfloor]$ 可以保证为真，所以可以不用判断，在写代码的时候就是从小到大枚举每一个质数 $P_j$ ，从大到小枚举每一个需要求的 $n$ ，当 $n<P_j^2$ 时就直接 break 即可。

总的转移方程：


$$
g(n,j)=g(n,j-1)-\begin{cases}0 &\ n<P_j^2\\ f_0(P_j)\times (g(\lfloor\frac{n}{P_j}\rfloor,j-1)-\sum_{i=1}^{j-1}f_0(P_i)) &\ n\ge P_j^2 \end{cases}

$$

<h3 id="求合数">求合数</h3>

求：


$$
\sum_{i=1}^nf(i)[i\text{是合数}]

$$

和求质数类似的，设：


$$
S(n,j)=\sum_{i=1}^nf(i)[i\text{是合数并且}i\text{的最小质因子大于等于}P_j]

$$

其中 $S()$ 的初值：


$$
S(n,j)=0\ \ \ P_j^2>n

$$

如何求解 $S(n,j)$ ？考虑 $S(n,j)$ 比 $S(n,j+1)$ 多算了什么（需要加上什么）。<strong>多算了最小质因子恰好等于 $P_j$ 的所有合数</strong>。所以我们把这部分加上就行了：


$$
S(n,j)=S(n,j+1)+\sum_{e=1}(f(P_j^e)\times (S(\lfloor\frac{n}{P_j^e}\rfloor,j+1)+\sum_{i=j+1}f(P_i)[P_iP_j^e\le n])+f(P_j^{e+1})[P_j^{e+1}\le n])

$$

（同样的， $S()$ 的第一维只需要求出 $\sqrt{n}$ 级别个数）

解释一下上面转移方程后半部分的含义，由于 $f()$ 只是积性函数，所以我们需要<strong>枚举最小质因子 $P_j$ 的出现次数 $e$ <strong>，然后分类讨论，</strong>一个合数除掉它的最小质因子之后有三种情况，第一种情况是除掉后仍然为合数，也就是 $f(P_j^e)S(\lfloor\frac{n}{P_j^e}\rfloor,j+1)$ ，第二种情况是除掉后为质数，也就是 $f(P_j^e)\sum_{i=j+1}f(P_i)[P_iP_j^e\le n]$ ，第三种情况是除掉之后等于 $1$ ，这种情况需要保证 $P_j$ 的出现次数大于 $1$ ，而 $e$ 是从 $1$ 开始枚举的，所以加上的是 $f(P_j^{e+1})[P_j^{e+1}\le n]$ 。</strong>

$\sum_{i=j+1}f(P_i)[P_iP_j^e\le n]$ 不好求，考虑使用前缀和相减：


$$
\sum_{i=1}f(P_i)[P_iP_j^e\le n]-\sum_{i=1}^{j}f(P_i)[P_iP_j^e\le n]\\
=g(\lfloor\frac{n}{P_j^e}\rfloor,m)-\sum_{i=1}^jf(P_i)[P_iP_j^e\le n]

$$

考虑 $e$ 的枚举上限是什么，当 $P_j^{e+1}>n$ 时，所有要加上去的部分得到的结果都是 $0$ ，所以 $e$ 的枚举上限就是满足 $P_j^{e+1}\le n$ 的最大的 $e$ ，此时所有 $[]$ 里面的判断都是真，所以可以去掉。当 $P_j^2>n$ 时根本就不需要进入循环，所以从大到小枚举 $n$ 当 $P_j^2>n$ 时直接 break 就行了。

总的转移方程：


$$
S(n,j)=S(n,j+1)+\sum_{e=1}^{P_j^{e+1}\le n}(f(P_j^e)\times (S(\lfloor\frac{n}{P_j^e}\rfloor,j+1)+g(\lfloor\frac{n}{P_j^e}\rfloor,m)-\sum_{i=1}^jf(P_i))+f(P_j^{e+1}))

$$

<h3 id="求答案">求答案</h3>

答案等于质数位置的值加上合数位置的值加上 $1$ 位置的值，所以答案就是：


$$
g(n,m)+S(n,1)+f(1)

$$

求合数的时候的状态设置可以改变一下，如果令：


$$
S(n,j)=\sum_{i=1}^nf(i)[i\text{的最小质因子大于等于}P_j]

$$

也是可以求解的，不过一开始 $S()$ 的初值可能就不是 $0$ 了，考虑的细节可能要多一点点。

<h2 id="套上整除分块">套上整除分块</h2>

我之前对 Min_25筛 和 杜教筛 在整除分块中的复杂度的理解有一点问题，考虑最简单的整除分块问题，求这个式子的值：


$$
\sum_{T}\lfloor\frac{n}{T}\rfloor f(T)

$$

一般的做法是整除分块，找到所有 $\lfloor \frac{n}{T}\rfloor $ 相同的区间 $[l,r]$ （其中 $r=\lfloor n/(n/l)\rfloor $ ），就是求 $\sum_{i=1}^rf(i)-\sum_{i=1}^{l-1}f(i)=S(r)-S(l-1)$ ，其中 $l-1$ 是上一次求出来的 $r$ ，而这次需要求的 $r$ 是 $n$ 除以一个数下取整得到的，所以<strong>所有要求的前缀和的位置都可以表示为 $n$ 除以一个数下取整</strong>，而 Min_25筛 和 杜教筛 都可以求出这些位置的所有值，所以它们套个整除分块的时间复杂度都是对的。

如果要求的是这个式子怎么办：


$$
\sum_T\lfloor\frac{n}{T}\rfloor\lfloor\frac{m}{T}\rfloor f(T)

$$

简单分析后发现要求的前缀和的位置要么可以表示为 $n$ 除以一个数下取整，要么可以表示为 $m$ 除以一个数下取整，所以预处理的时候只需要对 $n$ 求一遍前缀和对 $m$ 求一遍前缀和即可。

<h2 id="例题">例题</h2>

可能会慢慢加？

<h3 id="洛谷p5325-模板min_25筛"><a href="https://www.luogu.com.cn/problem/P5325" target="_blank" rel="noopener nofollow">洛谷P5325 【模板】Min_25筛</a></h3>

<h4 id="分析">分析</h4>

这道题目就是 Min_25 筛的模板题，所以没有什么好讲的，拆质数位置的函数值就是把 $f(p)=p(p-1)=p^2-p$ 拆成 $p^2$ 和 $p$ 。主要就是为了提供代码。

<h4 id="参考代码">参考代码</h4>

```cpp
#include<cmath>
#include<cstdio>
#include<cstring>
#include<iostream>
#include<algorithm>
#define ch() getchar()
#define pc(x) putchar(x)
using namespace std;
template<typename T>void read(T&x){
	static char c;static int f;
	for(c=ch(),f=1;c<'0'||c>'9';c=ch())if(c=='-')f=-f;
	for(x=0;c>='0'&&c<='9';c=ch())x=x*10+(c&15);x*=f;
}
template<typename T>void write(T x){
	static char q[65];int cnt=0;
	if(x<0)pc('-'),x=-x;
	q[++cnt]=x%10,x/=10;
	while(x)
		q[++cnt]=x%10,x/=10;
	while(cnt)pc(q[cnt--]+'0');
}
const int mod=1000000007,saxn=100005,inv6=(mod+1)/6;
int mo(const int x){
	return x>=mod?x-mod:x;
}
long long n;int sqr;
int id0[saxn],id1[saxn];
long long val[saxn*2];
int pos(long long v){
	return v<=sqr?id0[v]:id1[n/v];
}
int vis[saxn],pri[saxn];
int g[saxn*2],gs[saxn*2],s[saxn*2];
int main(){
	read(n);sqr=sqrt(n);int num=0,tot=0;
	for(int i=2;i<=sqr;++i){
		if(!vis[i])pri[++tot]=i;
		for(int j=1;j<=tot;++j){
			int t=i*pri[j];if(t>sqr)break;
			vis[t]=true;if(i%pri[j]==0)break;
		}
	}
	for(long long l=1,r=1;l<=n;l=r=r+1){
		long long tmp=n/l;r=n/tmp;
		val[++num]=tmp;
		if(tmp<=sqr)id0[tmp]=num;
		else id1[n/tmp]=num;
	}
	for(int i=1;i<=num;++i){
		g[i]=val[i]%mod;
		g[i]=1ll*g[i]*(g[i]+1)/2%mod;
		g[i]=mo(mod-1+g[i]);
	}
	int sm=0;
	for(int j=1;j<=tot;++j){
		int pj=pri[j];
		for(int k=1;k<=num;++k){
			long long va=val[k];if(va/pj<pj)break;
			g[k]=mo(mod-1ll*pj*mo(mod-sm+g[pos(va/pj)])%mod+g[k]);
		}
		sm=mo(sm+pj);
	}
	for(int i=1;i<=num;++i){
		gs[i]=val[i]%mod;
		gs[i]=1ll*gs[i]*mo(gs[i]+1)%mod*mo(mo(gs[i]*2)+1)%mod;
		gs[i]=1ll*gs[i]*inv6%mod;gs[i]=mo(mod-1+gs[i]);
	}
	int sms=0;
	for(int j=1;j<=tot;++j){
		int pj=pri[j];
		for(int k=1;k<=num;++k){
			long long va=val[k];if(va/pj<pj)break;
			gs[k]=mo(mod-1ll*pj*pj%mod*mo(mod-sms+gs[pos(va/pj)])%mod+gs[k]);
		}
		sms=mo(sms+1ll*pj*pj%mod);
	}
	sm=mo(mod-sm+sms);
	for(int j=1;j<=num;++j)
		g[j]=mo(mod-g[j]+gs[j]);
	for(int j=tot;j>=1;--j){
		int pj=pri[j];
		for(int k=1;k<=num;++k){
			long long va=val[k],pje=pj;if(va<pje*pj)break;
			for(int e=1;pje<=va/pj;++e,pje*=pj){
				s[k]=mo(s[k]+mo(1ll*(pje%mod)*((pje-1)%mod)%mod*mo(s[pos(va/pje)]+mo(mod-sm+g[pos(va/pje)]))%mod+1ll*((pje*pj)%mod)*((pje*pj-1)%mod)%mod));
			}
		}
		sm=mo(mod-1ll*pj*(pj-1)%mod+sm);
	}
	write(mo(mo(s[1]+1)+g[1])),pc('\n');
	return 0;
}

```

<h3 id="loj572-libreoj-round-11misaka-network-与求和"><a href="https://loj.ac/p/572" target="_blank" rel="noopener nofollow">LOJ572 「LibreOJ Round #11」Misaka Network 与求和</a></h3>

<h4 id="分析-1">分析</h4>

推式子：


$$
\sum_{i=1}^n\sum_{j=1}^nf(\gcd(i,j))^k=\sum_{T}\lfloor\frac{n}{T}\rfloor^2\sum_{d\mid T}f(d)^k\mu(\frac{T}{d})

$$

$f()$ 本来就不太好求，卷上一个 $\mu()$ 就更加不好求了，所以把 $f()$ 和 $\mu()$ 分开：


$$
\sum_{d}f(d)^k\sum_{t}\mu(t)\lfloor\frac{n}{td}\rfloor^2

$$

后面这个式子只和 $\lfloor\frac{n}{d}\rfloor$ 有关，所以整除分块后相当于是要求 $f(d)^k$ 的前缀和，而后面的部分也可以整除分块做，相当于是要求 $\mu(t)$ 的前缀和，整除分块套整除分块即可（时间复杂度是 $\mathcal O(n^{\frac{3}{4}})$ ）。

$\mu(t)$ 的前缀和好求，关键是 $f(d)^k$ 的前缀和，杜教筛无望，考虑使用Min_25筛。（注意这里是<strong>使用Min_25筛筛非积性函数的一个例子</strong>）

考虑Min_25筛的过程，求质数位置的值（求 $g(n,j)$ ）较容易，因为 $f(p)=1$ ，求合数位置的值如何求？<strong>考虑到我们在求合数位置的值 $S(n,j)$ 的时候，我们是按照质数从大到小构造一个数的，一个本来就是合数的数乘上一个比它最小质因子还要小的质数的若干次方， $f()$ 是不会发生改变的，所以在求 $S(n,j)$ 的时候，我们只需要额外考虑除掉 $P_j$ 后是质数或者是 $1$ 这些位置的值</strong>，由此可以将转移方程改写成这样：


$$
S(n,j)=S(n,j+1)+\sum_{e=1}^{P_j^{e+1}\le n}(S(\lfloor\frac{n}{P_j^e}\rfloor,j+1)+(g(\lfloor\frac{n}{P_j^e}\rfloor,m)-j)\times P_j^k+P_j^k)

$$

解释一下，前面的 $S(\lfloor\frac{n}{P_j^e}\rfloor,j+1)$ 就是除掉 $P_j$ 后仍然是合数的部分，除掉 $P_j$ 后无论是质数还是 $1$ ，它们的 $f()$ 都是 $P_j$ ，而 $g(n,m)$ 求的恰好是 $1\sim n$ 以内的质数个数，所以就用大于 $P_j$ 的质数个数乘上 $P_j^k$ 再加上 $P_j^k$ 就行了。

<h4 id="参考代码-1">参考代码</h4>

```cpp
#include<cmath>
#include<cstdio>
#include<cstring>
#include<iostream>
#include<algorithm>
#define ch() getchar()
#define pc(x) putchar(x)
#define uint unsigned int
using namespace std;
template<typename T>void read(T&x){
	static char c;static int f;
	for(c=ch(),f=1;c<'0'||c>'9';c=ch())if(c=='-')f=-f;
	for(x=0;c>='0'&&c<='9';c=ch())x=x*10+(c&15);x*=f;
}
template<typename T>void write(T x){
	static char q[65];int cnt=0;
	if(x<0)pc('-'),x=-x;
	q[++cnt]=x%10,x/=10;
	while(x)
		q[++cnt]=x%10,x/=10;
	while(cnt)pc(q[cnt--]+'0');
}
const uint sqrn=50005;
uint power(uint a,uint x){
	uint re=1;
	while(x){
		if(x&1)re*=a;
		a*=a,x>>=1;
	}
	return re;
}
uint vis[sqrn],pri[sqrn/5],prk[sqrn/5];
uint sqr,id0[sqrn],id1[sqrn];
uint val[sqrn*2],n;
uint pos(uint v){
	return v<=sqr?id0[v]:id1[n/v];
}
uint g[sqrn*2],sf[sqrn*2],smu[sqrn*2];
int main(){
	uint K;read(n),read(K);
	sqr=sqrt(n);uint tot=0;
	for(uint i=2;i<=sqr;++i){
		if(!vis[i])++tot,prk[tot]=power(pri[tot]=i,K);
		for(int j=1;j<=tot;++j){
			int t=i*pri[j];if(t>sqr)break;
			vis[t]=true;if(i%pri[j]==0)break;
		}
	}
	uint num=0;
	for(uint l=1,r=1;l<=n;l=r=r+1){
		uint tmp=n/l;r=n/tmp;
		val[++num]=tmp;
		if(tmp<=sqr)id0[tmp]=num;
		else id1[n/tmp]=num;
	}
	for(uint i=1;i<=num;++i)g[i]=val[i]-1;
	uint sm=0;
	for(uint j=1;j<=tot;++j){
		uint pj=pri[j];
		for(uint i=1;i<=num;++i){
			uint va=val[i];if(va/pj<pj)break;
			g[i]-=g[pos(va/pj)]-sm;
		}
		++sm;
	}
	for(uint j=tot;j>=1;--j){
		uint pj=pri[j],pjk=prk[j];
		for(uint k=1;k<=num;++k){
			uint va=val[k]/pj,pje=pj;if(va<pj)break;
			for(uint e=1;va>=pj;++e,pje*=pj,va/=pj){
				uint ps=pos(va);
				smu[k]+=(e==1?-1:0)*(smu[ps]-g[ps]+sm);
				sf[k]+=sf[ps]+pjk*(g[ps]-sm+1);
			}
		}
		--sm;
	}
	for(uint i=1;i<=num;++i)
		sf[i]+=g[i],smu[i]-=g[i]-1;
	uint ans=0;
	for(uint l0=1,r0=1;l0<=n;l0=r0=r0+1){
		uint m=n/l0,add=0;r0=n/m;
		for(uint l1=1,r1=1;l1<=m;l1=r1=r1+1){
			uint tmp=m/l1;r1=m/tmp;
			add+=tmp*tmp*(smu[pos(r1)]-smu[pos(l1-1)]);
		}
		ans+=add*(sf[pos(r0)]-sf[pos(l0-1)]);
	}
	write(ans),pc('\n');
	return 0;
}


```

<hr>

这是之前写的东西。。。忽略这些东西吧。

Min_25筛是一种可以在 $O(\frac{n^{\frac{3}{4}}}{\log_2n})$ 的时间内求积性函数 $f(x)$ 前缀和的筛法。

两个要求（ $p$ 为质数）：

<ol>
<li>$f(p)$ 是一个关于 $p$ 的简单多项式。</li>
<li>$f(p^k)$ 可以快速求。</li>
</ol>

首先对于每个 $x=\lfloor\frac{n}{i}\rfloor$ 求出 $\sum_{i\text{是质数}}^xf(i)$。

$\lfloor\frac{n}{i}\rfloor$ 共有 $\sqrt{n}$ 中取值，又 $\lfloor\frac{\lfloor\frac{n}{a}\rfloor}{b}\rfloor=\lfloor\frac{n}{ab}\rfloor$ 所以 $n$ 除以任何数向下取整最多有 $\sqrt{n}$ 中取值。

构造函数 $f_0(x)$ 满足 $f_0(x)$ 是<strong>完全积性函数</strong>，并且对于任意质数 $p$ 都满足 $f_0(p)=f(p)$ 。

设 $P_i$ 表示从小到大的第 $i$ 个质数。

设函数 $g(n,j)=\sum_{i\text{是质数或者}i\text{的最小质因子大于}P_j}^nf_0(i)$ 。

转移：


$$
g(n,j)=\begin{cases}\begin{matrix}g(n,j-1)&\ (P_j^2> n)&\\ g(n,j-1)-f_0(P_j)(g(\lfloor\frac{n}{P_j}\rfloor,j-1)-\sum_{i=1}^{j-1}f_0(P_i))&\ (P_j^2\le n)&\end{matrix}\end{cases}

$$

设 $x$ 满足 $P_x^2\le \operatorname{maxn}$ 并且 $P_{x+1}^2>\operatorname{maxn}$ 。

则有： $g(n,x)=\sum_{i\text{是质数}}^nf_0(i)=\sum_{i\text{是质数}}^nf(i)$ 。

设函数 $S(n,j)=\sum_{i\text{的最小质因子}\ge P_j}^nf(i)$ 。

答案是 $S(n,1)+1$ 。

转移：


$$
S(n,j)=g(n,x)-\sum_{i=1}^{j-1}f(P_i)+\sum_{k=j}^{P_k^2\le n}\sum_{e=1}^{P_k^{e+1}\le n}(f(P_k^e)S(\lfloor\frac{n}{P_k^e}\rfloor,k+1)+f(P_k^{e+1}))

$$

<hr>

