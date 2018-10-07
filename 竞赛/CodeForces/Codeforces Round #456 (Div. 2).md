[toc]

## A_Tricky Alchemy

[原题链接](http://codeforces.com/contest/912/problem/A)

![A1](http://oxnec2zdn.bkt.clouddn.com/codeforces/2018-01-05/20180105A1.png)
![A2](http://oxnec2zdn.bkt.clouddn.com/codeforces/2018-01-05/20180105A2.png)

思路：
当水晶足够的时候，需求数为0。

```cpp
#include<iostream>
using namespace std;

int main(){

	long long a, b;
	long long x, y, z;
	cin >> a >> b >> x >> y >> z;

	long long ans1 = (x*2+y)-a > 0 ? (x*2+y)-a : 0;
	long long ans2 = (y+z*3)-b > 0 ? (y+z*3)-b : 0;
	cout << ans1+ans2 << endl;

	return 0;
}
```

## B_New Year's Eve

[原题链接](http://codeforces.com/contest/912/problem/B)

![B1](http://oxnec2zdn.bkt.clouddn.com/codeforces/2018-01-05/20180105B1.png)
![B2](http://oxnec2zdn.bkt.clouddn.com/codeforces/2018-01-05/20180105B2.png)

## C_Perun, Ult!

[原题链接](http://codeforces.com/contest/912/problem/C)

![C1](http://oxnec2zdn.bkt.clouddn.com/codeforces/2018-01-05/20180105C1.png)
![C2](http://oxnec2zdn.bkt.clouddn.com/codeforces/2018-01-05/20180105C2.png)
![C3](http://oxnec2zdn.bkt.clouddn.com/codeforces/2018-01-05/20180105C2.png)
![C4](http://oxnec2zdn.bkt.clouddn.com/codeforces/2018-01-05/20180105C4.png)
![C5](http://oxnec2zdn.bkt.clouddn.com/codeforces/2018-01-05/20180105C51.png)

## D_Fishes

[原题链接](http://codeforces.com/contest/912/problem/D)

![D1](http://oxnec2zdn.bkt.clouddn.com/codeforces/2018-01-05/20180105D1.png)
![D2](http://oxnec2zdn.bkt.clouddn.com/codeforces/2018-01-05/20180105D2.png)

## E_Prime Gift

[原题链接](http://codeforces.com/contest/912/problem/E)

![E1](http://oxnec2zdn.bkt.clouddn.com/codeforces/2018-01-05/20180105E1.png)
![E2](http://oxnec2zdn.bkt.clouddn.com/codeforces/2018-01-05/20180105E1.png)
