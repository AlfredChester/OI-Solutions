# LCT入门

### 前言

$LCT$，真的是一个无比神奇的数据结构。

它可以**动态**维护**链信息**、**连通性**、**边权**、**子树信息**等各种神奇的东西。

而且，它其实并不难理解。

~~就算理解不了，它简短的代码也很好背。~~

### $LCT$与实边的定义

$LCT$，全称$Link\ Cut\ Tree$，中文名**动态树**。

它的实现有点类似于[**树链剖分**](https://www.cnblogs.com/chenxiaoran666/p/TreeChainDissection.html)，但树链剖分维护的是**重边**和**轻边**（故又称**重链剖分**），而$LCT$维护的则是**实边**和**虚边**。

什么是实边？

我们选择一个节点与其一个儿子的连边为实边，与其他儿子的连边为虚边，这里的实边是可以随时变化的。

而实链剖分与树链剖分最大的区别在于，树链剖分是静态的，所以可以用**线段树**维护，而实链剖分则是动态的，因此就需要用一个更为神奇的数据结构——[$Splay$](https://www.cnblogs.com/chenxiaoran666/p/Splay.html)来进行维护。

于是，就有了$LCT$这个奥秘重重的数据结构。

### $LCT$的简单性质

从上面的内容我们可以知道，$LCT$将一棵树的边分成了**实边**和**虚边**。

而连续的若干条实边构成了**实链**。

而我们**对每条实链分别用一个$Splay$进行维护**，可以保证，每个$Splay$中维护的节点按**中序遍历得到的顺序**在原树中**深度依次增加$1$**（证明：因为我们维护的是一条连续的链啊）。

而虚边的作用则是将这些$Splay$给链接起来，大体连接方式如下：

-   找到该$Splay$中在原树中深度最小的节点，记其为$k$。（具体代码实现时是无需求出这个$k$的，这里只是方便理解）
-   如果$k$是原树中的根节点，则无需连边。
-   否则，我们找到$fa_k$，将该$Splay$的根节点与$fa_k$之间连一条边。

这样一来，就把所有$Splay$连在了一起。

注意到一个节点可能有多个儿子，但实际上它只存储一个儿子，某大佬用一句很精辟的话对其进行了总结：**认父不认子**。

### $LCT$的基本操作

下面，我们来介绍几个$LCT$的基本操作。

-   $IsRoot(x)$
    
    $IsRoot(x)$的作用是判断一个节点$x$是否是当前**实树**的根。
    
    由于我们知道$LCT$是认父不认子的，所以只需要判断当前节点的父亲节点的两个子节点是否为当前节点即可。
    
    代码如下：
    

```cpp
inline bool IsRoot(int x)//判断一个节点x是否是当前实树的根
{
  	return node[node[x].Father].Son[0]^x&&node[node[x].Father].Son[1]^x;//判断fa[x]的两个子节点是否为x
}
```

-   $Rotate(x)\&\&Splay(x)$
    
    关于这个可以自行参考[简析平衡树（三）——浅谈Splay](https://www.cnblogs.com/chenxiaoran666/p/Splay.html)。
    
    然而$Splay$和$LCT$中这两个操作其实还是有一定区别的。
    
    比如说，$LCT$每次固定将节点旋到根，因此只需要一个参数（虽然我博客中$Splay$的第一个模板也是只传一个参的）。
    
    再比如，$LCT$在$Splay$前需要先将当前节点到根节点的路径上所有节点从上往下$PushDown()$一遍。这可以函数递归，也可以直接栈模拟。
    
    具体代码如下：
    

```cpp
#define Which(x) (node[node[x].Father].Son[1]==x)
#define Connect(x,y,d) (node[node[x].Father=y].Son[d]=x)
inline void Rotate(int x)
{
  	register int fa=node[x].Father,pa=node[fa].Father,d=Which(x);
  	!IsRoot(fa)&&(node[pa].Son[Which(fa)]=x),node[x].Father=pa,Connect(node[x].Son[d^1],fa,d),Connect(fa,x,d^1),PushUp(fa),PushUp(x);
} 
inline void Splay(int x)
{
  	register int fa=x,Top=0;
  	while(Stack[++Top]=fa,!IsRoot(fa)) fa=node[fa].Father;//存入栈中
  	while(Top) PushDown(Stack[Top]),--Top;//依次PushDown
  	while(!IsRoot(x)) fa=node[x].Father,!IsRoot(fa)&&(Rotate(Which(x)^Which(fa)?x:fa),0),Rotate(x);
}
```

-   $Access(x)$
    
    $Access(x)$的作用是**把根节点到$x$的路径上的边全部变为实边**。
    
    则我们首先考虑在当前$Splay$中将$x$旋到根，然后将$x$与$fa_x$间的连边更新为实边，即更新$fa_x$的右儿子为$x$；再将$fa_x$在其所在$Splay$中旋到根，同理更新$fa_{fa_x}$的右儿子为$fa_x$... ...
    
    以此类推，直到处理到根节点所在的$Splay$为止。
    
    这样就打通了一条从根节点到$x$的路径。
    
    $Access(x)$可谓是$LCT$最核心的操作，也是后面许多操作的基础。
    
    具体实现可以详见代码：
    

```cpp
inline void Access(int x)//把根节点到x的路径上的边全部变为实边
{
  	for(register int son=0;x;x=node[son=x].Father)
  		Splay(x),node[x].Son[1]=son,PushUp(x);//注意Access过程中要PushUp
}
```

-   $FindRoot(x)$
    
    $FindRoot(x)$的作用是**找到$x$所在的原树中的根节点**，可以用来判断连通性，实现**可撤销并查集**。
    
    我们首先$Access(x)$打通一条从根到$x$的路径，此时$x$就与根节点在同一个$Splay$内了。
    
    然后$Splay(x)$将$x$旋到根。
    
    记住前面提到的$LCT$的性质：每个$Splay$中维护的节点按**中序遍历得到的顺序**在原树中**深度依次增加$1$**。
    
    所以根节点必然是$Splay$中**中序遍历顺序为$1$的节点**。
    
    而这其实就是$x$尽量向左儿子拓展最后得到的节点。
    
    代码如下：
    

```cpp
inline int FindRoot(int x) 
{
  	Access(x),Splay(x);//一波操作，将x转到根节点所在Splay的根
  	while(node[x].Son[0]) PushDown(x),x=node[x].Son[0];//尽量向左儿子拓展，注意每次拓展前先PushDown
  	return Splay(x),x;//最后不忘Splay的优良传统：每执行完一个操作就Splay一下，防被卡
}
```

-   $MakeRoot(x)$
    
    $MakeRoot(x)$的作用是**将$x$作为原树中的新的根节点**。
    
    首先，依然是先$Access(x)$打通一条从根到$x$的路径，然后$Splay(x)$将$x$旋到根。
    
    由前面的操作可知，根节点是$Splay$中**中序遍历顺序为$1$的节点**。
    
    而此时，$x$必然是$Splay$中中序遍历最后得到的点。
    
    因此我们只要翻转该$Splay$，$x$就变成中序遍历顺序为$1$的节点了。
    
    代码如下：
    

```cpp
inline void Rever(int x)//翻转子树
{
  	swap(node[x].Son[0],node[x].Son[1]),node[x].Rev^=1;//交换左右儿子，然后更新标记
}
inline void MakeRoot(int x)//将x作为原树中的新的根节点
{
  	Access(x),Splay(x),Rever(x);//将x转到根节点所在Splay的根，然后翻转Splay
}
```

-   $Link(x,y)$
    
    $Link(x,y)$的作用是**在$x$和$y$两个节点间连一条边**。
    
    首先，我们将$x$作为它所在树的根，即$MakeRoot(x)$。
    
    然后，我们需要判断$x$与$y$是否联通。由于$x$是其所在子树的根节点，因此只要判断$FindRoot(y)$是否为$x$即可。
    
    连接只需要更新$x$的父亲为$y$即可。
    
    代码如下：
    

```cpp
inline void Link(int x,int y)//在x和y两个节点间连一条边
{
  	MakeRoot(x),FindRoot(y)^x&&(node[x].Father=y);//判断x和y的连通性，然后连接
}
```

-   $Cut(x,y)$
    
    $Cut(x,y)$的作用是**删除$x$和$y$之间的边**。
    
    首先，我们依然将$x$作为它所在树的根。
    
    则可以保证，若$x$和$y$有边相连，一定满足一下三个条件：
    
    1.  $y$所在树的根为$x$，即$FindRoot(y)==x$。
    2.  $y$的父亲节点为$x$，即$fa_y==x$。
    3.  $y$没有左儿子。因为如果$y$有左儿子，由于$LCT$的性质，可得$Depth_x<Depth_{leftson_y}<Depth_y$，则$x$和$y$必然不相连。
    
    代码如下：
    

```cpp
inline void Cut(int x,int y)//删除x和y之间的边
{
  	MakeRoot(x),!(FindRoot(y)^x)&&!(node[y].Father^x)&&!node[y].Son[0]&&(node[y].Father=node[x].Son[1]=0,PushUp(x));//判断x和y的连通性，然后删边
}
```

-   $Split(x,y)$
    
    $Split(x,y)$的作用是**从$LCT$中抠出$x$与$y$之间的路径**。
    
    这样一来，就方便我们查询了。
    
    这个操作第一步便是将$x$作为根，然后打通$x$到$y$的路径。
    
    可以保证，此时$x$与$y$所在的$Splay$内**只包含$x$与$y$路径上的节点**。
    
    然后我们将$Splay(y)$将$y$旋至$Splay$的根，这样一来就可以通过查询$y$的信息来进行询问了。
    
    代码如下：
    

```cpp
inline void Split(int x,int y)//从LCT中抠出x与y之间的路径
{
  	MakeRoot(x),Access(y),Splay(y);//将x作为根，打通x与y的路径并将y旋到根
}
```

### 模板（[板子题](https://www.luogu.org/problemnew/show/P3690)）

```cpp
#include<bits/stdc++.h>
#define N 300000
#define swap(x,y) (x^=y^=x^=y)
using namespace std;
int n,a[N+5];
class Class_FIO
{
    private:
        #define Fsize 100000
        #define tc() (A==B&&(B=(A=Fin)+fread(Fin,1,Fsize,stdin),A==B)?EOF:*A++)
        #define pc(ch) (FoutSize<Fsize?Fout[FoutSize++]=ch:(fwrite(Fout,1,Fsize,stdout),Fout[(FoutSize=0)++]=ch))
        int Top,FoutSize;char ch,*A,*B,Fin[Fsize],Fout[Fsize],Stack[Fsize];
    public:
        Class_FIO() {A=B=Fin;}
        inline void read(int &x) {x=0;while(!isdigit(ch=tc()));while(x=(x<<3)+(x<<1)+(ch&15),isdigit(ch=tc()));}
        inline void writeln(int x) {while(Stack[++Top]=x%10+48,x/=10);while(Top) pc(Stack[Top--]);pc('\n');}
        inline void clear() {fwrite(Fout,1,FoutSize,stdout),FoutSize=0;}
}F;
class Class_LCT
{
    private:
        #define LCT_SIZE N
        #define PushUp(x) (node[x].Sum=node[x].Val^node[node[x].Son[0]].Sum^node[node[x].Son[1]].Sum)
        #define Rever(x) (swap(node[x].Son[0],node[x].Son[1]),node[x].Rev^=1)
        #define PushDown(x) (node[x].Rev&&(Rever(node[x].Son[0]),Rever(node[x].Son[1]),node[x].Rev=0))
        #define Which(x) (node[node[x].Father].Son[1]==x)
        #define Connect(x,y,d) (node[node[x].Father=y].Son[d]=x)
        #define IsRoot(x) (node[node[x].Father].Son[0]^x&&node[node[x].Father].Son[1]^x)
        #define MakeRoot(x) (Access(x),Splay(x),Rever(x))
        #define Split(x,y) (MakeRoot(x),Access(y),Splay(y)) 
        int Stack[LCT_SIZE+5];
        struct Tree
        {
            int Val,Sum,Father,Rev,Son[2];
        }node[LCT_SIZE+5];
        inline void Rotate(int x)
        {
            register int fa=node[x].Father,pa=node[fa].Father,d=Which(x);
            !IsRoot(fa)&&(node[pa].Son[Which(fa)]=x),node[x].Father=pa,Connect(node[x].Son[d^1],fa,d),Connect(fa,x,d^1),PushUp(fa),PushUp(x);
        }
        inline void Splay(int x)
        {
            register int fa=x,Top=0;
            while(Stack[++Top]=fa,!IsRoot(fa)) fa=node[fa].Father;
            while(Top) PushDown(Stack[Top]),--Top;
            while(!IsRoot(x)) fa=node[x].Father,!IsRoot(fa)&&(Rotate(Which(x)^Which(fa)?x:fa),0),Rotate(x);
        }
        inline void Access(int x) {for(register int son=0;x;x=node[son=x].Father) Splay(x),node[x].Son[1]=son,PushUp(x);}
        inline int FindRoot(int x) {Access(x),Splay(x);while(node[x].Son[0]) PushDown(x),x=node[x].Son[0];return Splay(x),x;}
    public:
        inline void Init(int len,int *data) {for(register int i=1;i<=len;++i) node[i].Val=data[i];}
        inline void Link(int x,int y) {MakeRoot(x),FindRoot(y)^x&&(node[x].Father=y);}
        inline void Cut(int x,int y) {MakeRoot(x),!(FindRoot(y)^x)&&!(node[y].Father^x)&&!node[y].Son[0]&&(node[y].Father=node[x].Son[1]=0,PushUp(x));}
        inline void Update(int x,int v) {Splay(x),node[x].Val=v;}
        inline int Query(int x,int y) {return Split(x,y),node[y].Sum;}
}LCT;
int main()
{
    register int query_tot,i,op,x,y;
    for(F.read(n),F.read(query_tot),i=1;i<=n;++i) F.read(a[i]);
    for(LCT.Init(n,a);query_tot;--query_tot)
    {
        F.read(op),F.read(x),F.read(y);
        switch(op)
        {
            case 0:F.writeln(LCT.Query(x,y));break;
            case 1:LCT.Link(x,y);break;
            case 2:LCT.Cut(x,y);break;
            case 3:LCT.Update(x,y);break;
        }
    }
    return F.clear(),0;
} 
```

### 后记

推荐一些比较好的$LCT$题目：[【转载】LCT题单](https://www.cnblogs.com/chenxiaoran666/p/LCT_problems.html)。