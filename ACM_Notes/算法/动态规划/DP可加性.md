# DP可加性

## 思路（图论模型）

形如$dp[i][j]=max(dp[i-1][j-w[i]]+v[i],dp[i][j])其中w[i]\leq j\leq m$的dp方程，本质是**dag图最短路径问题**（有向无环图）

对于一条最短路径s->t，其可以拆分成s->u->v->t，其中u->v一条单边（也可以不只是单边，为**边割集**就行）。

反过来，已知s->u与v->t的最短路径，就可以枚举所有的u->v的**边割集**与s->u,v->t拼接就可以求出s->t从最短路径，可以得到如下公式：$dis(s\rightarrow t)=dis(s\rightarrow u)+dis(u\rightarrow v)+dis(v\rightarrow t) 其中(u\rightarrow v)是(s\rightarrow t)的边割集$

也就是说**最短路径满足可加性**，而**部分dp类似于最短路问题，也是满足可加性的**

[例题：清楚姐姐学01背包(Hard Version)](https://ac.nowcoder.com/acm/contest/46812/D)