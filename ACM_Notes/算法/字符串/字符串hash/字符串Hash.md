[toc]

# 字符串Hash

## 思路

字符串Hash就是字符串的映射：**将一个字符串唯一与一个整数对应**

> 给定一个字符串$S=s_1s_2s_3...s_n$，对于第i个字符，定义：
>
> $idx(s[i])=s[i]-'a'+1$
>
> + 可以直接用字符的Ascall码也行
> + 若S为数字组成的字符串，那么设置为idx(x)=s[i]-'0'比较方便
>
> `hash[i]`：**子串[0,i]的hash值**
>
> `Base`：设置的**基数（进制数）**
>
> `p[i]`：其值为Base^i^

## 自然溢出

$hash[i]=hash[i-1]*Base+idx(s[i])$

若设置为hash类型为usinged long long，自然溢出就是$mod=2^{64}-1$

## 单hash

使用自然溢出时得到的数字会很大，因此会给mod取一定值

于是对应的hash公式为：

$hash[i]=(hash[i-1]*Base+idx(s[i]))\% mod$

> 将**Base和mod尽可能取大**，这样出现**hash冲突**的概率会降低
>
> 例如：
>
> 取Base = 13，MOD=101，对字符串abc进行Hash
>
> hash[0] = 0  (相当于 0 字串)
>
> hash[1] = (hash[0] * 13 + 1) % 101 = 1 $\Leftrightarrow$a
>
> hash[2] = (hash[1] * 13 + 2) % 101 = 15 $\Leftrightarrow$ab
>
> hash[3] = (hash[2] * 13 + 3) % 101 = 97 $\Leftrightarrow$abc

## 双hash

为了最大限度减少**hash冲突**，可以将**一个字符串唯一对应一对（两个）数字**

建立两个hash值，其Base和mod不同：

$hash1[i]=(hash[i-1]*Base1+idx(s[i]))\% mod2$

$hash2[i]=(hash[i-1]*Base2+idx(s[i]))\% mod2$

于是映射的结果为`pair<int,int>(hash1[i],hash2[i])`

## 子串的hash

上面的hash方法是求**子串[0...i]**的hash值，但是为了求**子串[l...r]**的hash值，可以利用上述hash方法$O(1)$时间内求出

> 例如$S=s_1s_2s_3s_4s_5$，其hash值为（先忽略mod以及idx(s[i])=s[i]）：
>
> $hash[0]=0$
>
> $hash[1]=s_1$
>
> $hash[2]=s_1*Base+s_2$
>
> $hash[3]=s_1*Base^2+s_2*Base+s_3$
>
> $hash[4]=s_1*Base^3+s_2*Base^2+s_3*Base+s_4$
>
> $hase[5]=s_1*Base^4+s_2*Base^3+s_3*Base^2+s_4*Base+s_5$
>
> 可以知道：字串[3...4]的hash值为：$hash[3...4]=s_3*Base+s_4$
>
> 而其可以通过消去$hash[4]中s_1,s_2项$获得
>
> 于是可以得出$hase[3...4]=hash[4]-hash[2]*Base^2$
>
> 而$Base^2$中次方就是**子串长度r-l+1**

于是可以得到求**子串[l...r]**的hash值公式为：

$hash[l...r]=hash[r]-hash[l-1]*Base^{r-l+1}$

通过**模运算**，注意上述公式中有**减法，可能得到负值，需要考虑对负数取模**，于是公式为：

$hash[l...r]=((hash[r]-hash[l-1]*Base^{r-l+1}\% mod) + mod)\% mod$

> 为了简化其中的$Base^{r-l+1}$的计算，可以对其预处理出来，于是就有了上述的$p[i]=(Base^i)\%mod$
>
> 于是$Base^{r-l+1}=p[r-l+1]\% mod$

于是最终公式为：

$hash[l...r]=((hash[r]-hash[l]*p[r-l+1]\%mod)+mod)\%mod$

