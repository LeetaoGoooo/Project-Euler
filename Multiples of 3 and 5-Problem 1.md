>If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.
Find the sum of all the multiples of 3 or 5 below 1000.

题目意思很简单:求出1000以下3或者5的倍数的自然数的和

# 分析

3 的倍数可以表示为 n%3 == 0
5 的倍数可以标识为 n%5 == 0


# method 1

有这样的认识,很显然我们得出第一种解决办法:

从 1 遍历到 999,判断出现的数是不是 3 或者 5 的倍数,是的话则将加上


```python

def sumN(n):
    return sum[i for i in range(1,n) if i%3 == 0 or i%5 == 0]
```

很简单也很暴力的解法,这个 n 是多少也就意味着我们多少次,有没有更巧妙的解法呢?

# method 2

别急让我们仔细看这些数( n//m 表示 n 整除 m,eg: 7//3 = 2):

3 的倍数:3,6,9....,n//3

5 的倍数:5,10,15,....,n//5

是不是只需要简单的将这两个和加在一起就对了? 别忘了还有 3 和 5 的最小公倍数 15,

正确的方式应该是将 所有 3 的倍数的和 + 所有 5 的倍数 - 所有 15 的倍数的和

```python
def method3(n):
	sum_3 = sum([i for i in range(1,n,3)])
	sum_5 = sum([i for i in range(1,n,5)])
	sum_15 = sum([i for i in range(1,n,15)])
	return (sum_3 + sum_5 - sum_15)
```

## method 2 的优化

在 method 2 中我们是减去 所有 15 的倍数,这说明什么?说明在遍历过程中,所有 15 的倍数 被遍历了两次,那么如何把这两次变成一次呢?

看下面一组数:

3,5,6,9,10,12,15,18,20,21,24,25,27,30,33,35,40... 好像没看出什么,我们尝试把 3 的倍数全部移除

5,10,20,25,35,40 现在是不是看出来什么了?还没看出什么?再看

5,20,35... 和 10,25,40... 全新的两个序列 

这个时候的结果应该是 上面全新的两个序列的和再加上所有 3 的倍数的和

```python
def method3_promote(n):
	sum_3 = sum([i for i in range(1,n,3)])
	sum_5 = sum([i for i in range(1,n,15)])
	sum_10 = sum([i for i in range(1,n,15)])
	return (sum_3+sum_5+sum_10)	
```


# method 3

等等,这就算完了吗?如果现在把题目中的 1000 改成 10000,100000,1000000 呢?真的还这样遍历?

让我们回过头再仔细看看最初的3个序列:

3 的倍数: 3,6,9,...,n

5 的倍数: 5,10,15,...,n

15 的倍数: 15,30,45,...,n

难道不觉得这序列很熟悉吗? 这三个序列都是**等差数列**啊

等差数列怎么求和? 

>对于等差数列a1,a2,...,an,其中an=a1 x dn,他们的和Sn为：Sn=(a1+an) x n/2 或者 Sn=a1 x (1+dn) x n/2

```python
def SumDivisibleBy(n,target=999):
    p = target // n       # a1 = n, an = p*n
    return n*(p*(p+1))/2  # (a1+an)xp/2 

SumDivisibleBy(3)+SumDivisibleBy(5)-SumDivisibleBy(15)

```

