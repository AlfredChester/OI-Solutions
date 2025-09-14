# 【8】AC自动机学习笔记

### 前言

四大玄学玩意：SPFA，动态规划，组合数学，**AC自动机**。

前置知识：

[【5】KMP学习笔记](https://www.luogu.com.cn/blog/w9095/kmp-xue-xi-bi-ji)

[【6】字典树学习笔记](https://www.luogu.com.cn/blog/w9095/zi-dian-shu-xue-xi-bi-ji)

### AC 自动机

---

给定 $n$ 个模式串 $s_i$ 和一个文本串 $t$，求有多少个不同的模式串在文本串里出现过。

---

这是一个**多模字符串匹配问题**，对于这类问题，我们一般用 AC 自动机解决。

AC 自动机，笼统的说就是在字典树上跑 KMP 算法。整个 AC 自动机算法分为三个部分：建立字典树，建立 fail 指针，多模字符串匹配。

#### 建立字典树

参考 [【6】字典树学习笔记](https://www.luogu.com.cn/blog/w9095/zi-dian-shu-xue-xi-bi-ji)，不多赘述。

```cpp
void insert(char str[])
{
	int l=strlen(str),root=0;
	for(int i=0;i<l;i++)
	    {
	    	int id=str[i]-'a';
	    	if(!trie[root][id])trie[root][id]=++cnt;
	    	root=trie[root][id];
	    }
	ap[root]++;
}
```

#### 建立 fail 指针

和 KMP 一样，失配时，我们可以利用字典树的根节点到这个节点之间的字符时**匹配**的这个信息。构建失配指针，使当前字符失配时跳转到另一段从根节点开始的每一个字符都与当前已匹配字符段某一个后缀**完全相同**且**长度最大**的位置继续匹配。由此可知如果跳转，跳转后的串的**前缀**必须为跳转前的模式串的**后缀**，深度一定**小于**当前节点，所以可以通过 BFS 在字典树上求出 fail 指针。

说人话，就是因为我们已知主串中一段是匹配的，那么我们取出匹配的这一段的**后缀**，作为下一个匹配串的**前缀**，然后从此处继续匹配。

通过 BFS 求 fail 的过程，其在单个节点上求解的步骤也很简单：若这个节点的字符为 $c$，则沿其**父节点的 fail 指针**走，一直走到一个位置，使其子节点中**也有**字符为 $c$ 的节点，将待求节点的 fail 指针指向这个**子节点**就行了。

至于其正确性也不难得知，类似动态规划。沿这个字符为 $c$ 的节点的父节点的 fail 指针走到的位置，由 fail 指针的定义，这些位置一定是到达这个字符为 $c$ 的节点的父节点的字符串的某一个**作为前缀的最长后缀**。而这个字符为 $c$ 的节点的父节点后有一个字符为 $c$ 的节点，这些满足条件的位置后如果**也有**一个字符为 $c$ 的节点，证明后缀**可以更长**，所以可以直接转移。

上面几段话是重点，可能会非常绕人，甚至有些“只可意会，不可言传”的味道。不过我觉得多画画图，多思考，应该能理解。理解其本质之后，才能更好的运用。

下面这段代码节点（包括根节点）编号从 $1$ 开始，所以省去了根节点的特判。

```cpp
void build_ac()
{
	int head=0,tail=0;
	fail[0]=1;
	que[tail++]=1;
	while(head<tail)
	   {
	   	int now=que[head];
	   	for(int i=0;i<26;i++)
	   	    {
	   	       int p=fail[now];
	   	       if(!trie[now][i])continue;
               fail[trie[now][i]]=1;
               que[tail++]=trie[now][i];
			   while(p)
			      {
			      	if(trie[p][i])
			      	   {
					   fail[trie[now][i]]=trie[p][i];
					   break;
				       }
				    p=fail[p];
				  }	
			}
		head++;
	   }
}
```

#### Trie 图优化

上面这种写法，看起来很丑，而且效率不高。我们可以在字典树上建图来进行类似并查集的**路径压缩**，优化算法。

Trie 图优化的核心就是这两行：

```cpp
if(trie[now][i])fail[trie[now][i]]=trie[fail[now]][i],que[tail++]=trie[now][i];
```

这行的意思是，如果字符编号为 $i$ 的子节点存在，那么直接将其指向为父节点的 fail 指针指向的节点的编号为 $i$ 的子节点。如果父节点的 fail 指针指向的节点的编号为 $i$ 的子节点存在，那么很好，否则请看下面这一行。

```cpp
else trie[now][i]=trie[fail[now]][i];
```

这个 `else` 是接在上面那行的 `if` 下面的，也就是说，当父节点的 fail 指针指向的节点的编号为 $i$ 的子节点不存在时，按照这一行，其值已经变为了父节点的 fail 指针指向的节点的 fail 指针指向的节点，相当于未优化的 `while(p)` 中的 `p=fail[p]`。由于每次递推只扩展一层，层层推进，所以记录的状态一定是最新的，可以保证正确性。

注意要直接将根节点的子节点入队，避免在根节点处出现一些异常情况。

```cpp
void build_ac()
{
	int head=0,tail=0;
	fail[0]=0;
	for(int i=0;i<26;i++)
	    if(trie[0][i])que[tail++]=trie[0][i];
	while(head<tail)
	   {
	   	int now=que[head];
	   	for(int i=0;i<26;i++)
	   	    if(trie[now][i])fail[trie[now][i]]=trie[fail[now]][i],que[tail++]=trie[now][i];
	   	    else trie[now][i]=trie[fail[now]][i];
		head++;
	   }
}
```

这样就做到了在严格 $O(\sum|s_i|)$ 的复杂度下求出 fail 指针。

#### 多模字符串匹配

由于求 fail 的时候已经让不存在的节点自动跳到了 fail 指针指向的节点（`trie[now][i]=trie[fail[now]][i]`），所以直接在字典树上走字符边就行了。到每一个节点时通过 fail 指针统计贡献（每一个 fail 指针都必须遍历到，因为如果匹配，作为某个后缀的 fail 肯定也是匹配的），注意计算与标记已经出现过即可。

```cpp
int match_ac(char str[])
{
	int i=0,l=strlen(str),root=0,ans=0;
	while(i<l)
	   {
	   	int id=str[i]-'a';
	   	root=trie[root][id];
	   	for(int j=root;j!=0&&ap[j]!=-1;j=fail[j])ans+=ap[j],ap[j]=-1;
	   	i++;
	   }
	return ans;
}
```

值得一提的是，这个过程的复杂度经常被卡爆，需要通过各种手段来优化，最常用的是预处理和记忆化，这些优化主要出现在例题中。一般来说，一道 AC 自动机的题目超时了，十有八九是这个位置。

### 例题

例题 $1$ ：

[P3808 【模板】AC 自动机（简单版）](https://www.luogu.com.cn/problem/P3808)

AC 自动机板子题，不多赘述。

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,trie[2000010][26],ap[2000010],fail[2000010],que[2000010],vis[2000010],cnt=0;
char str[2000010];
void insert(char str[])
{
	int l=strlen(str),root=0;
	for(int i=0;i<l;i++)
	    {
	    	int id=str[i]-'a';
	    	if(!trie[root][id])trie[root][id]=++cnt;
	    	root=trie[root][id];
	    }
	ap[root]++;
}

void build_ac()
{
	int head=0,tail=0;
	fail[0]=0;
	for(int i=0;i<26;i++)
	    if(trie[0][i])que[tail++]=trie[0][i];
	while(head<tail)
	   {
	   	int now=que[head];
	   	for(int i=0;i<26;i++)
	   	    if(trie[now][i])fail[trie[now][i]]=trie[fail[now]][i],que[tail++]=trie[now][i];
	   	    else trie[now][i]=trie[fail[now]][i];
		head++;
	   }
}

int match_ac(char str[])
{
	int i=0,l=strlen(str),root=0,ans=0;
	while(i<l)
	   {
	   	int id=str[i]-'a';
	   	root=trie[root][id];
	   	for(int j=root;j!=0&&ap[j]!=-1;j=fail[j])ans+=ap[j],ap[j]=-1;
	   	i++;
	   }
	return ans;
}

int main()
{
	scanf("%d",&n);
	for(int i=0;i<n;i++)
	    {
	    	scanf("%s",str);
	    	insert(str);
		}
	scanf("%s",str);
	build_ac();
	printf("%d",match_ac(str));	
	return 0;
}
```

例题 $2$ ：

[P3796 【模板】AC 自动机（加强版）](https://www.luogu.com.cn/problem/P3796)

比较例题 $1$ 确实有加强，必须使用 Trie 图优化。操作有些许不一样，但对比例题 $1$ 只是不需要标记出现过以及多一个分类计数而已。

```cpp
#include <bits/stdc++.h>
using namespace std;
int t,n,trie[20010][26],ap[20010],pl[20010],fail[20010],que[20010],vis[20010],sum[20010][200],to[20010],th[20010],cnt=0;
char str[2000010][200];
void clean(int i)
{
	ap[i]=pl[i]=fail[i]=vis[i]=0;
	for(int j=0;j<26;j++)trie[i][j]=0;
	for(int j=1;j<=n;j++)sum[i][j]=0;
} 

void insert(char str[],int xu)
{
	int l=strlen(str),root=0;
	for(int i=0;i<l;i++)
	    {
	    	int id=str[i]-'a';
	    	if(!trie[root][id])
	    	   {
			   trie[root][id]=++cnt;
			   clean(cnt);
		       }
	    	root=trie[root][id];
	    }
	ap[root]++;
	pl[root]=xu;
}

void build_ac()
{
	int head=0,tail=0;
	fail[0]=0;
	for(int i=0;i<26;i++)
	    if(trie[0][i])que[tail++]=trie[0][i];
	while(head<tail)
	   {
	   	int now=que[head];
	   	for(int i=0;i<26;i++)
	   	    if(trie[now][i])fail[trie[now][i]]=trie[fail[now]][i],que[tail++]=trie[now][i];
	   	    else trie[now][i]=trie[fail[now]][i];
		head++;
	   }
}

void match_ac(char str[])
{
	int i=0,l=strlen(str),root=0;
	while(i<l)
	   {
	   	int id=str[i]-'a';
	   	root=trie[root][id];
	   	for(int j=root;j!=0;j=fail[j])to[pl[j]]+=ap[j];
	   	i++;
	   }
}

int main()
{
	while(scanf("%d",&n)!=-1)
		{
		int maxn=0;
		if(n==0)return 0;
		clean(0);
		for(int i=0;i<n;i++)
		    {
		    	scanf("%s",str[i]);
		    	insert(str[i],i+1);
			}
		scanf("%s",str[n]);
		build_ac();
		match_ac(str[n]);
		for(int i=1;i<=n;i++)maxn=max(to[i],maxn);
		printf("%d\n",maxn);
		for(int i=1;i<=n;i++)
		    if(to[i]==maxn)printf("%s\n",str[i-1]);
		for(int i=1;i<=n;i++)to[i]=0;
		cnt=0;
	    }
	return 0;
}
```

例题 $3$ ：

[P3121 \[USACO15FEB\]Censoring G](https://www.luogu.com.cn/problem/P3121)

因为删除操作的复杂度是 $O(n)$ 的，所以用栈来存储匹配过的字符。每次新匹配一个字符，就压到栈顶，这样删除时只需要多次弹出栈顶字符，然后将状态（AC 自动机上的位置）重置为弹完栈后栈顶元素的状态就行了。

为了方便弹栈进行删除操作，需要预处理出每个节点的深度，用这个来确定弹栈的字符数量。这个预处理可以就在求 fail 时递推完成，十分方便。

按照普通的 AC 自动机的处理，超时了。用一个小优化：预处理出每个位置跳 fail 后第一个可能匹配的位置，匹配时就可以 $O(1)$ 查询。这个预处理同样可以就在求 fail 时递推完成，同样十分方便。

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,trie[100010][26],ap[100010],d[100010],fail[100010],fir[100010],que[100010],star[100010],top=0,cnt=0;
char chu[100010],str[100010],sta[100010];
void insert(char str[])
{
	int l=strlen(str),root=0;
	for(int i=0;i<l;i++)
	    {
	    	int id=str[i]-'a';
	    	if(!trie[root][id])trie[root][id]=++cnt;
	    	root=trie[root][id];
	    }
	ap[root]++;
}

void build_ac()
{
	int head=0,tail=0;
	fail[0]=0;d[0]=0;
	for(int i=0;i<26;i++)
	    if(trie[0][i])que[tail++]=trie[0][i],d[trie[0][i]]=1;
	while(head<tail)
	   {
	   	int now=que[head];
	   	if(ap[now])fir[now]=now;
	   	else fir[now]=fir[fail[now]];
	   	for(int i=0;i<26;i++)
	   	    if(trie[now][i])d[trie[now][i]]=d[now]+1,fail[trie[now][i]]=trie[fail[now]][i],que[tail++]=trie[now][i];
	   	    else trie[now][i]=trie[fail[now]][i];
		head++;
	   }
}

void match_ac(char str[])
{
	int l=strlen(str),root=0,i=0;
	while(i<l)
	   {
	   	int id=str[i]-'a';
	   	root=trie[root][id];
	   	sta[++top]=str[i];star[top]=root;
        for(int k=0;k<d[fir[root]];k++)top--;
	   	root=star[top];
		i++;
	   }
}

int main()
{
	scanf("%s",chu);
	scanf("%d",&n);
	for(int i=0;i<n;i++)
	    {
	    	scanf("%s",str);
	    	insert(str);
		}
	build_ac();
	match_ac(chu);
	for(int i=1;i<=top;i++)
	    printf("%c",sta[i]);
	return 0;
}
```

例题 $4$ ：

[P3041 \[USACO12JAN\]Video Game G](https://www.luogu.com.cn/problem/P3041)

AC 自动机上的动态规划。

由题目中这句话，不难想到 AC 自动机：

> $s_i$ 在 $t$ 中出现一次指的是 $s_i$ 是 $t$ 从某个位置起的连续子串。如果 $s_i$ 从 $t$ 的多个位置起都是连续子串，那么算作 $s_i$ 出现了多次。

考虑到每个位置有多种转移情况，且满足最优子结构性质，使用动态规划。观察后发现确定一个位置需要两个参数：目前是主串的的 $i$ 个字符，目前在 AC 自动机中的位置 $j$。易得转移方程：

$$
dp[i+1][trie[j][k]]=\max(dp[i+1][trie[j][k]],dp[i][j]+h[trie[j][k]]) 
$$

这里 $k$ 是枚举 AC 自动机中的出边，$h[i]$ 表示在 AC 自动机中的位置 $i$ 匹配可以得到的分数。实质上，这是一个根据现在的状态推出后面状态的方程，所以 $\max$ 里会有 $dp[i+1][trie[j][k]]$ 这一项，而后面一项则是目前枚举的状态转移之后的得分。

这里把 $h[i]$ 预处理出来了，实际上不预处理应该也是可以的。

注意状态需要初始化为负无穷，很明显是不能从不可能的状态转移的。

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,k,ans,trie[310][3],ap[310],fail[310],que[310],f[1010][310],h[310],cnt=0;
char str[310];
void insert(char str[])
{
	int l=strlen(str),root=0;
	for(int i=0;i<l;i++)
	    {
	    	int id=str[i]-'A';
	    	if(!trie[root][id])trie[root][id]=++cnt;
	    	root=trie[root][id];
	    }
	ap[root]++;
}

void build_ac()
{
	int head=0,tail=0;
	fail[0]=0;
	for(int i=0;i<3;i++)
	    if(trie[0][i])que[tail++]=trie[0][i];
	while(head<tail)
	   {
	   	int now=que[head];
	   	for(int i=0;i<3;i++)
	   	    if(trie[now][i])fail[trie[now][i]]=trie[fail[now]][i],que[tail++]=trie[now][i];
	   	    else trie[now][i]=trie[fail[now]][i];
		head++;
	   }
}

int main()
{
	scanf("%d%d",&n,&k);
	for(int i=0;i<n;i++)
	    {
	    	scanf("%s",str);
	    	insert(str);
		}
	build_ac();
	for(int i=0;i<=cnt;i++)
	    for(int j=i;j!=0;j=fail[j])h[i]+=ap[j];
	for(int i=0;i<=k;i++)
	    for(int j=0;j<=cnt;j++)
	        f[i][j]=-99999999;
	f[0][0]=0;
	for(int i=0;i<k;i++)
	    for(int j=0;j<=cnt;j++)
	        for(int k=0;k<3;k++)
	            f[i+1][trie[j][k]]=max(f[i+1][trie[j][k]],f[i][j]+h[trie[j][k]]);
	for(int i=0;i<=cnt;i++)
	    ans=max(ans,f[k][i]);
	printf("%d",ans);
	return 0;
}
```

例题 $5$ ：

[P3966 \[TJOI2013\]单词](https://www.luogu.com.cn/problem/P3966)

个人认为这三道紫例题中最难的一道。

关于这题的文本串：输入的每个单词，既是模式串，也是文本串。

首先，对所有单词建立一个 AC 自动机，类似例题 $2$，注意重复的单词统一记录，只插入字典树一次，但匹配时需要插入多次。然后，因为文本串就是这些单词，直接进行匹配，可以拿到 $90$ 分。

由于每次匹配都有可能退化为 $O((\sum s_i)^2)$，且因为空间限制无法对每一个位置进行记忆化，只能考虑其他做法。

在正常统计时，每个节点的 fail 计数时有方向的，一定是从深度深的节点统计到深度浅都节点。又由于文本串已知，基于这两点，我们可以考虑统计出每一个节点被计算的次数，根据标号与结束标记，最后加入对应字符串的贡献。

我们发现，在匹配过程中，每个字符串匹配到的每一个位置都会向上进行一次 fail 计数，可以直接在插入的时候处理。对于重复访问的位置，这里只需要乘上次数即可，不会增加复杂度，大大优化了时间。

根据 fail 的方向性以及指向节点的唯一性，很容易想到树这种结构。可以以 fail 指针的指向为边，再建立一颗 **Fail 树**。注意，这里必须在未经过 Trie 图优化的字典树上建立 fail 树，否则 Trie 图会导致无限循环，干扰结果。如果使用了 Trie 图优化，请额外存储一颗原始的字典树，方便建立 Fail 树。

在 Fail 树上，如果一个节点被计算过一次，那么根据 AC 自动机的求法，其父节点一定也会被计算一次。所以，可以通过递归到叶子节点，通过回溯不断累加节点被计算的次数，乘以该节点的本身贡献就是这个节点做出的总贡献。

这个方法也可以用于优化 AC 自动机的其他题目，一般来说，这样优化后复杂度为较为严格的 $O(\sum s_i+t)$。

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,trie[2000010][26],bie[2000010][26],ap[2000010],pl[2000010],tol[2000010],fail[2000010],que[2000010],vis[20010],to[20010],y[20010],cnt=0,newc=0;
string str[201];
vector<int>ft[2000010];
int insert(string str)
{
	int l=str.size(),root=0;
	for(int i=0;i<l;i++)
	    {
	    	int id=str[i]-'a';
	    	if(!trie[root][id])trie[root][id]=++cnt,bie[root][id]=cnt;
	    	root=trie[root][id];
	    	tol[root]++;
	    }
	if(ap[root]==0)newc++;
	else return pl[root];
	ap[root]++;
	pl[root]=newc;
	return newc;
}

void build_ac()
{
	int head=0,tail=0;
	fail[0]=0;
	for(int i=0;i<26;i++)
	    if(trie[0][i])que[tail++]=trie[0][i];
	while(head<tail)
	   {
	   	int now=que[head];
	   	for(int i=0;i<26;i++)
	   	    if(trie[now][i])fail[trie[now][i]]=trie[fail[now]][i],que[tail++]=trie[now][i];
	   	    else trie[now][i]=trie[fail[now]][i];
		head++;
	   }
}

void build_ft()
{
	for(int root=0;root<=cnt;root++) 
		for(int i=0;i<26;i++)
		    if(bie[root][i])ft[fail[bie[root][i]]].push_back(bie[root][i]);
}

int match_ac(int root)
{
	int cnt=0,l=ft[root].size();
	for(int i=0;i<l;i++)
	    cnt+=match_ac(ft[root][i]);
	cnt+=tol[root];
	to[pl[root]]+=cnt*ap[root];
	return cnt;
}

int main()
{
	scanf("%d",&n);
	for(int i=0;i<n;i++)
		{
		    cin>>str[i];
		    y[i]=insert(str[i]);
		}
	build_ac();
	build_ft();
	match_ac(0);
	for(int i=0;i<n;i++)
		printf("%d\n",to[y[i]]);
	return 0;
}
```

### 后记

教练推荐的 AC 自动机博客：[AC自动机算法详解 （转载）](https://www.cnblogs.com/cmmdc/p/7337611.html)

自己找到的 AC 自动机博客：[强势图解AC自动机](https://www.luogu.com.cn/blog/3383669u/qiang-shi-tu-xie-ac-zi-dong-ji)