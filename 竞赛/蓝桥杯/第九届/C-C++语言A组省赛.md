[toc]

## 分数（5 分）

1/1 + 1/2 + 1/4 + 1/8 + 1/16 + .... 

每项是前一项的一半，如果一共有 20 项，求这个和是多少，结果用分数表示出来。
类似：3/2

当然，这只是加了前 2 项而已。分子分母要求互质。

***思路：***
正常思路直接进行累加。
或利用性质:

1/2 + 1/4 + 1/8 + ... + 1/2<sup>n</sup> + 1/2<sup>n</sup> - 1/2<sup>n</sup>
= 1 - 1/2<sup>n</sup>

```cpp
#include<cstdio>
using namespace std;

int pow(int x, int n){
	if(n == 0){
		return 1;
	}
	if(n == 1){
		return x;
	}
	
	int t = pow(x, n / 2);
	
	if(n % 2 == 1){
		return t * t * x;
	}
	else{
		return t * t;
	}
}

int gcd(int a, int b){
	if(a % b == 0){
		return b;
	}
	else{
		return gcd(b, a % b);
	}
}

int main(){
	
	int temp = pow(2, 19);
	int ans = 2 * temp - 1;
	
	int t = gcd(ans, temp);
	printf("%d/%d\n", ans / t, temp / t);
	
    return 0;
}
```

## 星期一（7 分）

整个 20 世纪（1901 年 1 月 1 日至 2000 年 12 月 31 日之间），一共有多少个星期一？
（不要告诉我你不知道今天是星期几）

***思路：***
2000 年 12 月 31 日是周日（右下角），则将距离 1901 年之间的所有天数累加除以 7 即可知道有几个周一。
最后进行余数判断。

```cpp
#include<cstdio>
using namespace std;

int leap(int year){
	if(year % 400 == 0 || (year % 100 != 0 && year % 4 == 0)){
		return 1;
	}
	else{
		return 0;
	}
}

int main(){
	
	int sum = 0;
	for(int i = 1901; i <= 2000; i ++){
		sum += 365 + leap(i);
	}
	
	int ans = sum / 7 + (sum % 7 == 0? 1: 0);
	printf("%d\n", ans);
	
    return 0;
}
```

## 乘积尾零（9 分）

如下的 10 行数据，每行有 10 个整数，请你求出它们的乘积的末尾有多少个零？

```
5650 4542 3554 473 946 4114 3871 9073 90 4329 
2758 7949 6113 5659 5245 7432 3051 4434 6704 3594 
9937 1173 6866 3397 4759 7557 3070 2287 1453 9899 
1486 5722 3135 1170 4014 5510 5120 729 2880 9019 
2049 698 4582 4346 4427 646 9742 7340 1230 7683 
5693 7015 6887 7381 4172 4341 2909 2027 7355 5649 
6701 6645 1671 5978 2704 9926 295 3125 3878 6785 
2066 4247 4800 1578 6652 4616 1113 6205 3264 2915 
3966 5291 2904 1285 2193 1428 2265 8730 9436 7074 
689 5510 8243 6114 337 4096 8199 7313 3685 211
```

***思路：***

阶乘尾零的变种，当且仅当 2 的倍数乘以 5 的倍数时才能产生尾 0。

统计 5、25、125、625 的倍数，其中

- 每一个 5 的倍数乘以 2 的倍数，尾零加 1。
- 每一个 25 的倍数乘以 4 的倍数，尾零加 2。
- 以此类推。

```cpp
#include<cstdio>
using namespace std;
int count1 = 0;
int count2 = 0;

void f1(int a){
	while(a && a % 5 == 0){
		count1 ++;
		a /= 5;
	}
}

void f2(int a){
	while(a && a % 2 == 0){
		count2 ++;
		a /= 5;
	}
}

int main(){
	
	int temp;
	for(int i = 1; i <= 100; i ++){
		scanf("%d", &temp);
		if(temp % 5 == 0){
			f1(temp);
			f2(temp;)
		}
	}
	
	printf("%d\n", count1 > count2? count1 : count2);
	
    return 0;
}
```

## 第几个幸运数（13 分）

到 x 星球旅行的游客都被发给一个整数，作为游客编号。
x 星的国王有个怪癖，他只喜欢数字 3，5 和 7。
国王规定，游客的编号如果只含有因子：3，5，7 就可以获得一份奖品。

我们来看前 10 个幸运数字是：
3 5 7 9 15 21 25 27 35 45
因而第 11 个幸运数字是：49

小明领到了一个幸运数字 59084709587505，他去领奖的时候，人家要求他准确地说出这是第几个幸运数字，否则领不到奖品。

请你帮小明计算一下，59084709587505 是第几个幸运数字。

***思路：***
这道题直接暴力是无法在规定时间内得到结果的。
所以我们想办法依次枚举每个 "幸运数" 并计数。
这里使用优先队列将每个最小的数提取出来，再将其 3 的倍数、5 的倍数、7 的倍数放进优先队列中即可。
注意数字可能重复，所以使用 `set` 进行标记。

```cpp
#include<cstdio>
#include<queue>
#include<set>
using namespace std;

priority_queue<long long, vector<long long>, greater<long long> > q;
set<long long> s;

int main(){
	
	int count = 0;
	
	long long temp = 1;
	q.push(temp * 3);
	q.push(temp * 5);
	q.push(temp * 7);
	s.insert(temp * 3);
	s.insert(temp * 5);
	s.insert(temp * 7);
	
	while(temp != (long long)59084709587505){
		temp = q.top();
		q.pop();
		count ++;
		if(s.find(temp * 3) == s.end()){
			q.push(temp * 3);
			s.insert(temp * 3);
		}
		if(s.find(temp * 5) == s.end()){
			q.push(temp * 5);
			s.insert(temp * 5);
		}
		if(s.find(temp * 7) == s.end()){
			q.push(temp * 7);
			s.insert(temp * 7);
		}
	}
	printf("%d\n", count);
	
    return 0;
}
```

## 打印图形（11 分）

如下的程序会在控制台绘制分形图（就是整体与局部自相似的图形）。

当 n = 1，2，3 的时候，输出如下：
请仔细分析程序，并填写划线部分缺少的代码。

n = 1时：

```
 o 
ooo
 o 
```

n = 2时：

```
    o    
   ooo   
    o    
 o  o  o 
ooooooooo
 o  o  o 
    o    
   ooo   
    o    
```

n = 3时：

```
             o             
            ooo            
             o             
          o  o  o          
         ooooooooo         
          o  o  o          
             o             
            ooo            
             o             
    o        o        o    
   ooo      ooo      ooo   
    o        o        o    
 o  o  o  o  o  o  o  o  o 
ooooooooooooooooooooooooooo
 o  o  o  o  o  o  o  o  o 
    o        o        o    
   ooo      ooo      ooo   
    o        o        o    
             o             
            ooo            
             o             
          o  o  o          
         ooooooooo         
          o  o  o          
             o             
            ooo            
             o     
```        

源程序：

```cpp
#include <stdio.h>
#include <stdlib.h>

void show(char* buf, int w){
	int i,j;
	for(i=0; i<w; i++){
		for(j=0; j<w; j++){
			printf("%c", buf[i*w+j]==0? ' ' : 'o');
		}
		printf("\n");
	}
}

void draw(char* buf, int w, int x, int y, int size){
	if(size==1){
		buf[y*w+x] = 1;
		return;
	}
	
	int n = _________________________ ; //填空
	draw(buf, w, x, y, n);
	draw(buf, w, x-n, y ,n);
	draw(buf, w, x+n, y ,n);
	draw(buf, w, x, y-n ,n);
	draw(buf, w, x, y+n ,n);
}

int main()
{
	int N = 3;
	int t = 1;
	int i;
	for(i=0; i<N; i++) t *= 3;
	
	char* buf = (char*)malloc(t*t);
	for(i=0; i<t*t; i++) buf[i] = 0;
	
	draw(buf, t, t/2, t/2, t);
	show(buf, t);
	free(buf);
	
	return 0;
}
```

***答案：*** `size / 3`

## 航班时间（17 分）

小 h 前往美国参加了蓝桥杯国际赛。小 h 的女朋友发现小 h 上午十点出发，上午十二点到达美国，于是感叹到 "现在飞机飞得真快，两小时就能到美国了"。

小 h 对超音速飞行感到十分恐惧。仔细观察后发现飞机的起降时间都是当地时间。
由于北京和美国东部有 12 小时时差，故飞机总共需要 14 小时的飞行时间。

不久后小 h 的女朋友去中东交换。小 h 并不知道中东与北京的时差。但是小 h 得到了女朋友来回航班的起降时间。小 h 想知道女朋友的航班飞行时间是多少。

对于一个可能跨时区的航班，给定来回程的起降时间。假设飞机来回飞行时间相同，求飞机的飞行时间。

**输入格式**

从标准输入读入数据。
一个输入包含多组数据。

输入第一行为一个正整数 T，表示输入数据组数。
每组数据包含两行，第一行为去程的 起降 时间，第二行为回程的 起降 时间。
起降时间的格式如下

`h1:m1:s1 h2:m2:s2`
或
`h1:m1:s1 h3:m3:s3 (+1)`
或
`h1:m1:s1 h4:m4:s4 (+2)`
表示该航班在当地时间 h1 时 m1 分 s1 秒起飞，

第一种格式表示在当地时间 当日 h2 时 m2 分 s2 秒降落
第二种格式表示在当地时间 次日 h3 时 m3 分 s3 秒降落。
第三种格式表示在当地时间 第三天 h4 时 m4 分 s4 秒降落。

对于此题目中的所有以 h:m:s 形式给出的时间, 保证（0 &le; h &le; 23, 0 &le; m，s &le; 59）。

**输出格式**

输出到标准输出。

对于每一组数据输出一行一个时间 hh:mm:ss，表示飞行时间为 hh 小时 mm 分 ss 秒。
注意，当时间为一位数时，要补齐前导零。如三小时四分五秒应写为 03:04:05。

**样例输入**

```
3
17:48:19 21:57:24
11:05:18 15:14:23
17:21:07 00:31:46 (+1)
23:02:41 16:13:20 (+1)
10:19:19 20:41:24
22:19:04 16:41:09 (+1)
```

**样例输出**

```
04:09:05
12:10:39
14:22:05
```

**限制与约定**

保证输入时间合法，飞行时间不超过 24 小时。

***思路：***

- 对于两个时间相减后两次行程的小时数相等，则直接输出。
- 对于两个时间相减后两次行程的小时数不等，
    - 对于降落时间在当天之内至次日比起飞时间小的小时数内（即相减小于 24），说明跨越了两次时区（来一次去一次）。
    - 对于单次行程小时数大于 24 小时，说明该单次行程跨越了两次时区，总共跨越了三次时区。
    - 对于两次行程小时数均大于 24 小时，说明均跨越了两次时区，总共跨越了四次时区。

> 这里跨越时区不知是否正确。

```cpp
#include<iostream>
#include<string>
#include<cctype>
using namespace std;

struct time{
	int hour;
	int minute;
	int second;
	
	time sub(const time x){
		time ans;
		ans.hour = hour - x.hour;
		ans.minute = minute - x.minute;
		ans.second = second - x.second;
		
		if(ans.second < 0){
			ans.minute --;
			ans.second += 60;
		}
		if(ans.minute < 0){
			ans.hour --;
			ans.minute += 60;
		}
		
		return ans;
	}
};

string str;

int main(){

	int n;
	scanf("%d", &n);
	getchar();
	while(n --){
		time t1, t2, t3, t4;
		time tt1, tt2;
		
		getline(cin, str);
		
		t1.hour = (str[0] - '0') * 10 + str[1] - '0';
		t1.minute = (str[3] - '0') * 10 + str[4] - '0';
		t1.second = (str[6] - '0') * 10 + str[7] - '0';
		
		t2.hour = (str[9] - '0') * 10 + str[10] - '0';
		t2.minute = (str[12] - '0') * 10 + str[13] - '0';
		t2.second = (str[15] - '0') * 10 + str[16] - '0';
		
		if(str[str.length() - 1] == ')'){
			t2.hour += (str[str.length() - 2] - '0') * 24;
		}
		
		tt1 = t2.sub(t1);
		
		getline(cin, str);
			
		t3.hour = (str[0] - '0') * 10 + str[1] - '0';
		t3.minute = (str[3] - '0') * 10 + str[4] - '0';
		t3.second = (str[6] - '0') * 10 + str[7] - '0';
		
		t4.hour = (str[9] - '0') * 10 + str[10] - '0';
		t4.minute = (str[12] - '0') * 10 + str[13] - '0';
		t4.second = (str[15] - '0') * 10 + str[16] - '0';
		
		if(str[str.length() - 1] == ')'){
			t4.hour += (str[str.length() - 2] - '0') * 24;
		}
		
		tt2 = t4.sub(t3);
		
		if(tt1.hour == tt2.hour){
			printf("%02d:%02d:%02d\n", tt1.hour, tt1.minute, tt1.second);
		}
		else if(tt1.hour <= 24 && tt2.hour <= 24){
			printf("%02d:%02d:%02d\n", (tt1.hour + tt2.hour) / 2, tt1.minute, tt1.second);
		}
		else if(tt1.hour > 24 && tt2.hour > 24){
			int temp = tt1.hour > tt2.hour? 
						tt2.hour + (tt1.hour - tt2.hour) / 4: tt1.hour + (tt2.hour - tt1.hour) / 4;
			printf("%02d:%02d:%02d\n", temp, tt1.minute, tt1.second);
		}
		else{
			int temp = tt1.hour > tt2.hour? 
						tt2.hour + (tt1.hour - tt2.hour) / 3: tt1.hour + (tt2.hour - tt1.hour) / 3;
			printf("%02d:%02d:%02d\n", temp, tt1.minute, tt1.second);
		}
	}
	
    return 0;
}
```

## 三体攻击（19 分）

三体人将对地球发起攻击。为了抵御攻击，地球人派出了 A × B × C 艘战舰，在太空中排成一个 A 层 B 行 C 列的立方体。其中，第 i 层第 j 行第 k 列的战舰（记为战舰 (i, j, k)）的生命值为 d(i, j, k)。

三体人将会对地球发起 m 轮 "立方体攻击"，每次攻击会对一个小立方体中的所有战舰都造成相同的伤害。具体地，第 t 轮攻击用 7 个参数 lat, rat, lbt, rbt, lct, rct, ht 描述；
所有满足 i ∈ [lat, rat],j ∈ [lbt, rbt],k ∈ [lct, rct] 的战舰 (i, j, k) 会受到 ht 的伤害。如果一个战舰累计受到的总伤害超过其防御力，那么这个战舰会爆炸。

地球指挥官希望你能告诉他，第一艘爆炸的战舰是在哪一轮攻击后爆炸的。

**输入格式**
从标准输入读入数据。

第一行包括 4 个正整数 A, B, C, m；
第二行包含 A × B × C 个整数，其中第 ((i − 1)×B + (j − 1)) × C + (k − 1)+1 个数为 d(i, j, k)；
第 3 到第 m + 2 行中，第 (t − 2) 行包含 7 个正整数 lat, rat, lbt, rbt, lct, rct, ht。

**输出格式**
输出到标准输出。

输出第一个爆炸的战舰是在哪一轮攻击后爆炸的。保证一定存在这样的战舰。

**样例输入**

```
2 2 2 3
1 1 1 1 1 1 1 1
1 2 1 2 1 1 1
1 1 1 2 1 2 1
1 1 1 1 1 1 2
```

**样例输出**
2

**样例解释**
在第 2 轮攻击后，战舰 (1，1，1) 总共受到了 2 点伤害，超出其防御力导致爆炸。

**数据约定**
对于 10% 的数据，B = C = 1；
对于 20% 的数据，C = 1；
对于 40% 的数据，A × B × C, m ≤ 10, 000；
对于 70% 的数据，A, B, C ≤ 200；
对于所有数据，A × B × C ≤ 10<sup>6</sup>, m ≤ 10<sup>6</sup>, 0 ≤ d(i, j, k), ht ≤ 10<sup>9</sup>。

***思路：***
三维线段树，暂空。

## 全球变暖（21 分）

你有一张某海域 N*N 像素的照片，"." 表示海洋、"#" 表示陆地，如下所示：

```
.......
.##....
.##....
....##.
..####.
...###.
.......
```

其中 "上下左右" 四个方向上连在一起的一片陆地组成一座岛屿。例如上图就有 2 座岛屿。  

由于全球变暖导致了海面上升，科学家预测未来几十年，岛屿边缘一个像素的范围会被海水淹没。
具体来说如果一块陆地像素与海洋相邻（上下左右四个相邻像素中有海洋），它就会被淹没。  

例如上图中的海域未来会变成如下样子：

```
.......
.......
.......
.......
....#..
.......
.......
```

请你计算：依照科学家的预测，照片中有多少岛屿会被完全淹没。  

**输入格式**
第一行包含一个整数 N。（1 &le; N &le; 1000）
以下 N 行 N 列代表一张海域照片。  

照片保证第 1 行、第 1 列、第 N 行、第 N 列的像素都是海洋。  

**输出格式**
一个整数表示答案。

**输入样例**

```
7 
.......
.##....
.##....
....##.
..####.
...###.
.......
```

**输出样例**
1

***思路：***
典型 BFS 题目。
利用 BFS 对岛屿进行编号，再模拟被水淹后的结果，统计剩余岛屿数。
注意有可能岛屿上有类似山峰地形的地方，会形成两个岛屿，所以需要对统计出来的结果去重。

```cpp
#include<cstdio>
#include<cstring>
#include<queue>
#include<set>
using namespace std;

struct node{
	int x;
	int y;
};

int n;
const int maxn = 1000 + 5;
char map[maxn][maxn];
int island[maxn][maxn];

int count = 0;
queue<node> q;
set<int> s;

int x[] = {0, 1, 0, -1};
int y[] = {1, 0, -1, 0};

void bfs(){
	node temp;
	while(!q.empty()){
		temp = q.front();
		q.pop();
		island[temp.x][temp.y] = count;

		int xx, yy;
		for(int i = 0; i < 4; i ++){
			xx = temp.x + x[i];
			yy = temp.y + y[i];
			if(xx >= 0 && xx < n && yy >= 0 && yy < n && 
				map[xx][yy] == '#' && island[xx][yy] == 0){
				q.push((node){xx, yy});
			}
		}
	}
}

int main()
{
	memset(island, 0, sizeof(island));

	scanf("%d", &n);
	
	for(int i = 0; i < n; i ++){
		scanf("%s", map[i]);
	}
	
	for(int i = 0; i < n; i ++){
		for(int j = 0; j < n; j ++){
			if(map[i][j] == '#' && island[i][j] == 0){
				count ++;
				q.push((node){i, j});
				bfs();
			}
		}
	}
	
	for(int i = 0; i < n; i ++){
		for(int j = 0; j < n; j ++){
			int xx, yy;
			for(int k = 0; map[i][j] == '#' && k < 4; k ++){
				xx = i + x[k];
				yy = j + y[k];
				if(xx >= 0 && xx < n && yy >= 0 && yy < n &&map[xx][yy] == '.'){
					island[i][j] = 0;
					break;
				}
			}
		}
	}
	
	for(int i = 0; i < n; i ++){
		for(int j = 0; j < n; j ++){
			if(island[i][j] != 0){
				s.insert(island[i][j]);
			}
		}
	}
	
	printf("%d\n", count - s.size());
		
    return 0;
}
```

## 倍数问题（23 分）

众所周知，小葱同学擅长计算，尤其擅长计算一个数是否是另外一个数的倍数。
但小葱只擅长两个数的情况，当有很多个数之后就会比较苦恼。
现在小葱给了你 n 个数，希望你从这 n 个数中找到三个数，使得这三个数的和是 K 的倍数，且这个和最大。
数据保证一定有解。

**输入格式**
从标准输入读入数据。

第一行包括 2 个正整数 n, K。
第二行 n 个正整数，代表给定的 n 个数。

**输出格式**
输出到标准输出。
输出一行一个整数代表所求的和。

**样例输入**

```
4 3
1 2 3 4
```

**样例输出**
9

**样例解释**
选择 2、3、4。

**数据约定**
对于 30% 的数据，n &le; 100。
对于 60% 的数据，n &le; 1000。
对于另外 20% 的数据，K &le; 10。
对于 100% 的数据，1 &le; n &le; 10<sup>5</sup>, 1 &le; K &le; 10<sup>3</sup>，给定的 n 个数均不超过 10<sup>8</sup>。

***思路：***
将对 K 取余后所有的前三大值进行存储，对这些值进行循环找出最大值。

```cpp
#include<cstdio>
#include<cstring>
using namespace std;

const int maxn = 1000 + 5;
int num[maxn][3];

void move(int x, int i){
	num[i][2] = num[i][1];
	num[i][1] = num[i][0];
	num[i][0] = x;
}

int main(){
	
	memset(num, 0, sizeof(num));
	int n, k;
	scanf("%d%d", &n, &k);
	
	int temp;
	for(int i = 0; i < n; i ++){
		scanf("%d", &temp);
		if(temp >= num[temp % k][0]){
			move(temp, temp % k);
		}
	}
	
	int ans = 0;
	int t;
	for(int i = 0; i < k; i ++){
		for(int j = 0; j < k; j ++){
			temp = k - i - j;
			if(temp >= 0 && temp <= k){
				temp %= k;
				if(i == j && j == temp){
					t = num[i][0] + num[i][1] + num[i][2];
				}
				else if(i == j){
					t = num[i][0] + num[i][1] + num[temp][0];
				}
				else if(i == temp){
					t = num[i][0] + num[i][1] + num[j][0];
				}
				else if(j == temp){
					t = num[i][0] + num[j][0] + num[j][1];
				}
				else{
					t = num[i][0] + num[j][0] + num[temp][0];
				}
				ans < t? ans = t: 0;
			}
		}
	}
	printf("%d\n", ans);
	
    return 0;
}
```

## 付账问题（25 分）

几个人一起出去吃饭是常有的事。但在结帐的时候，常常会出现一些争执。

现在有 n 个人出去吃饭，他们总共消费了 S 元。其中第 i 个人带了 ai 元。幸运的是，所有人带的钱的总数是足够付账的，但现在问题来了：每个人分别要出多少钱呢？

为了公平起见，我们希望在总付钱量恰好为 S 的前提下，最后每个人付的钱的标准差最小。这里我们约定，每个人支付的钱数可以是任意非负实数，即可以不是1分钱的整数倍。你需要输出最小的标准差是多少。

标准差的介绍：标准差是多个数与它们平均数差值的平方平均数，一般用于刻画这些数之间的 "偏差有多大"。形式化地说，设第 i 个人付的钱为 bi 元，那么标准差为：

![付账问题](http://oxnec2zdn.bkt.clouddn.com/lanqiaobeidijiujie/fuzhangwenti.png)

**输入格式**
从标准输入读入数据。

第一行包含两个整数 n、S；
第二行包含 n 个非负整数 a1, ..., an。

**输出格式**
输出到标准输出。

输出最小的标准差，四舍五入保留 4 位小数。
保证正确答案在加上或减去 10<sup>−9</sup> 后不会导致四舍五入的结果发生变化。

**样例 1 输入**
5 2333
666 666 666 666 666

**样例输出**
0.0000

**样例解释**
每个人都出 2333/5 元，标准差为 0。

再比如：
**样例输入**
10 30
2 1 4 7 4 8 3 6 4 7

**样例输出**
0.7928

**数据说明**
对于 10% 的数据，所有 ai 相等；
对于 30% 的数据，所有非 0 的 ai 相等；
对于 60% 的数据，n ≤ 1000；
对于 80% 的数据，n ≤ 10^5；
对于所有数据，n ≤ 5 × 10<sup>5</sup>, 0 ≤ ai ≤ 10<sup>9</sup>。

***思路：***
二维二分，暂空。
