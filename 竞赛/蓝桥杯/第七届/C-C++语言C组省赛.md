[toc]

## 报纸页数（3 分）

X 星球日报和我们地球的城市早报是一样的，
都是一些单独的纸张叠在一起而已。每张纸印有 4 版。

比如，某张报纸包含的 4 页是：5，6，11，12。
可以确定它应该是最上边的第 2 张报纸。

我们在太空中捡到了一张 X 星球的报纸，4 个页码分别是：
1125，1126，1727，1728。

请你计算这份报纸一共多少页（也就是最大页码，并不是用了几张纸哦）？

***思路：***

> 那个第二页忽视（不懂）

设最后一页为 x。

- 第一张：1，2，x-1，x
- 第二张：3，4，x-3，x-2
- ...
- 第 563 张：1125，1126，x-1125，x-1124

所以 x - 1125 = 1727。
x = 2852

```cpp
#include<cstdio>
using namespace std;

int main(){
	
	printf("%d\n", 1125+1727);

	return 0;
}
```

## 煤球数目（5 分）

有一堆煤球，堆成三角棱锥形。具体：
第一层放 1 个，
第二层 3 个（排列成三角形），
第三层 6 个（排列成三角形），
第四层 10 个（排列成三角形），
....
如果一共有 100 层，共有多少个煤球？

***思路：***

每层规律满足下面公式：

- A<sub>n</sub> = 1 (n = 1)
- A<sub>n</sub> = A<sub>n-1</sub> + n (n &gt; 1)

注意是问所有层的总数。

```cpp
#include<cstdio>
using namespace std;

int main(){
	
	int an = 1;
	int sum = an;
	for(int i = 2; i <= 100; i ++){
		an += i;
		sum += an;
	}
	printf("%d\n", sum);
	
	return 0;
}
```

## 平方怪圈（7 分）

如果把一个正整数的每一位都平方后再求和，得到一个新的正整数。
对新产生的正整数再做同样的处理。

如此一来，你会发现，不管开始取的是什么数字，
最终如果不是落入 1，就是落入同一个循环圈。

请写出这个循环圈中最大的那个数字。

请填写该最大数字。

***思路：***

依次对出现过的数字进行标记数字。
由于我们的初始值只要不为 1，就一定会进入循环圈中，那么存在我们所举出的数本身并不在循环体内。
所以需要从任意一个数开始运算，直到某个数出现了三次（也就是刚开始循环一圈，然后自己循环一圈）停止。
统计出现两次或两次以上的数字中最大的数即可。

```cpp
#include<cstdio>
#include<cstring>
using namespace std;

const int maxn = 10000 + 5;
int number[maxn];

int main(){
	
	memset(number, 0, sizeof(number));
	
	int a = 2;
	int temp, sum;
	while(1){
		temp = a;
		sum = 0;
		while(temp){
			sum += (temp % 10) * (temp % 10);
			temp /= 10;
		}
		
		if(number[sum] == 2){
			break;
		}
		number[sum] ++;
		a = sum;
	}
	
	int ans = 0;
	for(int i = 0; i < maxn; i ++){
		if(number[i] == 2){
			ans < i? ans = i: 0;
		}
	}
	printf("%d\n", ans);
	
	return 0;
}
```

## 打印方格（11 分）

小明想在控制台上输出 mxn 个方格。
比如 10x4 的，输出的样子是：

```
+---+---+---+---+---+---+---+---+---+---+
|   |   |   |   |   |   |   |   |   |   |
+---+---+---+---+---+---+---+---+---+---+
|   |   |   |   |   |   |   |   |   |   |
+---+---+---+---+---+---+---+---+---+---+
|   |   |   |   |   |   |   |   |   |   |
+---+---+---+---+---+---+---+---+---+---+
|   |   |   |   |   |   |   |   |   |   |
+---+---+---+---+---+---+---+---+---+---+
```

以下是小明写的程序，请你分析其流程，填写划线部分缺少的代码。

```c
#include <stdio.h>

//打印m列，n行的方格 
void f(int m, int n)
{
	int row;
	int col;
	
	for(row=0; row<n; row++){
		for(col=0; col<m; col++) printf("+---");
		printf("+\n");
		for(col=0; col<m; col++) printf("|   ");
		printf("|\n");		
	}
	
	printf("+");
	_____________________________;   //填空
	printf("\n");
}

int main()
{
	f(10,4);
	return 0;
}
```

**答案：`for(col=0; col<m; col++) printf("---+")`**

## 快速排序（13 分）

排序在各种场合经常被用到。
快速排序是十分常用的高效率的算法。

其思想是：先选一个 "标尺"，
用它把整个队列过一遍筛子，
以保证：其左边的元素都不大于它，其右边的元素都不小于它。

这样，排序问题就被分割为两个子区间。
再分别对子区间排序就可以了。

下面的代码是一种实现，请分析并填写划线部分缺少的代码。

```c
#include <stdio.h>

void swap(int a[], int i, int j)
{
    int t = a[i];
    a[i] = a[j];
    a[j] = t;
}

int partition(int a[], int p, int r)
{
    int i = p;
    int j = r + 1;
    int x = a[p];
    while(1){
        while(i<r && a[++i]<x);
        while(a[--j]>x);
        if(i>=j) break;
        swap(a,i,j);
    }
    //______________________;

    return j;
}

void quicksort(int a[], int p, int r)
{
    if(p<r){
        int q = partition(a,p,r);
        quicksort(a,p,q-1);
        quicksort(a,q+1,r);
    }
}
    
int main()
{
    int i;
    int a[] = {5,13,6,24,2,8,19,27,6,12,1,17};
    int N = 12;
    
    quicksort(a, 0, N-1);
    
    for(i=0; i<N; i++) printf("%d ", a[i]);
    printf("\n");
    
    return 0;
}
```

**答案：`swqp(a, p, j)`**

## 凑算式（15 分）

```
    B   DEF
A + — + ——— = 10
    C   GHI
```

这个算式中 A~I 代表 1~9 的数字，不同的字母代表不同的数字。

比如：
6+8/3+952/714 就是一种解法，
5+3/1+972/486 是另一种解法。

这个算式一共有多少种解法？

***思路：***

不用想太多，直接枚举各个字母可能填的数字即可。

注意一个字母对应一个数字，所以不要选重复数字，使用标记数组标记。
如果之前有字母使用了数字直接跳过，否则可以使用这个数字，标记并继续循环。
使用过这个数字后，记得要放回数字。

为了避免除法，可以将等式变换成下面的形式。
A&sdot;C&sdot;GHI + B&sdot;GHI + DEF&sdot;C = 10&sdot;C&sdot;GHI

（其他代码实现请参照 B 组同一道题，这里只给出全排列代码。）

```cpp
#include<cstdio>
#include<algorithm>
using namespace std;

int number[10];
int cnt = 0;

bool check(){
	if((number[0]*number[2]*(100*number[6]+10*number[7]+number[8])
		+ number[1]*(100*number[6]+10*number[7]+number[8])
		+ (100*number[3]+10*number[4]+number[5])*number[2])
		== (10*number[2]*(100*number[6]+10*number[7]+number[8]))){
			cnt ++;		
			return true;
		}
	return false;
}

int main(){
	
	for(int i = 0; i < 9; i ++){
		number[i] = i+1;
	}
	
	while(next_permutation(number, number+9)){

		check();
	}
	
	printf("%d\n", cnt);
	
	return 0;
}
```

## 寒假作业（19 分）

现在小学的数学题目也不是那么好玩的。
看看这个寒假作业：

```
□ + □ = □
□ - □ = □
□ × □ = □
□ ÷ □ = □
```
   
每个方块代表 1~13 中的某一个数字，但不能重复。
比如：

```
6  + 7 = 13
9  - 8 = 1
3  * 4 = 12
10 / 2 = 5
```

以及：

```
7  + 6 = 13
9  - 8 = 1
3  * 4 = 12
10 / 2 = 5
```

就算两种解法。（加法，乘法交换律后算不同的方案）
 
你一共找到了多少种方案？

***思路：***

- DFS。
- 每当生成三个数字时就进行判断，减小数据量。
- 先对除法和乘法进行判断，减小数据量。

```cpp
#include<cstdio>
#include<cstring>
using namespace std;

bool used[14];
int number[12];
int count = 0;

void dfs(int index){
	
	if(index == 3){
		if(!((number[0] % number[1] == 0) && (number[0] / number[1] == number[2]))){
			return ;
		}
	}
	if(index == 6){
		if(!(number[3] * number[4] == number[5])){
			return ;
		}
	}
	if(index == 9){
		if(!(number[6] - number[7] == number[8])){
			return; 
		}
	}
	if(index == 12){
		if(number[9] + number[10] == number[11]){
			count ++;
		}
		return ;
	}
	
	for(int i = 1; i <= 13; i ++){
		if(!used[i]){
			used[i] = true;
			number[index] = i;
			dfs(index + 1);
			used[i] = false;
		}
	}
}

int main(){
	
	memset(used, false, sizeof(used));
	dfs(0);
	printf("%d\n", count);
	
	return 0;
}
```

## 冰雹数（21 分）

任意给定一个正整数 N，
如果是偶数，执行：N / 2
如果是奇数，执行：N * 3 + 1

生成的新的数字再执行同样的动作，循环往复。

通过观察发现，这个数字会一会儿上升到很高，
一会儿又降落下来。
就这样起起落落的，但最终必会落到 "1"。
这有点像小冰雹粒子在冰雹云中翻滚增长的样子。

比如 N = 9
9,28,14,7,22,11,34,17,52,26,13,40,20,10,5,16,8,4,2,1
可以看到，N = 9 的时候，这个 "小冰雹" 最高冲到了 52 这个高度。

***输入格式：***

一个正整数 N（N &lt; 1000000）

***输出格式：***

一个正整数，表示不大于 N 的数字，经过冰雹数变换过程中，最高冲到了多少。

***样例输入 1：***

10

***样例输出 1：***

52

***样例输入 2：***

100

***样例输出 2：***

9232

***思路：***

2n + 1 问题换了个描述而已。
按照题目描述，循环到该数字变为 1 位置，记录变化过程中数字的最大值即可。
注意是不大于 N。

```cpp
#include<cstdio>
using namespace std;

int main(){
	
	int n;
	scanf("%d", &n);
	
	int ans = n;
	for(int i = 1; i <= n ; i ++){
		int temp = i;
		while(temp != 1){
			if(temp % 2){
				temp = temp * 3 + 1;
			}
			else{
				temp /= 2;
			}
			ans < temp? ans = temp: 0;
		}
	}
	
	printf("%d\n", ans);
	
	return 0;
}
```

## 卡片换位（25 分）

你玩过华容道的游戏吗？
这是个类似的，但更简单的游戏。
看下面 3 x 2 的格子

```
+---+---+---+
| A | * | * |
+---+---+---+
| B |   | * |
+---+---+---+
```

在其中放 5 张牌，其中 A 代表关羽，B 代表张飞，* 代表士兵。
还有一个格子是空着的。

你可以把一张牌移动到相邻的空格中去（对角不算相邻）。
游戏的目标是：关羽和张飞交换位置，其它的牌随便在哪里都可以。

***输入格式：***

输入两行 6 个字符表示当前的局面

***输出格式：***

一个整数，表示最少多少步，才能把 AB 换位。（其它牌位置随意）

***样例输入 1：***

```
* A
**B
```

***样例输出 1：***

17

***样例输入 2：***

```
A B
***
```

***样例输出 2：***

12

***思路：***

- 由于 2\*3 的总排列数据量较小，可直接使用 `map` 标记每次走过的步。
- 使用 BFS。

BFS

```cpp
#include<iostream>
#include<map>
#include<queue>
#include<string>
using namespace std;

struct node{
	int x, y;
}a, b, s;
map<string, int> m;
queue<string> q;

int dirx[] = {0, 1, 0, -1};
int diry[] = {1, 0, -1, 0};

bool check(string str){
	bool flag1 = false, flag2 = false;
	
	for(int i = 0; i < str.length(); i ++){
		if(str[i] == 'A' && (b.x == i / 3) && (b.y == i % 3)){
			flag1 = true;
		}
		if(str[i] == 'B' && (a.x == i / 3) && (a.y == i % 3)){
			flag2 = true;
		}
	}
	
	return flag1 && flag2;
}

int ans;
void bfs(){
	
	struct node pos;
	while(q.size() > 0){
		string str = q.front();
		string temp;
		q.pop();
		
		for(int i = 0; i < str.length(); i ++){
			if(str[i] == ' '){
				pos.x = i / 3;
				pos.y = i % 3;
			}
		}
		
		for(int i = 0; i < 4; i ++){
			temp = string(str);
			int xx = pos.x + dirx[i];
			int yy = pos.y + diry[i];
			
			if(xx >= 0 && xx < 2 && yy >= 0 && yy < 3){
				char c = temp[xx * 3 + yy];
				temp[xx * 3 + yy] = temp[pos.x * 3 + pos.y];
				temp[pos.x * 3 + pos.y] = c;
				
				if(m.find(temp) != m.end()){
					continue;
				}
				
				m[temp] = m[str] + 1;
				if(check(temp)){
					ans = m[str] + 1;
					return ;
				}
				
				q.push(temp);
			}
		}
	}
}

int main(){
	
	string temp1, temp2, temp;
	getline(cin, temp1);
	getline(cin, temp2);
	temp = temp1 + temp2;
	
	for(int i = 0; i < temp.length(); i ++){
		if(temp[i] == 'A'){
			a.x = i / 3;
			a.y = i % 3;
		}
		else if(temp[i] == 'B'){
			b.x = i / 3;
			b.y = i % 3;
		}
		else if(temp[i] == ' '){
			s.x = i / 3;
			s.y = i % 3;
		}
	}
	
	m[temp] = 0;
	q.push(temp);
	
	bfs();
	
	printf("%d\n", ans);
	
	return 0;
}
```

## 密码脱落（31 分）

X 星球的考古学家发现了一批古代留下来的密码。
这些密码是由 A、B、C、D 四种植物的种子串成的序列。
仔细分析发现，这些密码串当初应该是前后对称的（也就是我们说的镜像串）。
由于年代久远，其中许多种子脱落了，因而可能会失去镜像的特征。

你的任务是：
给定一个现在看到的密码串，计算一下从当初的状态，它要至少脱落多少个种子，才可能会变成现在的样子。

***输入格式：***

输入一行，表示现在看到的密码串。（长度不大于 1000）

***输出格式：***

输出一个正整数，表示至少脱落了多少个种子。

***样例输入 1：***

ABCBA

***样例输出 1：***

0

***样例输入 2：***

ABDCDCBABC

***样例输出 2：***

3

***思路：***

贪心。

- 用两个指针指向字符串首尾。
- 当两侧相同时，两个指针向内移动一个位置。
- 当两侧不同时，
    - 分别从两端找与另一端相同的位置。
    - 取小者记录，指针更新。

```cpp
#include<cstdio>
#include<cstring>
using namespace std;

const int maxn = 1000 + 5;

int main(){
	
	char str[maxn];
	gets(str);
	
	int i = 0, j = strlen(str) - 1;
	int ans = 0;
	while(i <= j){
		if(str[i] == str[j]){
			i ++, j --;
		}
		else{
			int posi = 0, posj = 0;
			while(i + posi <= j && str[i + posi] != str[j]){
				++ posi;
			}
			while(j - posj >= i && str[j - posj] != str[i]){
				++ posj;
			}
			if(posi > posj){
				ans += posj;
				i ++;
				j -= posj + 1;
			}
			else{
				ans += posi;
				i += posi + 1;
				j --;
			}
		}
	}
	
	printf("%d\n", ans);
	
	return 0;
}
```
