title: 第六届蓝桥杯大赛软件类省赛 C/C++ 大学B 组
author: Monody12
tags:
  - Blue Bridge Cup
  - excel
  - enumeration
  - dfs
  - permutation
  - backtracking
categories:
  - algorithm
date: 2021-04-13 21:20:00
---
## <a name="目录">目录</a>
1. [奖券数目](#奖券数目)
2. [星系炸弹](#星系炸弹)
3. [三羊献瑞](#三羊献瑞)
4. [格子中输出](#格子中输出)
5. [九数组分数](#九数组分数)
6. [加法变乘法](#加法变乘法)
7. [牌型种数](#牌型种数)
8. [移动距离](#移动距离)
9. [垒骰子](#垒骰子)
10. [生命之树](#生命之树)

## <a name="奖券数目">1.奖券数目 (模拟)</a>

有些人很迷信数字，比如带“4”的数字，认为和“死”谐音，就觉得不吉利。
虽然这些说法纯属无稽之谈，但有时还要迎合大众的需求。某抽奖活动的奖券号码是5位数（10000-99999），要求其中不要出现带“4”的号码，主办单位请你计算一下，如果任何两张奖券不重号，最多可发出奖券多少张。

请提交该数字（一个整数），不要写任何多余的内容或说明性文字。


### 分析
依次检测每个奖票的号码是否合法(每位数都不含4)。

```c++
#include <iostream>
#include <cstdio>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

bool check(int n) {
    while (n) {
        if (n % 10 == 4)
            return false;
        n /= 10;
    }
    return true;
}

int main() {
    //input;
    int ans = 0;
    for (int i = 10000; i < 100000; ++i) {
        if (check(i))
            ++ans;
    }
    printf("%d", ans);  //输出 52488
    return 0;
}

```
输出答案：52488

[回到顶端](#目录)

-------
## <a name="星系炸弹">2.星系炸弹 （excel）</a>

在X星系的广袤空间中漂浮着许多X星人造“炸弹”，用来作为宇宙中的路标。
每个炸弹都可以设定多少天之后爆炸。
比如：阿尔法炸弹2015年1月1日放置，定时为15天，则它在2015年1月16日爆炸。
有一个贝塔炸弹，2014年11月9日放置，定时为1000天，请你计算它爆炸的准确日期。

请填写该日期，格式为 yyyy-mm-dd  即4位年份2位月份2位日期。比如：2015-02-19
请严格按照格式书写。不能出现其它文字或符号。

### 分析
使用excel的日期计算功能。

![星系炸弹_EXCEL_.jpg](https://i.loli.net/2021/04/14/GmUaxDtP19T8uhI.jpg)

输出答案：2017-8-5

[回到顶端](#目录)

-------
## <a name="三羊献瑞">3.三羊献瑞 （排列 枚举）</a>

观察下面的加法算式：
```
      祥 瑞 生 辉
  +   三 羊 献 瑞
-------------------
   三 羊 生 瑞 气
  ```


其中，相同的汉字代表相同的数字，不同的汉字代表不同的数字。

请你填写“三羊献瑞”所代表的4位数字（答案唯一），不要填写任何多余内容。

### 分析
根据等式可以观察出，这是一个加法，得到的和比最大的加数多了一位，那么这一位必定为1（也就是‘三’这个字为1），由这又可以推出‘祥’为9。推出了两个字，现在就剩下6个未知量了，从剩下的8个数去暴力枚举然后判断就行。

### 实现
我设的未知数如下：  
```
  瑞   生   辉   羊   献   气
a[1] a[2] a[3] a[4] a[5] a[6]
   ```



```c++
#include <iostream>
#include <cstdio>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

int a[7], vis[10];


void check() {  //检测表达式是否正确
    int xrsh = 1000 * 9 + 100 * a[1] + 10 * a[2] + a[3];
    int syxr = 1000 + 100 * a[4] + 10 * a[5] + a[1];
    int sysrq = 10000 + 1000 * a[4] + 100 * a[2] + 10 * a[1] + a[6];
    if (xrsh + syxr == sysrq) {
        printf("三羊献瑞 ：%d", syxr);
    }
}

void dfs(int step, int n) {  //在10个数中枚举这些未知数
    if (step > n) {
        check();
        return;
    }
    for (int i = 0; i < 9; ++i) {
        if (!vis[i]) {
            a[step] = i;
            vis[i] = 1;
            dfs(step + 1, n);
            vis[i] = 0;
        }
    }
}

int main() {
    //input;
    vis[1] = 1, vis[9] = 1; //由加法特性得  ’三‘=1  '祥'=9
    dfs(1, 6);
    return 0;
}

```

输出答案：`三羊献瑞 ：1085`  
填入答案：1085


[回到顶端](#目录)

-------
## <a name="格子中输出">4.格子中输出</a>

[回到顶端](#目录)

-------
## <a name="九数组分数">5.九数组分数 （递归 回溯）</a>
1,2,3...9 这九个数字组成一个分数，其值恰好为1/3，如何组法？

下面的程序实现了该功能，请填写划线部分缺失的代码。
```C++
#include <stdio.h>

void test(int x[])
{
	int a = x[0]*1000 + x[1]*100 + x[2]*10 + x[3];
	int b = x[4]*10000 + x[5]*1000 + x[6]*100 + x[7]*10 + x[8];
	
	if(a*3==b) printf("%d / %d\n", a, b);
}

void f(int x[], int k)
{
	int i,t;
	if(k>=9){
		test(x);
		return;
	}
	
	for(i=k; i<9; i++){
		{t=x[k]; x[k]=x[i]; x[i]=t;}
		f(x,k+1);
		_____________________________________________ // 填空处
	}
}
	
int main()
{
	int x[] = {1,2,3,4,5,6,7,8,9};
	f(x,0);	
	return 0;
}

```

### 分析

浅显易懂的递归回溯。  
dfs函数递归上方交换了x[i]和x[k]的值，所以回溯将其的值调换回来。

填入答案：`{t=x[i];x[i]=x[k];x[k]=t;}`


注意：只填写缺少的内容，不要书写任何题面已有代码或说明性文字。

[回到顶端](#目录)

-------
## <a name="加法变乘法">6.加法变乘法</a>
我们都知道：1+2+3+ ... + 49 = 1225
现在要求你把其中两个不相邻的加号变成乘号，使得结果为2015

比如：
1+2+3+...+10*11+12+...+27*28+29+...+49 = 2015
就是符合要求的答案。

请你寻找另外一个可能的答案，并把位置靠前的那个乘号左边的数字提交（对于示例，就是提交10）。

注意：需要你提交的是一个整数，不要填写任何多余的内容。

### 分析
题目要我们把连续相加的表达式中的其中两个加号改为乘号，这样表达式的计算值就会提升，题目要我们求提升2015 - 1225时应该修改哪两个加号。  
实现：对应做差即可。

```c++
#include <cstdio>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

int main() {
    //input;
    for (int i = 1; i <= 49; ++i)
        for (int j = i + 2; j <= 49; ++j)
            if (i * (i + 1) + j * (j + 1) - (i + (i + 1) + j + (j + 1)) == 2015 - 1225)  //对应作差：差值为将加号改为乘号等式能提升的数值
                printf("%d\n", i);
    return 0;
}

```
输出答案：  
10  
16

填入答案：16

[回到顶端](#目录)

-------
## <a name="牌型种数">7.牌型种数</a>

[回到顶端](#目录)

-------
## <a name="移动距离">8.移动距离</a>
X星球居民小区的楼房全是一样的，并且按矩阵样式排列。其楼房的编号为1,2,3...
当排满一行时，从下一行相邻的楼往反方向排号。
比如：当小区排号宽度为6时，开始情形如下：
```
1  2  3  4  5  6
12 11 10 9  8  7
13 14 15 .....
```
我们的问题是：已知了两个楼号m和n，需要求出它们之间的最短移动距离（不能斜线方向移动）

输入为3个整数w m n，空格分开，都在1到10000范围内  
w为排号宽度，m,n为待计算的楼号。  
要求输出一个整数，表示m n 两楼间最短移动距离。  

例如：  
用户输入：  
6 8 2  
则，程序应该输出：  
4

再例如：  
用户输入：  
4 7 20  
则，程序应该输出：  
5

### 分析
数据范围不大，也可以模拟，但是这样太慢了，太水了，所以用数学思维吧。    
实现：观察发现这是一个蛇形矩阵，每个偶数行相较于正常矩阵来说是相反的。那么计算距离的时候可以当成一个正常矩阵来计算。根据行数是否为偶，再进行特殊处理。  
二维矩阵计算行列小技巧：将起始值由1变为0。这样就可以快速的使用/和%来得到正常二维矩阵的行列下标了


```C++
#include <cmath>
#include <cstdio>

using namespace std;
#define input freopen("C:\\Users\\Administrator\\Desktop\\a.txt","r",stdin)

int main() {
    //input;
    int m, a, b;  //m行,a、b为两个楼号
    scanf("%d%d%d", &m, &a, &b);
    --a, --b;  //楼号都减一利于计算他们的横纵坐标
    int ax = a / m, ay = a % m;
    if (ax % 2 == 0)  //如果为偶数行，则列数需要翻转
        ay = m - ay - 1;
    int bx = b / m, by = b % m;
    if (bx % 2 == 0)  //如果为偶数行，则列数需要翻转
        by = m - by - 1;
    printf("%d", abs(ax - bx) + abs(ay - by));
    return 0;
}

```

[回到顶端](#目录)

-------
## <a name="垒骰子">9.垒骰子</a>

[回到顶端](#目录)

-------
## <a name="生命之树">10.生命之树</a>

[回到顶端](#目录)

-------

感谢[Nice night的题解博客](https://blog.csdn.net/firewater23/article/details/107923183)给我写这些题时提供了帮助。