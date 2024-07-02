


## 两个基本原理是排列和组合的基础
（1）加法原理：做一件事，完成它可以有n类办法，在第一类办法中有m1 种不同的方法，在第二类办法中有m2种不同的方法，……，在第n类办法中有mn 种不同的方法，那么完成这件事共有N = m 1 + m 2 + m 3 + ⋅ ⋅ ⋅ + mn 种不同方法。
（2）乘法原理：做一件事，完成它需要分成n个步骤，做第一步有m1种不同的方法，做第二步有m2 种不同的方法，……，做第n步有mn种不同的方法，那么完成这件事共有N = m 1 × m 2 × m 3 × ⋅ ⋅ ⋅ × mn种不同的方法。

这样完成一件事的分“类”和“步”是有本质区别的，因此也将两个原理区分开来。

### 例如
(1)从甲城市到乙城市一共有几条路？5+3+7=15条【加法原理】
<img width="215" alt="111" src="https://github.com/mcoblackmore/liuchanghui.github.io/assets/49425642/eda814c1-4d61-4b36-89b5-6433b88648df">

(2)甲城市到丙城市有几条走法？3x2=6条【乘法原理】
<img width="328" alt="222" src="https://github.com/mcoblackmore/liuchanghui.github.io/assets/49425642/892af673-d9ef-4c64-a433-bc0ff1eab7b8">

(3)甲城市到丙城市有几条走法？2+3+2x3条【乘法原理】&【加法原理】
<img width="289" alt="333" src="https://github.com/mcoblackmore/liuchanghui.github.io/assets/49425642/8b351b9e-4f82-497e-bd9d-6e28ae431910">


换句话说，加法、乘法原理就是排列组合的垫脚石。

##  排列组合

今天讲解信息学奥林匹克竞赛比较常见的一种题型——排列组合问题。阅读并理解本篇内容要求读者具有不低于高中一年级的数学素养，并且了解信息学中递归、深搜算法的基本实现方式，能理解一般的递归程序。

1、排列和组合的定义
(1)排列的定义
从n个不同元素中，选出m个元素按照一定顺序排成一列，叫做从n个不同元素中取出m个元素的一个排列。

(2)排列数的定义
从n个元素中选出m个元素的所有排列的个数，叫做从n个不同元素中取出m个元素的排列数。

(3)全排列的定义
当n=m时所有的排列情况叫做全排列。

(4)组合的定义
从n个不同元素中，选出m个元素并成一组，叫做从n个不同元素中取出m个元素的一个组合。

(5)组合数的定义
从n个元素中选出m个元素的所有组合的个数，叫做从n个不同元素中取出m个元素的组合数。

(6)排列&组合的区别
通俗地说，组合不分顺序，而排列分顺序，也就是说，对于数列1,2，有以下两种排列：1,2和2,1，但是仅有一种组合1,2或2，1.


## 2、排列&组合的公式
### (1)关于排列的公式
从n个不同元素中，选出m个元素的排列数，数学表示为：
<img width="38" alt="444" src="https://github.com/mcoblackmore/liuchanghui.github.io/assets/49425642/30148757-8253-44a9-8e5a-f4ca2130c4cf">
计算公式如下：
<img width="542" alt="555" src="https://github.com/mcoblackmore/liuchanghui.github.io/assets/49425642/96694c32-1239-4345-9f65-d3cead214425">

### (2)关于组合的公式
从n个不同元素中，选出m个元素的组合数，数学表示为：
<img width="50" alt="666" src="https://github.com/mcoblackmore/liuchanghui.github.io/assets/49425642/6106aadc-4192-460f-a8f9-6c63e0a02b9f">

计算公式如下：
<img width="356" alt="777" src="https://github.com/mcoblackmore/liuchanghui.github.io/assets/49425642/176a2fdc-08ba-46bf-9006-51889cab75e5">

(3)关于全排列的公式
某个数列的全排列数f(n)，计算公式如下：

<img width="104" alt="888" src="https://github.com/mcoblackmore/liuchanghui.github.io/assets/49425642/bbc41a76-6e16-44e2-9586-1a2f1cac7f11">

3、全排列的求法
例题：生成全排列（深搜基础题）
给定n,生成1−n的全排列。

我们考虑用递归来解决全排列问题：
递归出口是当x==n+1地时候，绝对不能仅仅等于n！！

我们的递归部分使用标记数组和数列数组实现，具体实现方法可以参照下图：
![999](https://github.com/mcoblackmore/liuchanghui.github.io/assets/49425642/0eed8b1b-3fb5-43da-9dc8-3f449cb22ceb)

我们递归的过程大体是以下的思路:

三个数位可能出现1-3每个数，所以我们使用递归算法求解的时候，先圈定这一个值，然后继续下搜，遍历完这“一条链”的时候，就上回一个数位看看还有没有其他选择，这样就保证了解不重不漏。

```c++
#include<bits/stdc++.h>
using namespace std;
int n,a[20],v[20];
void dfs(int x){
    if(x==n+1)    {
        for(int i=1;i<n;i++)
            printf("%d ",a[i]);
        printf("%d\n",a[n]);
        return;
    }
    for(int i=1;i<=n;i++)
    {
        if(v[i]==0)
        {
            a[x]=i;
            v[i]=1;
            dfs(x+1);
            v[i]=0;
        }
    }
}
int main(){
    scanf("%d",&n);
    dfs(1);
    return 0;
}

```
