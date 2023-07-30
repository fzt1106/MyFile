# 区间DP

## 介绍

### 思路

求一段区间上的最优解，通过分割区间为更小的区间，通过合并小区间的最优解得到大区间的最优解

### 辨别

1. 求一段区间上的最优解
2. $O(n^3)：区间长度1e2$

## 朴素区间DP

### 模板

```c++
int n;
int dp[maxn][maxn];

void solve(){
    for(int len = 1;len<=n;len++){//枚举长度
        for(int i = 1;i+len-1<=n;i++){//枚举起点，i+len-1<=n
            int j = i+len - 1;
            for(int k = i;k<j;k++){//枚举分割点，更新小区间最优解
                dp[i][j] = min/max(dp[i][j],dp[i][k]+dp[k+1][j]+something);//大区间i~j由小区间i~k与k+1~j合并而来，合并代价为something
            }
        }
    }
}
```

$O(n^3)$

## 区间DP变形

### 环形区间DP

通过增加区间长度将环形变为线性

```c++
int n;
int dp[2*maxn][2*maxn];

void solve(){
    for(int len = 1;len<=n;len++){//枚举长度
        for(int i = 1;i+len-1<=2*n;i++){//枚举起点，i+len-1<=2*n，注意区间起点范围扩大了n
            int j = i+len - 1;
            for(int k = i;k<j;k++){//枚举分割点，更新小区间最优解
                dp[i][j] = min/max(dp[i][j],dp[i][k]+dp[k+1][j]+something);//大区间i~j由小区间i~k与k+1~j合并而来，合并代价为something
            }
        }
    }
    int mx;
    for(int i=1;i<=n;i++)mx=max(mx,dp[i][i+n]);//长度为n的区间共有n个，取最值
}
```

