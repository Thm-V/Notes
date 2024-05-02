# Toming的蒟蒻集

## 1.杂项

### 1.1 读入优化

```c++
char buf[1<<21],*p1=buf,*p2=buf;
inline int getc(){
    return p1==p2&&(p2=(p1=buf)+fread(buf,1,1<<21,stdin),p1==p2)?EOF:*p1++;
}
inline int int_Read(){	//inline LL LL_Read()
    int ret=0,f=0;
    char ch=getc();
    while(!isdigit(ch)){
        if(ch=='-') f=1;
        ch=getc();
    }
    while(isdigit(ch)){
        ret=ret*10+ch-48;
        ch=getc();
    }
    return f?-ret:ret;
}
```

### 1.2 链式前向星

```c++
int n,m;	//n个点m条边
struct Edge{
	int to,w,next;
}edge[M];	//若建图为双向边,边数需*2
int head[N],cnt;  //head数组需初始化为-1
void init(){
	cnt=0;
	for(int i=1;i<=n;i++)
		head[i]=-1;
}
void Add_Edge(int u,int v,int w){
	edge[cnt].to=v;
	edge[cnt].w=w;
	edge[cnt].next=head[u];
	head[u]=cnt++;
}
```

```c++
for(int i=head[x];~i;i=edge[i].next)	//以此遍历与x相连的边
```

### 1.3 对拍

```
@echo off
:loop
    read.exe
    my.exe
    std.exe
    fc my.out std.out
    if not errorlevel 1 goto loop
pause
```

## 2.常用STL

### 2.1 优先队列



## 3.数学

### 3.1 线性基

- #### 查询最大子集异或和

- #### 查询第K小子集异或和

- #### 合并线性基

```c++
inline int lg2(int x){
	if(!x)	return -1;
	return 31^__builtin_clz(x); // 63^__builtin_clzll(x)用于处理LL类型的x
}

struct Base{
	int CNT=0;
	bool Zero=false;
	LL B[BN];
	Base(){
		for(int i=0;i<BN;i++)
			B[i]=0;
	}
	
	//向线性基中插入元素
	void Ins(LL x){	
		for(int i=lg2(x);~i;i=lg2(x))
				if(B[i])
					x^=B[i];
				else{
					B[i]=x;
					return;	//插入成功
				}
		Zero=true;	//x能被线性基中的数表示,标记特殊情况0
	}
	
	//查询线性基中异或最大值
	LL Query_Max(){	
		LL ANS=0;
		for(int i=BN-1;~i;i--)
			ANS=max(ANS,ANS^B[i]);
		return ANS;
	} 
	
	//查询线性基第K小异或值(重建)
	void Rebulid(){
		for(int i=0;i<BN;i++)
			for(int j=0;j<i;j++)
				if(B[i]>>j&1)
					B[i]^=B[j];
		for(int i=0;i<BN;i++)
			if(B[i])
				B[CNT++]=B[i];
	}
	
	//查询线性基第K小异或值(查询)
	LL Query_MinK(LL k){
		LL ANS=0;
		if(Zero)	
			k--;
		if(k<0)
			return 0;
		if(1ll<<CNT<k+1)
			return -1;
		for(int i=0;i<CNT;i++)
			if(k>>i&1)
				ANS^=B[i];
		return ANS;
	}
	
	//线性基合并
	void Merge(Base &BB){
		for(int i=0;i<BN;i++)
			if(BB.B[i])
				Ins(BB.B[i]);
	}
};
```

### 3.2 位运算

#### 3.2.1 GCC内建函数

- `int __builtin_clz(unsigned int x)`：返回x的二进制的前置零的个数。当x为0时，结果未定义。
- `int __builtin_ctz(unsigned int x)`：返回x的二进制末尾连续0的个数。当x为0时，结果未定义。
- `int __builtin_ffs(int x)`：返回x的二进制末尾最后一个1的位置，最低位编号为1，当x为时返回0。
- `int __builtin_popcount(unsigned int x)`：返回x的二进制中1的个数。
  以上各函数都可以在函数名末尾添加ll（如`__builtin_clzll`），使参数类型变为`(undigned) long long`（返回值仍然是`int`类型）

### 3.3 素数

#### 3.3.1 欧拉筛(线性筛)

```c++
unsigned tot;
vector<int> prime[N];
bool is_not_pr[N];
void GetPrime(){
    for(int i=2;i<=MAX_NUM;i++){
        if(!is_not_pr[i]){
			prime.push_back(i);
             tot++;
        }
        for(usigned j=0;j<tot && i*prime[j]<=MAX_NUM;j++){
            is_not_pr[i*prime[j]]=true;
            if(i%prime[j]==0)
                break;
        }    
    }
}
```



## 4.数据结构

### 4.1 ST表

- #### 用于查询区间 最大值/最小值

```c++
int stmax[N][stN];  //以取区间最大值为例
inline int lg2(int x){
	if(!x)	return -1;
	return 31^__builtin_clz(x); // 63^__builtin_clzll(x)用于处理LL类型的x
}
void st_Build(){
	for(int i=1;i<=n;i++)
		stmax[i][0]=int_Read();
	for(int j=1;(1<<j)<=n;j++)
		for(int i=1;i+(1<<j)-1<=n;i++)
			stmax[i][j]=max(stmax[i][j-1],stmax[i+(1<<j-1)][j-1]);
}

int st_query(int L,int R){
	int wz=lg2(R-L+1);
	return max(stmax[L][wz],stmax[R-(1<<wz)+1][wz]);
}
```

## 5.图论

### 5.1 Tarjan算法

#### 5.1.1 缩点

将**有向图**中的强连通分量缩成一个点，此操作即为缩点

```c++
int dfn[N],low[N],tnt;
bool vis[N];
int Stack[N],t;
int col[N],Val[N],num; //染色
void Tarjan(int x){
	dfn[x]=low[x]=++tnt;
	vis[x]=true;
	Stack[++t]=x;
	for(int i=Head[x];~i;i=edge[i].Next){
		if(!dfn[edge[i].to]){
			Tarjan(edge[i].to);
			low[x]=min(low[x],low[edge[i].to]);
		}
		else if(vis[edge[i].to])
			low[x]=min(low[x],dfn[edge[i].to]);
	}
	if(dfn[x]==low[x]){
		num++;
		do{
			col[Stack[t]]=num;
			Val[num]+=w[Stack[t]];
			vis[Stack[t]]=false;
		}while(Stack[t--]!=x);
	}
}
```

#### 5.1.2 割边（桥）

如果删除**无向图**中的某条边会使无向图的连通分量数增多，则把这条边称为割边或桥。

```c++
int dfn[N],low[N],tnt;
bool vis[N];
int Stack[N],t;
pair<int,int>Bridge[M];
int Bt;
void Tarjan(int x){
	dfn[x]=low[x]=++tnt;
	vis[x]=true;
	Stack[++t]=x;
	for(int i=Head[x];~i;i=edge[i].Next){
		if(!dfn[edge[i].to]){
			Tarjan(edge[i].to);
			low[x]=min(low[x],low[edge[i].to]);
		}
		else if(vis[edge[i].to])
			low[x]=min(low[x],dfn[edge[i].to]);
		if(dfn[x]<low[edge[i].to])
			Bridge[++Bt]=make_pair(x,edge[i].to);
	}
	if(dfn[x]==low[x]){
		do{
			vis[Stack[t]]=false;
		}while(Stack[t--]!=x);
	}
}
```

### 5.2  单源最短路

#### 5.2.1 dijkstra（堆优化）

```c++
int n,m,s;  //结点数 边数 源
bool vis[MAXN];
LL dis[MAXN];                                                           
priority_queue<pair<LL,int> >q;

void dijk(){
    for(int i=1;i<=n;i++)
        vis[i]=false,dis[i]=-1;
    dis[s]=0;
    vis[s]=true;
    for(int i=head[s];~i;i=edge[i].to){
        q.push(make_pair(-edge[i].w,edge[i].v));
        if(dis[edge[i].v]==-1)
            dis[edge[i].v]=edge[i].w;
        else
            dis[edge[i].v]=min(dis[edge[i].v],edge[i].w);
    }
    while(!q.empty()){
        int x=q.top().second;
        q.pop();
        if(vis[x])
            continue;
        vis[x]=true;
        for(int i=head[x];~i;i=edge[i].to){
            if(dis[edge[i].v]>dis[x]+edge[i].w || dis[edge[i].v]==-1){
                dis[edge[i].v]=dis[x]+edge[i].w;
                q.push(make_pair(-dis[edge[i].v],edge[i].v));
            }
        }
    }
}
```

## 6.字符串

### 6.1 字符串哈希

#### 6.1.1 字符串单哈希

```C++
ull Hash(ull B,ull Modp,char s[]){  //此处B和Modp应该尽量大
    ull H=0;
    for(int i=1;i<=strlen(s+1);i++)
        H=((H*B)%Modp+s[i])%Modp;
    return H;
}
```

#### 6.1.2 字符串双哈希

和字符串单哈希类似，只是用不同的进制数和模数分别计算两次，得到该字符串的两个哈希值，用这两个哈希值来确定唯一一个字符串。

## 7.计算几何

### 7.1 前置

```C++
struct POINT{
    int x;
    int y;
    inline POINT operator +(const POINT &p){
        return POINT{x+p.x,y+p.y};
    }
    inline POINT operator -(const POINT &p){
        return POINT{x-p.x,y-p.y};
    }
    
    inline double operator *(const POINT &p){  //点乘
        return x*p.y+y*p.x;
    }
    
    inline double operator ^(const POINT &p){  //叉乘  //注意数据类型
        return x*p.y-y*p.x;
    }
    inline bool operator ==(const POINT &p){
        return x==p.x && y==p.y;
    }
    inline bool operator !=(const POINT &p){
        return !(POINT{x,y}==p);
    }
    inline double len(){
        return sqrt(x*x+y*y);
    }
};

struct LINE{
    POINT l,r;
};
```

### 7.2 极角排序

在struct POINT中插入如下代码段，后面即可用sort等进行排序。

```C++
inline int quadrant(POINT p){  //判断象限
    if(p.x>0 && p.y>=0)
        return 1;
    if(p.x<=0 && p.y>0)
        return 2;
    if(p.x<0 && p.y<=0)
        return 3;
    if(p.x>=0 && p.y<0)
        return 4;
    return 0;  //表示在原点上
}
inline bool operator <(const POINT &p){  //从x正半轴开始逆时针排序
    if(quadrant(POINT{x,y})==quadrant(p))  //先判断象限
        return POINT{x,y}^p>0;  //同一象限内通过判断叉积正负来判断相对大小
    return quadrant(POINT{x,y})<quadrant(p);
}
```

### 7.3 三角形面积(叉积法)

```C++
double Triangle_area_cross(POINT p1,POINT p2,POINT p0){
    return double(abs((p1-p0)*(p2-p0)))/2.0;
}
```

### 7.4 多边形面积（不论凸凹）

```C++
POINT point[N];  //多边形上的点按照逆时针排序
double Polygonal_area(){
    double ans=0;
    for(int i=2;i<n;i++)
        ans+=double((point[i]-point[1])^(point[i+1]-point[1]));  //将多边形切割成多个三角形
    return fabs(ans/2.0);
}
```

7.5  Pick定理（求多边形内部格点数）

给定顶点均为整点的简单多边形，其面积为$S$，内部格点数为$a$，边上格点数目为$b$。Pick定理给定它们之间的关系为$S=a+\frac{b}{2}-1$，常用来求内部格点数，变形为$a=S-\frac{b}{2}+1$。

### 7.6 凸包

#### 7.6.1 Graham扫描法（二维）

```C++
int top;
POINT q[N];

bool cmp(POINT p1,POINT p2){
    p1=p1-point[1];
    p2=p2-point[1];
    if(p1*p2==0)
        return p1.len()<p2.len();
    return p1*p2>0;
}

void Graham_scan(){
    top=0;
    for(int i=2;i<=n;i++)
        if(point[1].x>point[i].x || (point[1].x==point[i].x && point[1].y>point[i].y))
            swap(point[1],point[i]);
   	sort(point+2,point+1+n,cmp);  //以左下角的点为基准点进行极角排序
    q[++top]=point[1];
    for(int i=2;i<=n;i++){
        while(top>=2){
            if((q[top]-q[top-1])^(point[i]-q[top])>0)
                break;
            else{
                top--;
                continue;
            }
        }
        q[++top]=point[i];
    }
}
```

### 7.7 扫描线

#### 7.7.1 求矩形面积并

```C++
int n;	//矩形数量
i64 ans;
    
int leny;
int Y[2*N];  //所有出现的y值，用于离散化

struct POINT{
    int x;
    int y;
};

struct SCAN_LINE{  //垂直于x轴
    int bey;
    int eny;  //bey<=eny
    int x;
    int type;  // 1——进边 -1——出边
    inline bool operator <(const SCAN_LINE &linex1){
        return x==linex1.x ? type>linex1.type : x<linex1.x;  //在求矩形面积并时type先后没影响，但在求矩形周长时先加后减很重要
    }
}scanline[2*N];	//图中每条竖直线的信息

struct TREE{	//把每一个小区间看作一个结点
	int l,r;
	int len;	//真实长度
	int num;	//整个区间覆盖次数
}tree[2*8*N];

void Build(int L,int R,int k){
	tree[k].l=L;
	tree[k].r=R;
	tree[k].len=tree[k].num=0;
	if(L==R)
		return;
	int Mid=(L+R)/2;
	Build(L,Mid,k*2);
	Build(Mid+1,R,k*2+1);
}

void pushup(int k){  //虽是区间加，但无需lazy_tag，因为有进边就有出边，且当父亲被全覆盖时便和儿子没有关系
	if(tree[k].num)
		tree[k].len=Y[tree[k].r+1]-Y[tree[k].l];
	else
		tree[k].len=tree[k*2].len+tree[k*2+1].len;
}

void Update(int aiml,int aimr,int val,int L,int R,int k){	//aiml和aimr代表真实纵坐标值
	if(aiml<=Y[L] && Y[R+1]<=aimr){
		tree[k].num+=val;
		pushup(k);
		return;
	}
	int Mid=(L+R)/2;
	if(aiml<=Y[Mid])	
		Update(aiml,aimr,val,L,Mid,k*2);
	if(Y[Mid+1]<aimr)
		Update(aiml,aimr,val,Mid+1,R,k*2+1);
	pushup(k);
}

void ScanLine(){
    //先处理scanline和Y
    
	//纵坐标离散处理
	sort(Y+1,Y+1+2*n);
	leny=unique(Y+1,Y+1+2*n)-(Y+1);
	
	Build(1,leny-1,1);
	sort(scanline+1,scanline+1+2*n);
	
	for(int i=1;i<2*n;i++){
		Update(scanline[i].bey,scanline[i].eny,scanline[i].type,1,leny-1,1);
		ans=ans+1ll*(scanline[i+1].x-scanline[i].x)*tree[1].len;
	}
}
```

#### 7.7.2 求矩形周长

和求矩形面积并类似，只不过需要分别沿竖直和水平方向各扫描一遍，即只要重写scanline和Y即可。同时，将答案统计一行更改为：

```c++
ans=ans+abs(tree[1].len-last);
last=tree[1].len;	//last定义在循环外面
```

