# 数位DP

## 模板1：DFS

```c++
long long n;//区间大小
int num[100];//用于存储拆分数
ll dp[100][5][5];//记忆化数组：一定要注意将所有状态（增加维数）都记忆

long long dfs(int pos/*当前位*/,int limit,/*当前位是否有限制*/,其他状态){
    if(pos==0)return 具题意判断;
    if(dp[pos][limit][其他状态]!=-1)return dp[pos][limit][其他状态];
    int end=limit?num[pos]:9;//若第index位有限制则遍历0~num[pos]，若无限制则遍历0~9
    int ans=0;//统计当前状态的总计数和
    for(int i=0;i<=end;i++){
        ans+=dfs(pos-1/*下一位*/,i==end&&limit/*下一位是否限制*/,其他状态);
    }
    return dp[pos][limit][其他状态]=ans;//记忆化
}

long long digit(long long x){
    memset(dp,-1,sizeof(dp));//每次重新计算一个区间得初始化
    long long pos=0;//位数
    //拆分数
    while(x){
        num[++pos]=x%10;
        x/=10;
    }
    return dfs(pos/*最高位*/,1/*有限制*/,其他状态);//从高位向低位遍历
}
```

## 例1：

[P2602 [ZJOI2010] 数字计数](https://www.luogu.com.cn/problem/P2602)：给定两个正整数 $a$ 和 $b$，求在 $[a,b]$ 中的所有整数中，每个数码(digit)各出现了多少次。

```c++
const int N=100;
int l,r;
int num[N];//整数拆分
int dp[N][N][N];//记忆化
int fact[N]/*10的i次幂*/,suf[N]/*第i位的后缀数：如13413的第5位后缀数位3413*/;

//递归
int dfs(int pos/*当前为第pos位*/,bool limit/*是否有限制*/,bool is/*是否有前导0*/,int index/*统计index出现的次数*/){
    if(pos==0)return 0;
    if(dp[pos][limit][is])return dp[pos][limit][is];
    int end=limit?num[pos]:9;//*end为当前位可枚举的位数
    int ans=0;
    for(int i=0;i<=end;i++){//枚举当前位
        if(i==0&&is)ans+=dfs(pos-1,i==end&&limit,1,index);//*若有前导0
        else if(i==index&&(i==end&&limit))ans+=dfs(pos-1,1,0,index)+suf[pos-1]+1;//*否则，若当前位==index&&有限制，需要加上当前位贡献=suf[pos-1]
        else if(i==index)ans+=dfs(pos-1,i==end&&limit,0,index)+fact[pos-1];//*否则，若当前位==index，需要加上当前位的贡献=10^(pos-1)
        else ans+=dfs(pos-1,i==end&&limit,0,index);//*否则，枚举下一位
    }
    return dp[pos][limit][is]=ans;//记忆化
}

//整数拆分
int digit(int x,int index){
    memset(dp,0,sizeof(dp));
    int pos=0;
    while(x){
        num[++pos]=x%10;//初始化数位
        x/=10;
        suf[pos]=num[pos]*fact[pos-1]+suf[pos-1];//初始化后缀
    }
    return dfs(pos,1,1,index);
}

void solve(){
    fact[0]=1;
    for(int i=1;i<=15;i++)fact[i]=fact[i-1]*10;
    cin>>l>>r;
    for(int i=0;i<=9;i++){
        cout<<digit(r,i)-digit(l-1,i)<<' ';
    }
}
```

