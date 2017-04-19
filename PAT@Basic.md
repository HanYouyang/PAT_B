# PAT@Basic真题详解
## 1001. 害死人不偿命的(3N+1)猜想
卡拉兹(Callatz)猜想：

对任何一个自然数n，如果它是偶数，那么把它砍掉一半；如果它是奇数，那么把(3n+1)砍掉一半。这样一直反复砍下去，最后一定在某一步得到n=1。卡拉兹在1950年的世界数学家大会上公布了这个猜想，传说当时耶鲁大学师生齐动员，拼命想证明这个貌似很傻很天真的命题，结果闹得学生们无心学业，一心只证(3n+1)，以至于有人说这是一个阴谋，卡拉兹是在蓄意延缓美国数学界教学与科研的进展……

我们今天的题目不是证明卡拉兹猜想，而是对给定的任一不超过1000的正整数n，简单地数一下，需要多少步（砍几下）才能得到n=1？

**输入格式**：每个测试输入包含1个测试用例，即给出自然数n的值。

**输出格式**：输出从n计算到1需要的步数。

**输入样例**：

```
3
```
**输出样例**：

```
5
```

```c++
#include <iostream>
using namespace std;
int main() {
    int n;
    cin >> n;
    int count = 0;
    while (n != 1) {
        if (n % 2 != 0) {
            n = 3 * n + 1;
        }
        n = n / 2;
        count++;
    }
    cout << count;
    return 0;
}
```

1. if(n%2) == if(n&1),都可以用来直接判断是否为奇数，不用非得计算出来之后判断
2. ++count可以运算的更快
3. 注意cout之后是否需要endl;
4. scanf("%d",&a)跟地址有关，printf("%d",a)直接出
5. 如果数特别大，可以用long long n2 = n得到一个长长型数，以进行乘法中的*3
6. 用scanf和printf要在CPP中添加<cstdio>
7. 按位与运算通常用来对某些位清0或保留某些位。例如把a 的高八位清 0 ， 保留低八位， 可作 a&255 运算 ( 255 的二进制数为0000000011111111)。 


## 1002. 写出这个数 
读入一个自然数n，计算其各位数字之和，用汉语拼音写出和的每一位数字。

**输入格式**：每个测试输入包含1个测试用例，即给出自然数n的值。这里保证n小于10的100次方。

**输出格式**：在一行内输出n的各位数字之和的每一位，拼音数字间有1 空格，但一行中最后一个拼音数字后没有空格。

**输入样例**：


``` c++
1234567890987654321123456789
```

**输出样例**:

```c++
yi san wu
```


```c++
#include <iostream>
using namespace std;
int main() {
    string chinese[10] = {"ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu"};
    string s;
    cin >> s;
    int len = s.length();
    int sum = 0;
    for (int i = 0; i < len; i++) {
        sum = s[i] - '0' + sum;
    }
    int *a = new int [len];
    int t = 0;
    if (sum == 0) {
        cout << chinese[0];
        return 0;
    }
    while (sum != 0) {
        a[t++] = sum % 10;
        sum = sum / 10;
    }
    for (int i = t - 1; i >= 0; i--) {
        cout << chinese[a[i]];
        if(i != 0)
            printf(" ");
    }
    delete [] a;
    return 0;
}

```
1. 必须要使用string,并且学会调用每个数位可以用string[i],通过与字符'0'相减可以得到各个数位的值
2. 注意1中相减必须是与**单引号**的0相减
3. 对待这种“转换”的问题，首先想到能否用什么样的数组进行存储对应的内容
4. （动态规划）建立数组，再用数组存储每一位的值并将这个值对应到注意3中建立的数组
5. 注意到必须有特例0也得对应上，因为要跟while中的运算条件区分开
6. 逆向顺序输出的时候可以**倒着循环减**
7. 下面用了一个值得注意的情况是即使规定了10的100次方，总数值不会超过9*100，也就是三位数，所以空格的要求就是两种情况依次打出来就好了，连循环都不要

```c++
#include<iostream>
#include<string>
int main()
{
	std::string num, n;
	int sum = 0;
	std::string zs[10] = { "ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu" };
	std::cin >> num;
	for (int i = 0; i < num.size(); ++i)
	{
		sum += (num[i] - '0');
	}
	if (sum >= 100)
	{
		std::cout << zs[sum / 100] << ' ';
		std::cout << zs[sum % 100 / 10] << ' ';
		std::cout << zs[sum % 10];
	}
	else if (sum >= 10)
	{
		std::cout << zs[sum % 100 / 10] << ' ';
		std::cout << zs[sum % 10];
	}
	else
		std::cout << zs[sum % 10];
	return 0;
}

```
8.也可以用getchar()得到每个字符直接加和各位数
9.由此逆向输出的时候，想到除了第一个是不带空格，则分成后面的都在字符前带空格，用"%s"
10.本方法没有用到动态规划，而且运行中需要具体输入进去，实际可行程度未知，**仅做思路参考**，标准答案由前两个组成

```c++
#include<stdio.h>
int main()
{
    char a;
    int i=0;
    int sum=0;
    int ans[110];
    char str[][5]={"ling","yi","er","san","si","wu","liu",
    "qi","ba","jiu"};
    while((a=getchar())!='\n'){
        sum+=a-'0';
    }
    for(i=0;sum!=0;i++){
        ans[i]=sum%10;
        sum/=10;
    }
    int begin=i-1;
    for(i=begin;i>=0;i--){
        if(i==begin)
        printf("%s",str[ans[i]]);
        else
        printf(" %s",str[ans[i]]);
    }
    printf("\n");
    
    
    return 0;
}
```


