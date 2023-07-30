[toc]

# 状压DP

## 旅行商问题TSP

### [例1：白书1：3.4.1](白书1：3.4.1)

#### 题目描述

> 给定一个n个顶点组成的带权有向图的距离矩`g[i][j]`(INF表示没有边)。要求从顶点0出发，经过每个顶点恰好一次在后再回到顶点0。问所经过的边的总权重的最小值为多少？
>
> $2\leq n \leq 15,0\leq g_{i,j}\leq 1000$
>
> ```c
> input:
> 5
> 0 3 -1 4 -1
> -1 0 5 -1 -1
> 4 -1 0 5 -1
> -1 -1 -1 0 3
> 7 6 -1 -1 -1
> output:
> 22
> 0 3 4 1 2 0
> ```

#### 思路

`S`：已经访问过的顶点的集合（起点0当作还未访问过的顶点）

`v`：当前所在顶点

`u`：v向u转移，u不属于S

`dp[S][v]：经过集合S中的点，当前处于点v,访问所有剩余节点回到顶点0的最短距离`
$$ {状态转移方程}
\left\{ 
\begin{array}
    dp[V][v]=0 \\
    dp[S][v]=dp[S|(1<<u)][u]+g[v][u]
\end{array}
\right.
$$
PS:当需求0到其他点x的最短哈密顿路径时，只需改变一下边界条件

==核心思想：状态转移方程中下标是**集合**而不是**整数**，但是可以通过**记忆化搜索**将其**编码**成一个整数，或者给它们定义一个**全序关系并用二叉搜索树存储**，从而可以用记忆化搜索求解。特别的，对于元素只有0和1两种状态的集合，可以用**二进制数**来**压缩状态**==

#### 代码1：DFS递归

```c++
const int N=(1<<20);
int n;
int g[25][25];
/*
 *S为以访问的点，v是当前所在点
 *dp[N][v]表示从v出发，访问所有剩余节点后回到顶点0的最短距离
*/
int dp[N][25];
int pre[N][25];

int dfs(int S/*以访问过点的集合*/,int v/*目前所在点*/){
	if(dp[S][v]>=0)return dp[S][v];//记忆化
	if(S==(1<<n)-1&&v==0)return dp[S][v]=0;//边界条件  ！！当需求0到其他点x的最短哈密顿路径时，只需改变一下边界条件
	int ans=inf;
	for(int u=0;u<n;u++){
		if(!(S>>u&1)){
			ans=min(ans,dfs(S|(1<<u),u)+g[v][u]);//v的下一个点可以为u，状态从dp[S|(1<<u)][u]转移而来
            //*路径还原：pre数组存储路径
			// int tp=dfs(S|(1<<u),u)+g[v][u];//v的下一个点可以为u，状态中dp[S|(1<<u)][u]转移而来
			// if(tp<ans){
			// 	ans=tp;
			// 	pre[S|(1<<u)][u]=v;//记录路径：u的上一个节点为v
			// }
		}
	}
	return dp[S][v]=ans;
}

//路径还原
vector<int>Path(int S,int u){
	vector<int>P;
	P.push_back(u);
	while(S){
		int v=pre[S][u];//v是u的上一个节点
		P.push_back(v);
		S&=~(1<<u);
		u=v;
	}
	reverse(P.begin(),P.end());
	return P;
}

void solve(){
	cin>>n;
	for(int i=0;i<n;i++){
		for(int j=0;j<n;j++){
			cin>>g[i][j];
			if(g[i][j]==-1)g[i][j]=inf;
		}
	}
	memset(dp,-1,sizeof(dp));
	cout<<dfs(0,0)<<endl;
	// vector<int>path=Path((1<<n)-1,0);
	// for(auto i:path)cout<<i<<' ';
	// cout<<endl;
}

```

#### 代码2：逆向递推

相当于dfs回溯阶段，有时比较难确定递推顺序时用记忆化搜索可以解决

```c++
const int N=(1<<20);
int n;
int g[25][25];
int dp[N][25];

void solve(){
    cin>>n;
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            cin>>g[i][j];
            if(g[i][j]==-1)g[i][j]=inf;
        }
    }
    memset(dp,0x3f,sizeof(dp));
    dp[(1<<n)-1][0]=0;//初始化
    for(int S=(1<<n)-1;S>=0;S--){//逆序递推
        for(int v=0;v<n;v++){
            for(int u=0;u<n;u++){
                if(!(S>>u&1)){
                    dp[S][v]=min(dp[S][v],dp[S|(1<<u)][u]+g[v][u]);//当前节点v状态S从下一个节点u状态S|(1<<u)转移而来
                }
            }
        }
    }
    cout<<dp[0][0]<<endl;
}
/*
input:
5
0 3 -1 4 -1
-1 0 5 -1 -1
4 -1 0 5 -1
-1 -1 -1 0 3
7 6 -1 -1 -1
output:
22
0 3 4 1 2 0
*/
```

### [例题2：Acwing:91.最短Hamilton路径](https://www.acwing.com/problem/content/93/)

#### 问题描述

> 问题描述：给定一张n个点的带权无向图，点从0∼n−1标号，求起点0到终点n−1的最短Hamilton路径。
>
> $1\leq n \leq 20$
>
> $0 \leq a[i,j]\leq 10^7$

#### 思路

`S`：已经访问过的顶点的集合

`j`：终点

`dp[S][j]：经过集合S中的点，到达终点j中的最短路径`
$$
\left\{ 
\begin{array}
    dp[1][0]=0 \\
    dp[S加上k][j]=dp[S][j]+g[j][k]
\end{array}
\right.
$$
PS:问题可以转变成：从**任意起始点**到**任意终点**的最短哈密顿路径，只需改变一下初始化条件即可

#### 代码

```c++
//DP：正向递推
const int N=(1<<20);
int n;
int g[25][25];
int dp[N][25];//dp[S][j]：经过集合S中的点，到达终点j的最短路径


void solve(){
    cin>>n;
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            cin>>g[i][j];
            // if(g[i][j]==-1)g[i][j]=inf;
        }
    }
    memset(dp,0x3f,sizeof(dp));
    dp[1][0]=0;//经过顶点0,终点为0时距离为0    //起点不同时只需改变相应的初始化
    for(int S=0;S<(1<<n);S++){//S为经过点的集合
        for(int j=0;j<n;j++){//j为终点
            if(S>>j&1){//保证终点j在S中
                for(int k=0;k<n;k++){//j的下一步为k
                    if(!(S>>k&1)){//保证k不在S中
                        dp[S|(1<<k)][k]=min(dp[S|(1<<k)][k],dp[S][j]+g[j][k]);//S加k可以从S转移而来，从j下一步走到k
                    }
                }
            }
        }
    }
    cout<<dp[(1<<n)-1][n-1]<<endl;
}
```

---

## 利用转移转移建图

+ 将**状态**作为**顶点**
+ 将**状态转移**作为**边**
+ 建成图，之和就可以利用图论（最短路）来解决

### [例1：Traveling by Stagecoach](http://poj.org/problem?id=2686)

#### 题目描述

> 问题描述：有n张车票，m个城市，p条无向道路连接两座城市，第i张车票有t[i]个马车，从u到v使用第i张车票花费`g[u][v]/t[i]`时间，问从a城市到b城市所需的最短时间。若无法到达b城市则输出“Impossible”
>
> $1\leq n\leq 8$
>
> $2\leq m \leq 30$

#### 思路

`dp[S][u]：当前所用车票状态为S，所在城市为u的最小花费`

使用另一车票`k`(k不属于S)并从`u->v`，其花费为`g[u][v]/t[k]`<=>状态`dp[S][u]`与状态`dp[S|(1<<k)][v]`连了一条权重为`g[u][v]/t[k]`的边

#### 代码

```c++
const int N=(1<<8);
int n,m,p,a,b;
int t[10];
int g[35][35];
double dp[N][35];

void solve(){
 while(cin>>n>>m>>p>>a>>b&&n){
     memset(g,-1,sizeof(g));
     for(int i=0;i<n;i++)cin>>t[i];
     for(int i=0;i<p;i++){
         int x,y,z;
         cin>>x>>y>>z;
         g[x][y]=g[y][x]=z;
     }
     memset(dp,0x7f,sizeof(dp));
     dp[0][a]=0;
     for(int i=0;i<(1<<n);i++){//已使用的车票状态
         for(int u=1;u<=m;u++){//当前所在点u
             for(int k=0;k<n;k++){//k为未使用的车票
                 if(!(i>>k&1)){
                     for(int v=1;v<=m;v++){//从u转移到v
                         if(g[u][v]!=-1)dp[i|(1<<k)][v]=min(dp[i|(1<<k)][v],dp[i][u]+g[u][v]*1.0/t[k]);//利用车票k，从u走到v，代价未g[u][v]/t[k]
                     }
                 }
             }
         }
     }
     double ans=inf;
     for(int i=0;i<(1<<n);i++){
         ans=min(ans,dp[i][b]);
     }
     if(ans!=inf)cout<<ans<<endl;
     else cout<<"Impossible"<<endl;
 }
}
```

### [例2：P2622 关灯问题II](https://www.luogu.com.cn/problem/P2622)

#### 问题描述

> 问题描述：

#### 思路

每一种操作可以将**一状态**与**另一状态**连边，因此在Dijkstra算法中**枚举边**的部分换为**枚举状态转移**即可转换为Dijkstra求解

#### 代码

```c++
const int N=1100;
int n,m;
int a[N][N];
int dis[N],vis[N];

struct node{
int v,w;
 bool operator<(const node&x)const{
  return w>x.w;
}
};

void dijkstra(){
 memset(dis,0x3f,sizeof(dis));
 dis[(1<<n)-1]=0;
priority_queue<node>que;
que.push(node{(1<<n)-1,0});
while(!que.empty()){
     int u=que.top().v;
     que.pop();
     if(vis[u])continue;
     vis[u]=1;
     if(u==0)break;
     //!!!每一种操作可以对应一条边
     for(int i=1;i<=m;i++){
         int v=0;
         for(int j=0;j<n;j++){
             int cur=u>>j&1;
             if(cur==1&&a[i][j]==1)continue;
             else if(cur==0&&a[i][j]==-1)v|=(1<<j);
             else if(cur==1)v|=(1<<j);
         }
         if(dis[v]>dis[u]+1){
             dis[v]=dis[u]+1;
             que.push(node{v,dis[v]});
         }
     }
 }
 if(vis[0])cout<<dis[0]<<endl;
 else cout<< -1<<endl;
}

void solve(){
 cin>>n>>m;
 for(int i=1;i<=m;i++){
     for(int j=0;j<n;j++){
         cin>>a[i][j];
  }
}
dijkstra();
}
```
