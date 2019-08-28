---
title: TWHomework——生成迷宫
copyright: true
categories: 
  - 笔试
tags: 
  - Maze

---
<blockquote class="blockquote-center">The best hearts are always the bravest.
![](/myimages/19.jpg)

<!-- more -->

---

# 题目 #
1. 问题

用计算机生成迷宫是一个很有趣的任务。我们可以用道路网格(Road Grid) 来表示迷宫的道路，那么3 x 3的道路网格（图-1 左）可以对应一个7 x 7的渲染网格(Render Grid)——图-1 右的方式（迷宫的墙是灰色的，道路是白色的）：
![](/myimages/19TWHomework1.png)

如果我们将迷宫道路网格两个相邻的cell连通，则可以打通道路。如图-2所示：
![](/myimages/19TWHomework2.png)

连通 道路网格 有如下的约束条件：
● 每一个 cell 只能够直接与相邻正南、正北、正东、正西的 cell 连通。不能够和其他的 cell 连通。
● 两个 cell 之间的连通一定是双向的。即 cell(0,0) 和 cell(1,0) 连通等价于 cell(1,0) 和 cell(0,0) 的连通。

2. 要求1：将迷宫渲染为字符串

现在我们希望你书写程序，将给定迷宫的道路网格，渲染为字符串输出。例如，其使用方式如下（伪代码，仅做演示，实际实现时请应用实际语言的编程风格）
```
Maze maze = MazeFactory.Create(command);
String mazeText = maze.Render();
```
其中 command 是一个字符串。它的定义如下：
● 第一行是迷宫道路网格的尺寸。例如 3 x 3 的迷宫为 3 3 ，而 5 x 4 的迷宫为 5 4 （5 行 4 列） 。
● 第二行是迷宫道路网格的连通性定义。如果 cell(0,1) 和 cell(0,2) 是连通的，则表示为：
0,1 0,2 ，多个连通以分号 ; 隔开。
![](/myimages/19TWHomework3.png)

3. 要求2：检查输入的有效性

在处理输入的时候需要检查输入的有效性。需要检查的有效性包括如下的几个方面：
● 无效的数字：输入的字符串无法正确的转换为数字。此时，该函数的输出为字符串 ”Invalid
number format . ”
● 数字超出预定范围：数字超出了允许的范围，例如为负数等。此时，该函数的输出为字符串
”Number out of range . ”
● 格式错误：输入命令的格式不符合约定。此时，该函数的输出为字符串 ”Incorrect command
format . ”
● 连通性错误：如果两个网格无法连通，则属于这种错误。此时，该函数的输出为字符串 ”Maze
format error.”
当多个问题同时出现时，报告其中一个错误即可。


---

# 思路 #
1、观察给定用例的输入输出可得出以下规律：

（1）输入的第一行字符串“m n”用来确定迷宫的大小（scale），行数row=m*2+1，列数column=n*2
（2）输入的第二行字符串用来确定哪两个网格之间连通（path），并可以通过每一组网格的坐标及其之间的关系确定连通节点在新的迷宫中的位置

2、迷宫由两个参数确定
（1）int[][] mazeArray
首先由第一行字符串确定迷宫大小，此二维数组的每一个元素为mazeArray[i][j]，若i或者j其中有一个偶数，则将该元素暂时赋值为0（代表墙W），其余位置确定赋1（代表初始通路R）；
后面需要根据输入字符串的第二行确定其他通路，修改此代表迷宫的二维数组mazeArray，将原本暂时赋的0改为1。

（2）int[][] pathArray
将输入字符串第二行的处理为另一个二维数组pathArray，第一维度代表对字符串分组后的下标，第二维度代表每一个分组中的4个数字；

例如0,1 0,2;0,0 1,0可以被分为两组，其中第一组中的元素分别为pathArray[0][0]=0，pathArray[0][1]=1，pathArray[0][2]=0，pathArray[0][3]=2

3、确定连通节点在迷宫数组中的坐标
经过初始化的迷宫有了，怎么根据pathArray[][]找到int[][] mazeArray中连通节点的坐标呢？规律如下：
假设输入一组字符串“0,1 0,2”，根据在迷宫中的对应位置分别记为“x1,y1 x2,y2”，那么有：
（1）x1和x2、y1和y2必有一对相等，且不相等的一对数字必定相差1；
（2）连通节点的坐标mazeArray[a][b]：若x1=x2，a=x1*2+1,b=max(y1,y2)*2；若y1=y2，a=max(x1,x2)*2+1,b=y1*2
由此找到了其余连通节点在mazeArray中的坐标，将该位置上的元素重新置为1。

---

# 代码 #
## Test.java ##
![](/myimages/19TWCode_test.png)
## MazeFactory.java ##
![](/myimages/19TW_MazeFactory1.png)
![](/myimages/19TW_MazeFactory2.png)
![](/myimages/19TW_MazeFactory3.png)
![](/myimages/19TW_MazeFactory4.png)
![](/myimages/19TW_MazeFactory5.png)
![](/myimages/19TW_MazeFactory6.png)
## Maze.java ##
![](/myimages/19TWCode_Maze1.png)
![](/myimages/19TWCode_Maze2.png)
![](/myimages/19TWCode_Maze3.png)
## Const.java ##
![](/myimages/19TWCode_Constant.png)

---

# 代码说明 #
总体分为四个类
（1）Test.java——主函数所在类
键盘输入两行字符串，调用下面两个方法分别用来创建Maze对象，生成迷宫字符串mazeText。
Scanner scanner = new Scanner(System.in);
Maze maze = MazeFactory.createMaze(scaleInput,pathInput);
String mazeText =maze.render();

（2）MazeFactor.java——迷宫工厂类
分为两个模块
**创建Maze模块：**
createMaze方法中调用验证模块，对输入字符串进行处理，构造两个二维数组int[][] mazeArray,int[][] pathArray，并将此作为maze方法的参数传入Maze maze = new Maze(mazeArray,pathArray)，返回Maze对象
**验证模块：**
由于第一行和第二行的输入格式内容不同，验证规则也不同，所以分为两个部分。同时，对每一行输入进行4种验证：验证格式错误、验证非连通性、验证无效数字、验证数字超出预定范围

	验证格式错误：用字符串匹配正则表达式实现，两行输入都应当把格式错误作为第一个验证方法
	验证非连通性：对于第一行，非连通性指输入为为“0 0” 或者“1 1”或者“0 1”或者“1 0”，因为这四种情况没有连通性可言；对于第二行，非连通性指“x1,y1 x2,y2”中，x1和x2、y1和y2必有一对相等，且不相等的一对数字必定相差1
	验证无效数字：题目中的解释为“输入的字符串无法正确的转换为数字”，个人认为无效数字也属于格式错误的一种，例如“x1,y1 x2,y2”中有一个字母，因而无法转换为数字，但是要求每一项数字也是格式正确的一方面
	验证数字超出预定范围：判断“x1,y1 x2,y2”中每一个数字都大于0，并且小于对应的行和列

（3）Maze.java——迷宫类

	创建StringBuilder容器，准备将处理后的迷宫二维数组渲染为迷宫字符串mazeText输出；
	比较二维数组pathArray[][]中的元素，用来确定迷宫mazeArray[][]中通路的坐标，即要把哪里重置为1
	遍历迷宫二维数组，将处理后的迷宫二维数组渲染为迷宫字符串mazeText输出

（4）Constant.java——常量类
包含了其他类中用到的常量：
	
	public static final String INCORRECT_COMMAND = "Incorrect command format.";
	public static final String INVALID_NUMBER = "Invalid number format.";
	public static final String OUT_OF_RANGE = "Number out of range.";
	public static final String SCALE_FORMAT_OK = "scaleformat is ok";
	public static final String PATH_FORMAT_OK = "pathformat is ok";
	public static final String SCALE_REGEX1 = "^-?\\d+\\s-?\\d+$";
	public static final String SCALE_REGEX2 = "-?\\d+";
	public static final String PATH_REGEX1 = "^(-?\\d+,-?\\d+\\s-?\\d+,-?\\d+;)*(-?\\d+,-?\\d+\\s-?\\d+,-?\\d+)$";
	public static final String PATH_REGEX2 = "^\\[(-?\\d,\\s){3}-?\\d\\]$";
---

# 测试用例 #
**注意：**
（1）下面用例中，第一行、第二行为输入，下面即为输出；紧接着是另一组的输入......
（2）运行主程序进行测试，首先需要键盘录入两行字符串作为输入，回车即可得到测试输出结果（迷宫或者验证错误信息提示）。

**正常输出用例：**
```
2 3
0,0 0,1;0,1 1,1
[W] [W] [W] [W] [W] [W] [W]
[W] [R] [R] [R] [W] [R] [W]
[W] [W] [W] [R] [W] [W] [W]
[W] [R] [W] [R] [W] [R] [W]
[W] [W] [W] [W] [W] [W] [W]
1 3
0,1 0,2
[W] [W] [W] [W] [W] [W] [W]
[W] [R] [W] [R] [R] [R] [W]
[W] [W] [W] [W] [W] [W] [W]
3 1
0,0 1,0
[W] [W] [W]
[W] [R] [W]
[W] [R] [W]
[W] [R] [W]
[W] [W] [W]
[W] [R] [W]
[W] [W] [W]
3 3
0,1 0,2;0,0 1,0;0,1 1,1;0,2 1,2;1,0 1,1;1,1 1,2;1,1 2,1;1,2 2,2;2,0 2,1
[W] [W] [W] [W] [W] [W] [W]
[W] [R] [W] [R] [R] [R] [W]
[W] [R] [W] [R] [W] [R] [W]
[W] [R] [R] [R] [R] [R] [W]
[W] [W] [W] [R] [W] [R] [W]
[W] [R] [R] [R] [W] [R] [W]
[W] [W] [W] [W] [W] [W] [W]
```

**每一种错误的测试用例：**

```
23
0,0 0,1;0,1 1,1
Incorrect command format.

0,0 0,1;0,1 1,1
Incorrect command format.
2,3
0,0 0,1;0,1 1,1
Incorrect command format.
2 a
0,0 0,1;0,1 1,1
Incorrect command format.
2a
0,0 0,1;0,1 1,1
Incorrect command format.
2,a
0,0 0,1;0,1 1,1
Incorrect command format.
0 0
0,0 0,1;0,1 1,1
Maze format error.
0 1
0,0 0,1;0,1 1,1
Maze format error.
1 0
0,0 0,1;0,1 1,1
Maze format error.
1 1
0,0 0,1;0,1 1,1
Maze format error.
-2 3
0,0 0,1;0,1 1,1
Number out of range.
2 3
a,0 0,1;0,1 1,1
Incorrect command format.
2 3
0,0,0,1;0,1 1,1
Incorrect command format.
2 3
0,0;0,1;0,1 1,1
Incorrect command format.
2 3

Incorrect command format.
2 3
0,0
Incorrect command format.
2 3
a,0 0,1;0,1 1,1
Incorrect command format.
2 3
0,0 0,-1;0,1 1,1
Number out of range.
2 3
0,0 0,2;0,1 1,1
Maze format error.
```

---

# 总结 #
## 遇到的问题 ##
1. 找到输入字符串和输出迷宫的规律之后，就马上着手写了，一个Homework类从头写到尾，运行该主程序直接按照题目给定的输入得到了给定的输出。但其中存在很多问题，没有加验证、没有将各个功能模块分类、代码不是很简洁、存在一些细节上的问题...
2. 后面加了验证，并将功能实现分为了4个类，进行了一些代码细节的优化。这个过程中的一些问题如下
3. 输入格式的问题
	
		最开始以为输入字符串是一行，所以将输入作为用一个字符串变量标记，第一行和第二行的中间用"\r\n"隔开
		问题：但是这样做没有考虑到后面字符串的格式验证，因为第一行和第二行的验证规则是不同的，如果不分开用不同的变量标记，比较难处理
		解决：将输入作为两行，用两个字符串变量标记
4. 对第一行输入的处理问题

		最开始没有对"3 3"进行进一步切割，所以在取两个数字的时候使用了 int | String.charAt(index)-48 得到了int类型的数字
		问题：这样做使用题目给定的测试用例可以得到正确的输出，但是当"m n"中m或n有一个超过两位数字，对char类型-48就不能得到正确的int类型的数值
		解决：将"3 3"进一步用"\\s"切割，得到String[]数组，对数组中的每一个String元素用Integer.parseInt(String元素)就能得到正确的int类型的数值
5. 输入没有进行键盘录入
		
		问题：最开始将输入写成固定的了，不方便进行测试没有进行键盘录入
		解决：在Test类（主类）中加上了扫描器进行键盘录入
6. 字符串常量池

		问题：最开始把所有的字符串直接在功能实现类中写着，看起来不够简洁
		解决：创建了一个Constant类，把用到的字符串常量都定义在里面作为静态常量被调用。包括用到的错误信息展示、flag、正则表达式
7. Maze类的属性的确定
		问题：最开始对迷宫对象的属性不知道怎么定义，即要用哪几个变量唯一确定一个迷宫。
		解决：（1）private int[][] mazeArray;//初始迷宫二维数组
			 private int[][] pathArray;//用来找通路坐标
			（2）根据思路中提到的规律，若把墙"[W]"看作int类型的0，通路"[R]"看作int类型的1，则整个迷宫就是一个二维数组 int[][] mazeArray
			（3）按照规律对二维数组mazeArray进行初始化，得到一部分的通路，其余的通路要由输入的第二行字符串按照相应的规律确定，
				此时就需要对第二行字符串切割后把所有数字存入另一个二维数组int[][] pathArray，对pathArray中的元素进行比较后得到通路[R]在迷宫mazeArray中的坐标
				重置该坐标的元素为1，就能得到最终的迷宫数组了
8. 验证第一行输入过程中的问题

		问题1：4种验证方法的前后顺序需要考虑。不然会引起前面的验证方法把后面的几种错误全都给拦截了的问题（当然这还与验证的规则——正则表达式有关）
		解决1：格式验证一定在第一个；其次是连通性验证；再是无效数字验证；最后是超范围验证。
		
		问题2：格式验证的正则表达式书写。这个正则必须只验证格式，把后面几种错误都放过去。最开始写的时候没有加负号"-"，结果把超范围错误的负数错误给拦截到格式错误中了。
			  包括在第二行输入字符串格式验证的正则表达式中，也出现了同样的问题。
		解决2：在正则中加上了对于负号的验证。

		问题3：关于无效数字的验证。题目中对于无效数字的定义为：不能转换为正确的数字。个人认为无效数字应该也属于格式错误的一种。
			  例如，因为pathArray中有一个元素是字母不能正确转为数字，那么这种输入本身的格式就不对。
		解决3：在无效数字验证方法中，使用和格式验证同样的正则做了匹配验证

		问题4：对于第一行输入非连通性的定义
		解决4：个人认为scaleInput为“0 0” 或者“1 1”或者“0 1”或者“1 0”，这四种情况下没有连通性可言。
9. 验证第二行输入过程中的问题

		问题1：4种验证方法的前后顺序需要考虑。不然会引起前面的验证方法把后面的几种错误全都给拦截了的问题（当然这还与验证的规则——正则表达式有关）
		解决1：格式验证一定在第一个；其次是无效数字验证；再是超范围验证；最后是连通性验证。
		与第一行顺序稍有不同，因为第二行的连通性定义不同：假设有一组输入为"x1,y1 x2,y2"，x1和x2、y1和y2必有一对相等，且不相等的一对数字必定相差1。
		
		问题2：超范围验证的时候，最开始只把x1,y1,x2,y2和row、column进行了比较，没有和0进行比较，因为负数也是超范围的一种
		解决2：分别把x1,y1,x2,y2和0进行了比较。

## 需要改进的地方 ##

1. 验证部分写在了工厂类MazeFactory中，是不是可以把验证单独作为一个类
2. 验证第二行字符串的时候，需要拿到字符串中的每一个数字，于是又在验证模块中把字符串重新处理为二维数组int[][] pathArray，但是这一步在同一类中的createMaze(String scaleInput,String pathInput)方法中已经实现
3. 写验证模块判断条件比较长，例如验证第二行字符串超范围错误的条件用了很多个或"||"