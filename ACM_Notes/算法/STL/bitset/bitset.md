# bitset

## 使用说明

1. 头文件：`#include<bitset>`

2. 声明容器：`bitset<n>bs` n为容器大小，存储空间占n bits

3. 构造函数：
   + `bitset()` 每一位都是false，于是在定义bitset容器时，每一位都是0
   + `bitset(unsigned long val)` 若传参数val，则bitset中每一位都是val的二进制形式
   + `bitset(const string & str)` 若传参数str（01字符串），则将字符串str转化为bitset
   
4. 重载运算符：
   + `operator []`  下标运算符，访问其特定的一位（同数组）
   + `operator ==/!=` 相等/不等运算符
   + `operator &/|/^/~` 与/或/异或/取反（**bitset只能与bitset进行位运算符**，若要和整数进行位运算，需要先将整型转换位bitset）
   + `operator <</>>` 左移、右移运算符
   + `operator </>` 流运算符，可以直接输入输出bitset
   
5. 成员函数
   + `size()`
   + `count()` 返回1的数量
   + `any()` 若存在某一位为1，返回true，否则返回false
   + `all()` 若所有位都为1，返回true，否则返回false
   + `none()` 若所有位都为0，返回true，否则返回false
   + `set()` 将整个bitset设置为1
   + `set(pos,1/0)` 将某一位设置为1/0
   + `reset()` 将整个bitset设置为0
   + `reset(pos)` 将某一位设置为0（同`set(pos,0)`）
   + `flip()` 翻转所有位
   + `flip(pos)` 翻转某一位
   + `to_string()` 返回转换成字符串的表达
   + `to_ulong(),to_ullong` 返回转换成unsigned long/unsigned long long的表达
   + `_Find_first()` 返回bitset第一个true的下标，若没有true，则返回bitset的大小
   + `_Find_next(pos)` 返回pos（严格大于pos的位置）后第一个true的下标，若pos后没有true则返回bitset的大小
   
