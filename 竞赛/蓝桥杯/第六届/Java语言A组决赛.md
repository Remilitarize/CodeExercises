[toc]

## 胡同门牌号（15 分）

小明家住在一条胡同里。胡同里的门牌号都是连续的正整数，由于历史原因，最小的号码并不是从 1 开始排的。
有一天小明突然发现了有趣的事情：
如果除去小明家不算，胡同里的其它门牌号加起来，刚好是 100！
并且，小明家的门牌号刚好等于胡同里其它住户的个数！

请你根据这些信息，推算小明家的门牌号是多少？

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

## 铺瓷砖（203 分）

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

