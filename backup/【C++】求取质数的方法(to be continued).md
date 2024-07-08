# 求取质数的简单方法

质数是自然数中非常特别的数，在C++中经常考察有关质数的相关算法。我们列举出一些求取质数的方法如下：

## 1.最简单的试除法：

试除法是根据质数的定义来判定是否质数：一个数n如果不能被2~n-1之间的任何数整除，那么这个数n肯定是质数，如下代码就是求取100-1000之间质数的一个示例，它还计算出了区间质数的个数：

```C++

#include <iostream>
#include <cmath>
using namespace std; 
int main() {
	int sum = 0;
    for (int num = 100; num <= 1000; num++) {
        bool isPrime = true;
        for (int i = 2; i < num; i++) {
            if (num % i == 0) {
                isPrime = false;
                break;
            }
        }
        if (isPrime) {
            cout << num << " ";
            sum++;
        }
    }
    cout<<endl<<"质数个数是："<<sum;
    return 0;
}
```
当然，我也可以用while循环来实现试除法，代码如下：
```C++

int main(){
	int sum=0;
	for(int x=100;x<=1000;x++){
		int i=2;
		while(x%i!=0)		i++;
		if(i==x){
			cout<<x<<" ";sum++;
		}
	}
	cout<<endl<<sum;
}
```
## 2.埃拉托斯特尼筛法（筛法）

埃拉托斯特尼筛法确实基于这样一个重要的观察：每个大于1的自然数要么是质数，要么是质数的倍数。这个原理是该算法的核心思想。埃拉托斯特尼筛法的基本原理如下：

1. 列出要筛选的范围内的所有数字，从2开始。
2. 找到列表中的第一个未被标记的数（最开始是2），这个数就是一个质数。
3. 将这个质数的所有倍数都标记为合数（非质数）。
4. 重复步骤2和3，直到处理完列表中的所有数字。
5. 最后，所有未被标记的数就是质数。

**这种方法特别适合找出一定范围内的所有质数，尤其是当这个范围相对较大时。**

代码如下：

```C++
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n = 1000;  // 假设我们要找出1000以内的质数
    vector<bool> isPrime(n + 1, true);
    isPrime[0] = isPrime[1] = false;

    for (int i = 2; i * i <= n; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= n; j += i) {
                isPrime[j] = false;
            }
        }
    }

    for (int i = 2; i <= n; i++) {
        if (isPrime[i]) {
            cout << i << " ";
        }
    }
    return 0;
}

```