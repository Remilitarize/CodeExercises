[toc]

## 分机号（15 分）

X 老板脾气古怪，他们公司的电话分机号都是 3 位数，老板规定，所有号码必须是降序排列，且不能有重复的数位。比如：

751, 520, 321 都满足要求，而，
766, 918, 201 就不符合要求。

现在请你计算一下，按照这样的规定，一共有多少个可用的 3 位分机号码？

## 五星填数（31 分）

![五星填数](http://oxnec2zdn.bkt.clouddn.com/lanqiaobeidiliujie/wuxingtianshu.png)

如图的五星图案节点填上数字：1~12，除去 7 和 11。
要求每条直线上数字和相等。

如图就是恰当的填法。

请你利用计算机搜索所有可能的填法有多少种。

## 显示二叉树（37 分）

排序二叉树的特征是：
某个节点的左子树的所有节点值都不大于本节点值。
某个节点的右子树的所有节点值都不小于本节点值。

为了能形象地观察二叉树的建立过程，小明写了一段程序来显示出二叉树的结构来。

```java
class BiTree
{
	private int v;
	private BiTree l;
	private BiTree r;
	
	public BiTree(int v){
		this.v = v;
	}
	
	public void add(BiTree the){
		if(the.v < v){
			if(l==null) l = the;
			else l.add(the);
		}
		else{
			if(r==null) r = the;
			else r.add(the);
		}
	}
	
	public int getHeight(){
		int h = 2;
		int hl = l==null? 0 : l.getHeight();
		int hr = r==null? 0 : r.getHeight();
		return h + Math.max(hl,hr);
	}
	
	public int getWidth(){
		int w = (""+v).length();
		if(l!=null) w += l.getWidth();
		if(r!=null) w += r.getWidth();
		return w;
	}
	
	public void show(){
		char[][] buf = new char[getHeight()][getWidth()];
		printInBuf(buf, 0, 0);
		showBuf(buf);
	}
	
	private void showBuf(char[][] x){
		for(int i=0; i<x.length; i++){
			for(int j=0; j<x[i].length; j++)
				System.out.print(x[i][j]==0? ' ':x[i][j]);
			System.out.println();
		}
	}
	
	private void printInBuf(char[][] buf, int x, int y){
		String sv = "" + v;
		
		int p1 = l==null? x : l.getRootPos(x);
		int p2 = getRootPos(x);
		int p3 = r==null? p2 : r.getRootPos(p2+sv.length());
		
		buf[y][p2] = '|';
		for(int i=p1; i<=p3; i++) buf[y+1][i]='-';
		for(int i=0; i<sv.length(); i++) ________________________________;  //填空位置
		if(p1<p2) buf[y+1][p1] = '/';
		if(p3>p2) buf[y+1][p3] = '\\';
		
		if(l!=null) l.printInBuf(buf,x,y+2);
		if(r!=null) r.printInBuf(buf,p2+sv.length(),y+2);
	}
	
	private int getRootPos(int x){
		return l==null? x : x + l.getWidth();
	}
}

public class Main
{
	public static void main(String[] args)
	{		
		BiTree tree = new BiTree(500);
		tree.add(new BiTree(200));
		tree.add(new BiTree(509));
		tree.add(new BiTree(100));
		tree.add(new BiTree(250));
		tree.add(new BiTree(507));
		tree.add(new BiTree(600));
		tree.add(new BiTree(650));
		tree.add(new BiTree(450));
		tree.add(new BiTree(510));
		tree.add(new BiTree(440));
		tree.add(new BiTree(220));		
		tree.show();		
	}
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

## 表格计算（75 分）

某次无聊中， atm 发现了一个很老的程序。这个程序的功能类似于 Excel ，它对一个表格进行操作。
不妨设表格有 n 行，每行有 m 个格子。
每个格子的内容可以是一个正整数，也可以是一个公式。

公式包括三种：

1. `SUM(x1,y1:x2,y2)` 表示求左上角是第 x1 行第 y1 个格子，右下角是第 x2 行第 y2 个格子这个矩形内所有格子的值的和。
2. `AVG(x1,y1:x2,y2)` 表示求左上角是第 x1 行第 y1 个格子，右下角是第 x2 行第 y2 个格子这个矩形内所有格子的值的平均数。
3. `STD(x1,y1:x2,y2)` 表示求左上角是第 x1 行第 y1 个格子，右下角是第 x2 行第 y2 个格子这个矩形内所有格子的值的标准差。

标准差即为方差的平方根。
方差就是：每个数据与平均值的差的平方的平均值，用来衡量单个数据离开平均数的程度。

公式都不会出现嵌套。

如果这个格子内是一个数，则这个格子的值等于这个数，否则这个格子的值等于格子公式求值结果。

输入这个表格后，程序会输出每个格子的值。atm 觉得这个程序很好玩，他也想实现一下这个程序。

***输入格式：***

第一行两个数 n, m 。
接下来 n 行输入一个表格。每行 m 个由空格隔开的字符串，分别表示对应格子的内容。
输入保证不会出现循环依赖的情况，即不会出现两个格子 a 和 b 使得 a 的值依赖 b 的值且 b 的值依赖 a 的值。
n, m &le; 50

***输出格式：***

输出一个表格，共 n 行，每行 m 个保留两位小数的实数。
数据保证不会有格子的值超过 10<sup>6</sup> 。

***样例输入：***

```
3 2
1 SUM(2,1:3,1)
2 AVG(1,1:1,2)
SUM(1,1:2,1) STD(1,1:2,2)
```

***样例输出：***

```
1.00 5.00
2.00 3.00
3.00 1.48
```

## 铺瓷砖（101 分）

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
