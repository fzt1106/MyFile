# [B-Auspiciousness](https://ac.nowcoder.com/acm/contest/57357/B)

## Information

> + Resource：[2023牛客暑期多校训练营3](https://ac.nowcoder.com/acm/contest/57357/B)
> + Degree of difficulty：Hard- -
> + Tags：组合数学，DP

## 题意

> ###### Descripition
>
> Dog Card is a card game. In the game, there are a total of $ 2n$cards in the deck, each card has a value, and the values of these $2n$ cards form a permutation of $1∼2n$. There is a skill that works as follows:
>
> 1.  Draw a card from the top of the deck.
> 2.  If the deck is empty, then skip to step 3, otherwise you guess whether the card on the top of the deck has a higher value than your last drawn card and draw a card from the top of the deck. If your guess is correct, then repeat this step, otherwise skip to step 3.
> 3.  End this process.
>
> Nana enjoys playing this game, although she may not be skilled at it. Therefore, her guessing strategy when using this skill is simple: if the value of the last drawn card is less than or equal to $n$, then she guesses that the next card's value is higher; otherwise, she guesses that the next card's value is lower. She wants to know, for all different decks of cards (Obviously, there are $2n!$ cases), how many cards she can draw in total if she uses the skill only once in each case. Since this number can be very large, please provide the answer modulo a given value.
>
> ###### Input
>
> The first line contains a single integer $t\ (1\leq t \leq 300)$— the number of test cases.Each test case consists of one line containing two integers$n\ (1\leq n \leq 300)$and $m\ (2\leq m \leq 10^9)$, denoting half the total number of cards in the deck and the modulus respectively.It is guaranteed that the sum of nn over all test cases does not exceed 300.
>
> ###### Output
>
> For each test case, print one integer — the total number of cards Nana can draw modulo m.

> 对于$1\backsim 2*n$的每一个排列，执行以下操作：
>
> + 从头开始抽一个数$x$
>
> + 若没有数剩余，则停止操作
>
> + 若有数剩余，需要进行以下猜测
>
>   + 若$x\leq n$，猜下一个数$y\geq x$
>   + 若$x>n$，猜下一个数$y<n$
>
> + 若猜错，则停止操作，发走
>
>   

## 思路



## 代码

复杂度

```c++

```

