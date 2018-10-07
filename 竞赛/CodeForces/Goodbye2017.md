[toc]

## A_New Year and Counting Cards

[原题链接](http://codeforces.com/contest/908/problem/A)

![A1](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/A1.png)
![A2](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/A2.png)

思路：

|数字|字母|结果|
|-|-|-|
|偶数|元音|真|
|偶数|非元音|真|
|奇数|元音|假|
|奇数|非元音|真|

即出现奇数或元音时需要判断另一面。

```cpp
#include<iostream>
#include<cctype>
using namespace std;

int main(){

	string s;
	cin >> s;

	int count = 0;
	for(int i = 0; i < s.length(); i ++){
		if(s[i] == 'a' || s[i] == 'e' || s[i] == 'i' || s[i] == 'o' || s[i] == 'u'){
			count ++;
		}
		else if(isdigit(s[i]) && (s[i] - '0') % 2){
			count ++;
		}
	}

	cout << count << endl;

	return 0;
}
```

## B_New Year and Buggy Bot

[原题链接](http://codeforces.com/contest/908/problem/B)

![B1](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/B1.png)
![B2](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/B2.png)
![B3](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/B3.png)
![B4](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/B4.png)

思路：
对于给出的序列，由于都没有规定方向，所以枚举各个数字对应的方向即可，然后进行 DFS，能够走到出口就计数。

```cpp
#include<iostream>
#include<string>
#include<cstdio>
using namespace std;

const int maxn = 100+5;
char map[maxn][maxn];
int m, n;
int startx, starty, endx, endy;
string temp;
int xx[] = {0, 1, 0, -1};
int yy[] = {1, 0, -1, 0};

int posx, posy;
bool dfs(int a, int b, int c, int d, int pos){
	if(posx < 0 || posx >= m || posy < 0 || posy >= n || map[posx][posy] == '#'){
		// 判断是否出界
		return false;
	}
	else if(posx == endx && posy == endy){
		// 判断是否为终点
		return true;
	}
	else if(pos >= temp.length()){
		// 判断步数
		return false;
	}
	else{
		if(temp[pos] == '0'){
			posx += xx[a], posy += yy[a];
			return dfs(a, b, c, d, pos+1);
		}
		else if(temp[pos] == '1'){
			posx += xx[b], posy += yy[b];
			return dfs(a, b, c, d, pos+1);
		}
		else if(temp[pos] == '2'){
			posx += xx[c], posy += yy[c];
			return dfs(a, b, c, d, pos+1);
		}
		else if(temp[pos] == '3'){
			posx += xx[d], posy += yy[d];
			return dfs(a, b, c, d, pos+1);
		}
	}
}

int main(){

	cin >> m >> n;

	for(int i = 0; i < m; i ++){
		scanf("%s", map[i]);
		for(int j = 0; j < n; j ++){
			if(map[i][j] == 'S'){
				startx = i, starty = j;
			}
			else if(map[i][j] == 'E'){
				endx = i, endy = j;
			}
		}
	}

	cin.ignore();
	getline(cin, temp);

	bool flag[4] = {false, false, false, false};

	int count = 0;
	for(int i = 0; i <= 3; i ++){
		flag[i] = true;
		for(int j = 0; j <= 3; j ++){
			if(flag[j]){
				continue;
			}
			flag[j] = true;
			for(int k = 0; k <= 3; k ++){
				if(flag[k]){
					continue;
				}
				flag[k] = true;
				for(int x = 0; x <= 3; x ++){
					if(flag[x]){
						continue;
					}
					posx = startx, posy = starty;
					dfs(i, j, k, x, 0)? count ++: 0;
				}
				flag[k] = false;
			}
			flag[j] = false;
		}
		flag[i] = false;
	}
	cout << count << endl;

	return 0;
}
```

借助 `next_permutation()` 函数简化写法：

```cpp
#include<iostream>
#include<string>
#include<cstdio>
#include<algorithm>
using namespace std;

const int maxn = 100+5;
char map[maxn][maxn];
int m, n;
int startx, starty, endx, endy;
string temp;
int dir[] = {0, 1, 2, 3};

int posx, posy;
bool dfs(int pos){
	if(posx < 0 || posx >= m || posy < 0 || posy >= n || map[posx][posy] == '#'){
		// 判断是否出界
		return false;
	}
	else if(posx == endx && posy == endy){
		// 判断是否为终点
		return true;
	}
	else if(pos >= temp.length()){
		// 判断步数
		return false;
	}
	else{
		for(int i = 0; i < 4; i ++){
			if((temp[pos] - '0') == dir[i]){
				if(i == 0){
					posx ++;
				}
				else if(i == 1){
					posx --;
				}
				else if(i == 2){
					posy ++;
				}
				else if(i == 3){
					posy --;
				}
				return dfs(pos+1);
			}
		}
	}
}

int main(){

	cin >> m >> n;

	for(int i = 0; i < m; i ++){
		scanf("%s", map[i]);
		for(int j = 0; j < n; j ++){
			if(map[i][j] == 'S'){
				startx = i, starty = j;
			}
			else if(map[i][j] == 'E'){
				endx = i, endy = j;
			}
		}
	}

	cin.ignore();
	getline(cin, temp);

	int count = 0;
	do{
		posx = startx, posy = starty;
		dfs(0)? count ++: 0;
	}
	while(next_permutation(dir, dir+4));
	cout << count << endl;

	return 0;
}
```

## C_New Year and Curling

[原题链接](http://codeforces.com/contest/908/problem/C)

![C1](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/C1.png)
![C2](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/C2.png)
![C3](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/C3.png)

思路：
数据 1000 以内，可以暴力枚举之前所有的圆心与当前圆心的距离，撞上了当前圆的最高点（即圆心 y 坐标）为相撞圆的圆最高点 + sqrt((2 倍半径)<sup>2</sup> - (两圆心 x 坐标差)<sup>2</sup>)。
这道题卡精度，需要对流进行特殊处理。

```cpp
#include<iostream>
#include<cmath>
#include<algorithm>
using namespace std;

const int maxn = 1000+5;
long double x[maxn];
long double y[maxn];

int main(){

//	ios_base::sync_with_stdio(false);  // 取消与 stdio 的同步，提高效率
	cout.setf(ios::fixed);             // 固定输出的精度
	cout.precision(20);                // 设置精度为 20
//	cout.tie(nullptr);                 // 解除绑定，提高速度
//	cin.tie(nullptr);
	// nullptr 为 c++11 标准，编译时添加命令 --std=c++11

	int n;
	long double radius;
	cin >> n >> radius;

	for(int i = 0; i < n; i ++){
		cin >> x[i];

		y[i] = radius;
		for(int j = i-1; j >=0; j --){
			if(abs(x[i] - x[j]) <= 2 * radius){
				y[i] = max(y[i], y[j] + sqrt(4*radius*radius - (x[i]-x[j])*(x[i]-x[j])));
			}
		}
	}

	for(int i = 0; i < n; i ++){
		cout << y[i] << ((i==n-1)?"\n":" ");
	}

	return 0;
}
```

## D_New Year and Arbitrary Arrangement

[原题链接](http://codeforces.com/contest/908/problem/D)

![D1](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/D1.png)
![D2](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/D2.png)


## E_New Year and Entity Enumeration

[原题链接](http://codeforces.com/contest/908/problem/E)

![E1](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/E1.png)
![E2](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/E2.png)

## F_New Year and Rainbow Roads

[原题链接](http://codeforces.com/contest/908/problem/F)

![F1](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/F1.png)
![F2](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/F2.png)

## G_New Year and Original Order

[原题链接](http://codeforces.com/contest/908/problem/G)

![G](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/G.png)

## H_New Year and Boolean Bridges

[原题链接](http://codeforces.com/contest/908/problem/H)

![H1](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/H1.png)
![H2](http://oxnec2zdn.bkt.clouddn.com/codeforces/2017-12-29/H2.png)
