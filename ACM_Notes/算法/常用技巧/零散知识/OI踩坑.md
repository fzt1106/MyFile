# OJ踩坑

## 注意点

### 时间估算

> [引用di'z](https://blog.csdn.net/hzf0701/article/details/114931836?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167617240916782429727444%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=167617240916782429727444&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-114931836-null-null.142^v73^control,201^v4^add_ask,239^v1^control&utm_term=%E7%AB%9E%E8%B5%9B%E4%B8%AD%E6%97%B6%E9%97%B4%E4%BC%B0%E7%AE%97&spm=1018.2226.3001.4187)
>
> ![image-20230212112843965](E:\typro图像\image-20230212112843965.png)

## 卡常数

### 关闭同步流

```c++
ios::sync_with_stdio(false);
cin.tie(0);cout.tie(0);
```



### 用`\n`而不用`endl`

在IO很多时，`endl`比`\n`慢很多

![img](E:\typro图像\dfc907caec2c1acbee0ee02594a48fe6.png)

