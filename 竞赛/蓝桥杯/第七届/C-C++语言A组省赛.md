[toc]

## 网友年龄（3 分）

某君新认识一网友。
当问及年龄时，他的网友说：
"我的年龄是个 2 位数，我比儿子大 27 岁,
如果把我的年龄的两位数字交换位置，刚好就是我儿子的年龄。"

请你计算：网友的年龄一共有多少种可能情况？

提示：30 岁就是其中一种可能哦.

***思路：***

枚举两位数中符合条件的即可。

```cpp
#include<cstdio>
using namespace std;

int main(){

	int cnt = 0;
	for(int i = 10; i < 100; i ++){
		int a = i / 10;
		int b = i % 10;
		
		if(i - (b*10 + a) == 27){
			cnt ++;
		}
	}
	printf("%d\n", cnt);
	
	return 0;
}
```

## 生日蜡烛（5 分）

某君从某年开始每年都举办一次生日 party，并且每次都要吹熄与年龄相同根数的蜡烛。
现在算起来，他一共吹熄了 236 根蜡烛。
请问，他从多少岁开始过生日 party 的？

请填写他开始过生日 party 的年龄数。

***思路：***

从 1 岁开始枚举年龄，一旦超过 236 就从 2 岁开始枚举，以此类推。

```cpp
#include<cstdio>
using namespace std;

int main(){
	
	int sum;
	for(int i = 1; i < 236; i ++){
		sum = 0;
		for(int j = i; j < 236; j ++){
			sum += j;
			if(sum == 236){
				printf("%d\n", i);
				break;
			}
			else if(sum > 236){
				break;
			}
		} 
	}
	
	return 0;
}
```

## 方格填数（11 分）

如下的 10 个格子填入 0~9 的数字。

![方格填数](http://oxnec2zdn.bkt.clouddn.com/lanqiaobeidiqijie/fanggetianshu.jpg)

要求：连续的两个数字不能相邻。
（左右、上下、对角都算相邻）

一共有多少种可能的填数方案？

***思路：***

将方格进行标号。

```
    +---+---+---+
    | 0 | 1 | 2 |
+---+---+---+---+
| 3 | 4 | 5 | 6 |
+---+---+---+---+
| 7 | 8 | 9 |
+---+---+---+
```

同凑算式。
枚举总共有多少种排列，分别填进格中，对条件进行判断。
（这里假设数字不可重复。）

（STL）

```cpp
#include<cstdio>
#include<algorithm>
#include<cmath>
using namespace std;

int number[10];
int cnt = 0;
int block[4][3];

bool valid(int x, int y){
	if((x == 0 && y == 0) || (x == 3 && y == 2)){
		return false;
	}
	else if(x < 0 || x > 3 || y < 0 || y > 2){
		return false;
	}
	return true;
}

bool check(){
	
	for(int i = 0; i < 4; i ++){
		for(int j = 0; j < 3; j ++){
			if(valid(i, j)){
				for(int xx = -1; xx <= 1; xx ++){
					for(int yy = -1; yy <= 1; yy ++){
						if(xx == 0 && yy == 0){
							continue;
						}
						if(valid(i + xx, j + yy) && abs(block[i][j] - block[i+xx][j+yy]) == 1){
							return false;
						}
					}
				}
			}
		}
	}
	
	return true;
}

int main(){
	
	for(int i = 0; i <= 9; i ++){
		number[i] = i;
	}
	
	while(next_permutation(number, number+10)){
		int temp = 0;
		for(int i = 0; i < 4; i ++){
			for(int j = 0; j < 3; j ++){
				if(valid(i, j)){
					block[i][j] = number[temp ++];
				}
				
			}
		} 
		if(check()){
			cnt ++;
		}
	}
	
	printf("%d\n", cnt);
	
	return 0;
}
```

## 快速排序（9 分）

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

## 消除尾一（13 分）

下面的代码把一个整数的二进制表示的最右边的连续的1全部变成0
如果最后一位是0，则原数字保持不变。

如果采用代码中的测试数据，应该输出：

```
00000000000000000000000001100111   00000000000000000000000001100000
00000000000000000000000000001100   00000000000000000000000000001100
```

请仔细阅读程序，填写划线部分缺少的代码。

```c
#include <stdio.h>

void f(int x)
{
	int i;
	for(i=0; i<32; i++) printf("%d", (x>>(31-i))&1);
	printf("   ");
	
	x = _______________________;
	
	for(i=0; i<32; i++) printf("%d", (x>>(31-i))&1);
	printf("\n");	
}

int main()
{
	f(103);
	f(12);
	return 0;
}
```

**答案：`x&(x+1)`**

## 寒假作业（15 分）

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

## 剪邮票（19 分）

![剪邮票 1](http://oxnec2zdn.bkt.clouddn.com/lanqiaobeidiqijie/jianyoupiao1.jpg)

有 12 张连在一起的 12 生肖的邮票。
现在你要从中剪下 5 张来，要求必须是连着的。
（仅仅连接一个角不算相连）

![剪邮票 2](http://oxnec2zdn.bkt.clouddn.com/lanqiaobeidiqijie/jianyoupiao2.jpg)

![剪邮票 3](http://oxnec2zdn.bkt.clouddn.com/lanqiaobeidiqijie/jianyoupiao3.jpg)

如上图中，粉红色所示部分就是合格的剪取。

***思路：***

同方格填数。
这次只需要生成 1~12 中的五个数的排列即可。
为了防止重复，这次生成的时候只允许向比前一个数更大的数中挑选。

找出来后再进行一次 dfs，如果有数字未被访问，则不计数。

```cpp
#include<cstdio>
#include<cstring>
using namespace std;

int number[5];
bool visited[5];
int cnt = 0;
int xdir[] = {0, 1, 0, -1};
int ydir[] = {1, 0, -1, 0};

bool valid(int x, int y){
	if(x < 0 || x > 2 || y < 0 || y > 3){
		return false;
	}
	return true;
}


void check(int t){
	int x = (t - 1) / 4;
	int y = (t - 1) % 4;
	
	for(int i = 0; i < 4; i ++){
		int xx = x + xdir[i];
		int yy = y + ydir[i];
		if(valid(xx, yy)){
			int temp = xx * 4 + yy + 1;
			for(int j = 0; j < 5; j ++){
				if(!visited[j] && number[j] == temp){
					visited[j] = true;
					check(number[j]);
				}
			}
		}
	}
}

void dfs(int last, int index){
	
	if(index == 5){
		memset(visited, false, sizeof(visited));
		visited[0] = true;
		check(number[0]);
		for(int i = 0; i < 5; i ++){
			if(!visited[i]){
				return;
			}
		}
		cnt ++;
		return ;
	}
	
	for(int i = last + 1; i <= 12; i ++){
		number[index] = i;
		dfs(i, index + 1); 
	}
}

int main(){
	
	dfs(0, 0);
	printf("%d\n", cnt);
	
	return 0;
}
```

## 四平方和（21 分）

四平方和定理，又称为拉格朗日定理：
每个正整数都可以表示为至多 4 个正整数的平方和。
如果把 0 包括进去，就正好可以表示为 4 个数的平方和。

比如：
5 = 0&sup2; + 0&sup2; + 1&sup2; + 2&sup2;
7 = 1&sup2; + 1&sup2; + 1&sup2; + 2&sup2;

对于一个给定的正整数，可能存在多种平方和的表示法。
要求你对 4 个数排序：
0 &le; a &le; b &le; c &le; d
并对所有的可能表示法按 a，b，c，d 为联合主键升序排列，最后输出第一个表示法。

***输入格式：***

一个正整数 N（N &lt; 5000000）

***输出格式：***

输出 4 个非负整数，按从小到大排序，中间用空格分开。

***样例输入 1：***

5

***样例输出 1：***

0 0 1 2

***样例输入 2：***

12

***样例输出 2：***

0 2 2 2

***样例输入 3：***

773535

***样例输出 3：***

1 1 267 838

***思路：***

- 直接四层循环必然超时。
- 推导出 d&sup2;=n-a&sup2;-b&sup2;-c&sup2;，并对 a、b、c 进行遍历，三层循环对于第三个样例仍然很慢。
- 将四个数平方和看作两个数平方和加两个数平方和，对所有两个数平方和打表，并循环 a、b，若 n-a&sup2;-b&sup2; 为一个两个数的平方和，则枚举 c。

```cpp
#include<cstdio>
#include<cmath>
#include<cstring>
#include<set>
using namespace std;

const int maxn = 5000000 + 5;
int n;
bool hy[maxn];

void init(){
	memset(hy, false, sizeof(hy));
	
	for(int i = 0; i * i <= n; i ++){
		for(int j = 0; j * j <= (n - i*i); j ++){
			hy[i*i + j*j] = true;
		}
	}
}

int main(){

	scanf("%d", &n);
	init();
	
	for(int a = 0; a * a <= n; a ++){
		for(int b = 0; b * b <= (n - a*a); b ++){
			int temp = n - a*a - b*b;
			if(hy[temp]){
				for(int c = 0; c * c <= temp; c ++){
					int d = sqrt(temp - c*c);
					if(d*d == (temp - c*c)){
						printf("%d %d %d %d\n", a, b, c, d);
						return 0;
					}
				}
			}
		}
	}
	
	return 0;
}
```

## 密码脱落（25 分）

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

## 最大比例（29 分）

X 星球的某个大奖赛设了 M 级奖励。每个级别的奖金是一个正整数。
并且，相邻的两个级别间的比例是个固定值。
也就是说：所有级别的奖金数构成了一个等比数列。比如：
16，24，36，54
其等比值为：3/2

现在，我们随机调查了一些获奖者的奖金数。
请你据此推算可能的最大的等比值。

***输入格式：***

第一行为数字 N（0 &lt; N &le; 100），表示接下的一行包含 N 个正整数。
第二行 N 个正整数 X<sub>i</sub>（X<sub>i</sub> &lt; 1000000000000），用空格分开。每个整数表示调查到的某人的奖金数额。

***输出格式：***

一个形如 A/B 的分数，要求 A、B 互质。表示可能的最大比例系数。

> 测试数据保证了输入格式正确，并且最大比例是存在的。

***样例输入 1：***

```
3
1250 200 32
```

***样例输出 1：***

25/4

***样例输入 2：***

```
4
3125 32 32 200
```

***样例输出 2：***

5/2

***样例输入 3：***

```
3
549755813888 524288 2
```

***样例输出 3：***

4/1

***思路：***

设给出三个数 a<sub>1</sub>，a<sub>m</sub>，a<sub>n</sub>。
由于必然存在最大比例，则设最大比例为 q。

- **a<sub>n</sub> / a<sub>m</sub> = q<sup>k<sub>1</sub></sup>**
- **a<sub>m</sub> / a<sub>1</sub> = q<sup>k<sub>2</sub></sup>**

此时 k<sub>1</sub>、k<sub>2</sub> **一定互质**。（若不互质，则 q 不是最大比例，与假设矛盾。）  
则有 GCD(k<sub>1</sub>, k<sub>2</sub>) = 1。  
那么，现在研究一下怎么通过 q<sup>k<sub>1</sub></sup> 和 q<sup>k<sub>2</sub></sup> 来求 q<sup>GCD(k<sub>1</sub>,k<sub>2</sub>)</sup>（即 q）。  

对于一般的两个数，我们一定可以通过 **辗转相除法** 来求出其最大公约数。

1. 设两个初始值 x、y（1 &le; x, y）。
2. 不妨令 x &lt; y，则有 r = y - x。
3. 由于 r &lt; y，所以比较 r 与 x 的大小关系。
    - 若 r &gt; x，则 r = r - x。
    - 若 r &lt; x，则 r = x - r。
4. 不断重复第三步，直至 r = 0，则最后一次得到的 x 为最大公约数。

那么对于这道题，
q<sup>k<sub>1</sub>-k<sub>2</sub></sup> 相当于 q<sup>k<sub>1</sub></sup> / q<sup>k<sub>2</sub></sup>。
所以我们总是让大数除以小数，直至得到商为 1（指数为 0）时，最后一次所除的数就是 q<sup>GCD(k<sub>1</sub>,k<sub>2</sub>)</sup>。

当然，对于三个数的求法是这样。  
对于四个数 a<sub>1</sub>、a<sub>i</sub>、a<sub>m</sub>、a<sub>n</sub>，我们可以得到三个比例 q<sup>k<sub>1</sub></sup>、q<sup>k<sub>2</sub></sup>、q<sup>k<sub>3</sub></sup>，那么先通过前两个比例求出 q，再把 q 与最后一个比例求出更精准的 q。
以此类推。

```cpp
#include<cstdio>
#include<set>
#include<queue>
using namespace std;

class fraction{
public:
	long long a;
	long long b;
	
	long long gcd(long long a, long long b){
		return (a % b == 0)? b: gcd(b, a%b);
	}
	
	void maintain(){
		long long temp = gcd(a, b);
		a = a / temp;
		b = b / temp;
	}
	
	fraction(long long a, long long b){
		this->a = a;
		this->b = b;
		maintain();
	}
	
	fraction gcd(fraction f){
		if((a == f.a) && (b == f.b)){
			return f;
		}
		if((a > f.a) && (b >= f.b)){
			fraction temp(a/f.a, b/f.b);
			return temp.gcd(f);
		}
		else{
			fraction temp(f.a/a, f.b/b);
			return temp.gcd(*this);
		}
	}
};

set<long long> s;
set<long long>::iterator it;
queue<fraction> q;

int main(){

	int n;
	scanf("%d", &n);
	
	long long temp;
	for(int i = 1; i <= n; i ++){
		scanf("%I64d", &temp);
		s.insert(temp);
	}
	
	for(it = s.begin(); it != s.end(); it ++){
		if(it != s.begin()){
			q.push(fraction(*it, temp));
		}
		temp = *it;
	}
	
	while(q.size() >= 2){
		fraction f1 = q.front();
		q.pop();
		fraction f2 = q.front();
		q.pop();
		q.push(f1.gcd(f2));
	}
	
	printf("%I64d/%I64d\n", q.front().a, q.front().b);
	return 0;
}
```


