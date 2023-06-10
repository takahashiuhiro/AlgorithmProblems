# URL:[HDU2152 Fruit](http://acm.hdu.edu.cn/showproblem.php?pid=2152)

# Problem

转眼到了收获的季节，由于有TT的专业指导，Lele获得了大丰收。特别是水果，Lele一共种了N种水果，有苹果，梨子，香蕉，西瓜……不但味道好吃，样子更是好看。 

于是，很多人们慕名而来，找Lele买水果。 

甚至连大名鼎鼎的HDU ACM总教头 lcy 也来了。lcy抛出一打百元大钞，"我要买由M个水果组成的水果拼盘，不过我有个小小的要求，对于每种水果，个数上我有限制，既不能少于某个特定值，也不能大于某个特定值。而且我不要两份一样的拼盘。你随意搭配，你能组出多少种不同的方案，我就买多少份！" 

现在就请你帮帮Lele，帮他算一算到底能够卖出多少份水果拼盘给lcy了。 

注意，水果是以个为基本单位，不能够再分。对于两种方案，如果各种水果的数目都相同，则认为这两种方案是相同的。 

最终Lele拿了这笔钱，又可以继续他的学业了～ 

# Input

本题目包含多组测试，请处理到文件结束(EOF)。 
每组测试第一行包括两个正整数N和M(含义见题目描述，0<N,M<=100) 
接下来有N行水果的信息，每行两个整数A,B(0<=A<=B<=100)，表示至少要买该水果A个，至多只能买该水果B个。 

# Output

对于每组测试，在一行里输出总共能够卖的方案数。 
题目数据保证这个答案小于10^9

# Example

#### Input

2 3
1 2
1 2
3 5
0 3
0 3
0 3

#### Output

2 
12 

# Solution

This is a basic problem for generating function. We construct a power series which elements represent the choice num of fruit, and then make convolutions for every series. Finally , Output the coefficient of an element which power is m.

これは生成函数の基本問題です。果物の選択数を表す要素を持つ級数を構築し、それぞれの級数に畳み込みを行います。最終的には、級数の冪がmである要素の係数を出力します。

# Accept Code #[38590118](http://acm.hdu.edu.cn/viewcode.php?rid=38590118 "Source")

```
#include<bits/stdc++.h>
int n,m,q,w;
std::vector<std::pair<int,int>>tu;
std::vector<int>dp;
std::vector<int>dp_change;
int main()
{
    while(std::cin>>n>>m)
    {
        dp.clear();
        dp_change.clear();
        tu.clear();
        for(int a=0;a<=m;a++)
        {
            dp.push_back(0);
            dp_change.push_back(0);
        }
        for(int a=0 ;a<n;a++)
        {
            std::cin>>q>>w;
            tu.push_back(std::make_pair(q,w));
        }
        for(int a=tu[0].first;a<=tu[0].second;a++)
        {
            dp[a] = 1;
        }
        for(int a=1;a<n;a++)
        {
            for(int b=0;b<=m;b++)
            {
                for(int c=tu[a].first;c<=tu[a].second;c++)
                {
                    if(c+b >= dp.size())continue;
                    dp_change[c+b] += dp[b];
                }
            }
            for(int b=0;b<dp.size();b++)
            {
                dp[b] = dp_change[b];
                dp_change[b] = 0;
            }
        }
        std::cout<<dp[m]<<std::endl;
    }
}
```
