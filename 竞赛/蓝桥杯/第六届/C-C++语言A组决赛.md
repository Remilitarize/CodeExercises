[toc]

## 方格填数（19 分）

在 2 行 5 列的格子中填入 1 到 10 的数字。

要求：
相邻的格子中的数，右边的大于左边的，下边的大于上边的。

![方格填数](http://oxnec2zdn.bkt.clouddn.com/lanqiaobeidiliujie/fanggetianshu.png)

请你计算一共有多少种可能的方案。

## 四阶幻方（35 分）

把 1~16 的数字填入 4x4 的方格中，使得行、列以及两个对角线的和都相等，满足这样的特征时称为：四阶幻方。

四阶幻方可能有很多方案。如果固定左上角为1，请计算一共有多少种方案。

比如：

```
  1  2 15 16
 12 14  3  5
 13  7 10  4
  8 11  6  9
```

以及：

```
  1 12 13  8
  2 14  7 11
 15  3 10  6
 16  5  4  9
```
 
就可以算为两种不同的方案。

## 显示二叉树（31 分）

排序二叉树的特征是：
某个节点的左子树的所有节点值都不大于本节点值。
某个节点的右子树的所有节点值都不小于本节点值。

为了能形象地观察二叉树的建立过程，小明写了一段程序来显示出二叉树的结构来。

```c
#include <stdio.h>
#include <string.h>
#define N 1000
#define HEIGHT 100
#define WIDTH 1000

struct BiTree
{
	int v;
	struct BiTree* l;
	struct BiTree* r;
};

int max(int a, int b)
{
	return a>b? a : b;
}

struct BiTree* init(struct BiTree* p, int v)
{
	p->l = NULL;
	p->r = NULL;
	p->v = v;
	
	return p;
}

void add(struct BiTree* me, struct BiTree* the)
{
	if(the->v < me->v){
		if(me->l==NULL) me->l = the;
		else add(me->l, the);
	}
	else{
		if(me->r==NULL) me->r = the;
		else add(me->r, the);
	}
}

//获得子树的显示高度	
int getHeight(struct BiTree* me)
{
	int h = 2;
	int hl = me->l==NULL? 0 : getHeight(me->l);
	int hr = me->r==NULL? 0 : getHeight(me->r);
	
	return h + max(hl,hr);
}

//获得子树的显示宽度	
int getWidth(struct BiTree* me)
{
	char buf[100];
	sprintf(buf,"%d",me->v); 
	int w = strlen(buf);
	if(me->l) w += getWidth(me->l);
	if(me->r) w += getWidth(me->r);
	return w;
}

int getRootPos(struct BiTree* me, int x){
	return me->l==NULL? x : x + getWidth(me->l);
}

//把缓冲区当二维画布用 
void printInBuf(struct BiTree* me, char buf[][WIDTH], int x, int y)
{
	int p1,p2,p3,i;
	char sv[100];
	sprintf(sv, "%d", me->v);
	
	p1 = me->l==NULL? x : getRootPos(me->l, x);
	p2 = getRootPos(me, x);
	p3 = me->r==NULL? p2 : getRootPos(me->r, p2+strlen(sv));
	
	buf[y][p2] = '|';
	for(i=p1; i<=p3; i++) buf[y+1][i]='-';
	for(i=0; i<strlen(sv); i++) buf[y+1][p2+i]=sv[i];
	if(p1<p2) buf[y+1][p1] = '/';
	if(p3>p2) buf[y+1][p3] = '\\';
	
	if(me->l) printInBuf(me->l,buf,x,y+2);
	if(me->r) ____________________________________;  //填空位置
}

void showBuf(char x[][WIDTH])
{
	int i,j;
	for(i=0; i<HEIGHT; i++){
		for(j=WIDTH-1; j>=0; j--){
			if(x[i][j]==' ') x[i][j] = '\0';
			else break;
		}
		if(x[i][0])	printf("%s\n",x[i]);
		else break;
	}
}
	
void show(struct BiTree* me)
{
	char buf[HEIGHT][WIDTH];
	int i,j;
	for(i=0; i<HEIGHT; i++)
	for(j=0; j<WIDTH; j++) buf[i][j] = ' ';
	
	printInBuf(me, buf, 0, 0);
	showBuf(buf);
}

int main()
{	
	struct BiTree buf[N];	//存储节点数据 
	int n = 0;              //节点个数 
	init(&buf[0], 500); n++;  //初始化第一个节点 

	add(&buf[0], init(&buf[n++],200));  //新的节点加入树中 
	add(&buf[0], init(&buf[n++],509));
	add(&buf[0], init(&buf[n++],100));
	add(&buf[0], init(&buf[n++],250));
	add(&buf[0], init(&buf[n++],507));
	add(&buf[0], init(&buf[n++],600));
	add(&buf[0], init(&buf[n++],650));
	add(&buf[0], init(&buf[n++],450));
	add(&buf[0], init(&buf[n++],440));
	add(&buf[0], init(&buf[n++],220));
	
	show(&buf[0]);	
	return 0;	
}
```

对于上边的测试数据，应该显示出：

![显示二叉树](http://oxnec2zdn.bkt.clouddn.com/lanqiaobeidiliujie/xianshierchashu.png)

## 穿越雷区（41 分）

X 星的坦克战车很奇怪，它必须交替地穿越正能量辐射区和负能量辐射区才能保持正常运转，否则将报废。
某坦克需要从 A 区到 B 区去（A，B 区本身是安全区，没有正能量或负能量特征），怎样走才能路径最短？

已知的地图是一个方阵，上面用字母标出了 A，B 区，其它区都标了正号或负号分别表示正负能量辐射区。
例如：

```
A + - + -
- + - - +
- + + + -
+ - + - +
B + - + -
```

坦克车只能水平或垂直方向上移动到相邻的区。

***输入格式***：

输入第一行是一个整数 n，表示方阵的大小， 4 &le; n &le; 100
接下来是 n 行，每行有 n 个数据，可能是 A，B，+，- 中的某一个，中间用空格分开。
A，B 都只出现一次。

***输出格式：***

输出一个整数，表示坦克从 A 区到 B 区的最少移动步数。
如果没有方案，则输出 -1。

***样例输入：***

```
5
A + - + -
- + - - +
- + + + -
+ - + - +
B + - + -
```

***样例输出：***

10

## 切开字符串（75 分）

Pear 有一个字符串，不过他希望把它切成两段。
这是一个长度为 N（N &le; 10<sup>5</sup>）的字符串。
Pear 希望选择一个位置，把字符串不重复不遗漏地切成两段，长度分别是 t 和 N-t（这两段都必须非空）。

Pear 用如下方式评估切割的方案：
定义"正回文子串"为：长度为奇数的回文子串。
设切成的两段字符串中，前一段中有 A 个不相同的正回文子串，后一段中有 B 个不相同的非正回文子串，则该方案的得分为 A\*B。

注意，后一段中的 B 表示的是："...非正回文..."，而不是: "...正回文..."。
那么所有的切割方案中，A\*B 的最大值是多少呢？

***输入格式：***

输入第一行一个正整数 N（N &le; 10<sup>5</sup>）。
接下来一行一个字符串，长度为 N。该字符串仅包含小写英文字母。

***输出格式：***

一行一个正整数，表示所求的 A\*B 的最大值。

***样例输入：***

10
bbaaabcaba

***样例输出：***

38

## 铺瓷砖（99 分）

为了让蓝桥杯竞赛更顺利的进行，主办方决定给竞赛的机房重新铺放瓷砖。机房可以看成一个 n\*m 的矩形，而这次使用的瓷砖比较特别，有两种形状，如图所示。在铺放瓷砖时，可以旋转。
 
![铺瓷砖](http://oxnec2zdn.bkt.clouddn.com/lanqiaobeidiliujie/pucizhuan.png)

主办方想知道，如果使用这两种瓷砖把机房铺满，有多少种方案。

***输入格式：***

输入的第一行包含两个整数，分别表示机房两个方向的长度。
1 &le; n &le; 10<sup>15</sup>，1 &le; m &le; 6

***输出格式：***

输出一个整数，表示可行的方案数。这个数可能很大，请输出这个数除以 65521 的余数。

***样例输入 1：***

4 4

***样例输出 1：***

2

***样例输入 2：***

2 6

***样例输出 2：***

4
