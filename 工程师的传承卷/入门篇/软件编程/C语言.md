# <font color=dodgerblue> C语言</font>

## 编程重要的是思维而不是语法
**在开始学习编程之前，首先我们要先明确一件事，就是学编程重要的是思维而不是语法。**  
语言只是工具，思维才是本质。  
工具只是便捷你的工作与提高你的效率，但真正决定你能否解决问题的是你的思维方式.  
就好比做几何题的时候,垂直平行等那套数学符号语言（工具）真的重要吗?  
那只是一种表述方式而已，不会那套数学符号还可以写文字描述和算式。  
决定你最终能否解答这道题的还是你是否有解题的思路。

编程也是如此。

## 模块化概念
在编程学习开始之前，我们还需要了解一个概念，就是模块化。

c语言是一个模块化的语言，这个模块化体现在很多方面，比如函数，比如结构体，比如多文件。  
一个c语言程序，就是由一个个变量拼成结构体，一个个结构体与函数拼成文件，最后再由一个个文件拼成最后的整个程序

任何一个C语言程序都是由一个或者多个程序段构成，每个程序段分别负责各自的功能，最后由主程序段统合到一起形成可以执行的程序。  ——这种负责某一部分功能的程序段我们通常称之为“函数”。

## 主函数
电脑计算机执行程序总要有个开始，总要有个第一行。  
前面提到过c语言中每个程序段分别负责各自的功能，那么计算机执行的时候又怎么能知道先执行哪里后执行哪里呢？  
为了解决这一问题，我们需要有一段程序来做各个程序段的统合

这个程序也就是`main`函数  
也就是说，如果你写的程序要运行的话，一定要有一个main函数  
那么`main`函数如何写呢？  
为了解决这一问题，我们需要了解一下函数的结构。

## 函数的结构

前面提到，一个c语言程序，就是由一个个变量拼成结构体，一个个结构体与函数拼成文件，最后再由一个个文件拼成最后的整个程序。

那么函数是怎么样的呢？这里我们以不可或缺的主函数为例

```c
void main()
{    
    //代码内容    
}
```

这就是一个最简化的函数（当然我们通常不用void，这里只是为了方便理解）

这个函数的结构是这样的

```c
void main()   // 返回值类型 函数名（参数列表）       表示创建一个函数
    {    //代码块开始
    	//代码块可以简单理解为我们要运行的一段代码
	}	//代码块结束
```

这里的void是指返回值的类型,void表示没有返回值，为什么函数需要返回值呢？因为我们在执行一个函数的时候通常是需要它来实现某个功能的，如果没有返回值我们就不知道它有没有成功执行，或者不知道它的执行结果了（譬如执行开平方以后我们没有收到返回值，平方是开完了，但是结果捏？？？？？）

而且有些时候我们的系统在执行函数的时候也强制要求返回值（一些系统，不是所有系统）

综上，我们的程序最好提供一个返回值

因此在事实上我们的一个函数表达出来其实是这样的

```c
类型名 main(){
    //函数内容    
    return 数值;
}
```

这里的类型名常用的有 int char double long short float bool

分别对应为整数，字符，双精度浮点数，长数，短数，浮点数，浮点数就是带小数点的数，计算机处理浮点数有误差，double比float精准一些

这里我们所写的函数名字叫main，也就是前文提到的主函数

由于是主函数，所以我们的返回值自己是没有办法用到了，但是有的计算机系统可能会要求，因此通常的写法是返回一个0，代表程序正确执行完成，此时我们的程序就变成了

```c
int main()    //创建一个返回值为整数的函数 名为main 传入参数列表为空
{
    //函数内容 
    return 0；  //  返回整数 0 
}
```

这里我们运行一下这个程序给大家看看结果

（展示）因为我们函数内容什么都没有写，所以也什么都没有显示

那么怎么证明我们的程序真的执行成功了呢？我们再增加一行用于显示的指令

```c
#include <stdio.h>

int main()    //创建一个返回值为整数的函数 名为main 传入参数列表为空
{
    printf("hello world")；//显示hello world
    return 0；  //  返回整数 0 
}

```
**注意！C语言函数里的语句结束后需要以英文分号结尾，每一句都需要**


printf是c语言里的打印（显示到屏幕）语句，这句话的意思是显示hello world

加上之后我们再来运行一下(展示)程序就会显示输出hello world
加上以后我们的命令后就显示出了我们要显示的内容，这证明本喵刚才认真讲了没有胡说八道（骄傲）

可能刚才有银还意识到一个问题，就是刚才的程序比上面说的多了一行

```c
#include <stdio.h>
```

这一句的意思又是什么呢？

\#后面加的字符是c语言里的预处理指令，就是编译器程序执行前预先处理的指令，用来补足程序运行中需要的一些东西

这个#include就是包含头文件的意思

`#include <stdio,h>`

的意思就是这个程序要包含文件stdio.h里面的内容，stdio.h是c语言编译器自带的一个头文件，包含了输入输出的函数之类的一些常用的功能函数，printf（）其实也是一个函数，它在stdio文件里面，我们使用的printf就是调用了程序外部文件stdio.h里面的print函数。

刚才咱们已经说了，c语言由很多函数构成，下面咱们再定义一下别的函数来个多函数的程序

```c
#include 

int die()
{
    printf(" bay~ world " );	
    return 0;
}


int main()
{
    die();
    return 0;
}


```

由于这个不是主函数了所以函数名就可以自己起了，不过这里要注意，函数名不能和关键字（c语言里面已经被占用的词）同名

这里我们起名die

这里左下角就正确显示了

这里是先定义了die函数然后在主函数里面调用了它

```c
die();
```

就酱，我们写了一个多个函数的c语言程序，（在学习编程的初期可以先不用考虑多文件，先从一个文件写起）

**这里要注意！我们使用的程序要在使用的地方之前出现，例如dnlm函数在main函数之前**

然后下面我们讲一坨选择分支结构
## 选择分支


这个选择分支结构它是这个样子的

```c
if()
{
    //如果条件成立执行
}
else
{
    //如果条件不成立执行
}
```

这里我们添加一个丢乃老父并且声明母亲父亲两个整数来测试一下

```c
#include 

int dnlm()
{	
    int a=30;//声明整数a等于30	
    int b=20;    //声明整数b等于20	
    if a>b)    //如果a大于b	
    {
        printf(" a" );    //显示	
    }
    else    //否则
    {
        printf(" b" );    //显示
    }	
    return 0;
}

int main()
{
    dnlm();
    return 0;
}


```

运行结果正确的显示在了左下角。

这样一来证明我们的选择结构发挥了它应有的效果

刚才讲完了选择分支结构，这次我们从循环结构开始讲，一课时之内把c语言入门讲完，然后看看时间能不能够讲一些其他的东西。

## 循环结构

首先是while循环，这是平常会经常用到的一个循环

```c
while(循环条件){    //代码内容}
```

while循环的结构是这样的，首先判断循环条件是不是成立，例如循环条件是变量a>=2,那么当a大于等于2的时候，就会执行代码内容一次，然后再次判断a是不是还大于等于2，如果是的话再执行一次。直到有一次发现a不再大于等于2了，才结束循环开始执行循环外面的内容。这样一来的话，实际上也就是说，我们在写这种循环的时候，里面一定要写改变循环条件的代码，否则循环条件一直不变就变成死循环了。
while循环是先判断然后才决定执不执行的，所以while循环最少会执行0次，也就是说如果一开始条件就不成立的话就一次都不执行。
与之相对的是do。。。while循环   

```c
do
{
//代码内容
}while() 
```  

do。。。while和while相反，dowhile是先做了再说，做一遍到做完以后再判断，是不是条件成立 ，条件成立的话继续循环，条件不成立的话到此为止，继续往下执行，do while循环是至少会执行一次的循环。  

与while系列循环用的同样多的就是for循环了  

```c
for(i=100;i>=0;i--){    //代码内容}
```

for循环的条件分为三部分，第一部分是声明控制循环的变量，比如声明个i=100，第二个部分和while的控制条件一样是用来判断循环执不执行,最后一个部分是用来声明控制变量的变化，就和while一样，for也需要改变控制条件来防止死循环，不过for直接在控制条件里就可以写好改变控制变量的语句。

以上是循环结构的三种语句。

## 跳转语句

下面进行跳转语句

c语言里的跳转语句其实就是goto

语法也很简单，在任意一个地方立下flag，然后就可以随时goto到这个地方了

```c
lable1:
  //此处省略两百万亿行
goto lable1
```

就可以直接飞回flag

goto用的好的话不仅可以跳转还可以实现各种循环，不过goto很容易出现不知道飞到了哪里去但是编译器不报错导致查错人员头比地球还大的现象，因此一般的来说，不建议使用goto语句。

## 数组

数组部分也很简单，其实就是把数据连起来存

我们平常声明一个变量a，可以储存一个数据。

我们现在声明一个数组a[100],就可以储存100个数据

```c
数据类型 数组名[数组长度]
```

可以通过a[0-99]来分别使用这100个数据，就不用写100遍声明变量了，而且如果我们要查找的数有好几个特点，还可以使用二维或者多维数组。

比如a[1][2][3][4][5][6][7]就是七维数组里第二组的第三小组的第四小组的第五小组的第六小组的第七小组的第八个数

这里要注意<font color=red>数组里面排序序号是从零开始的，第一个内容的编号是0，所以数组名加数字实际访问到位置是数字加一的位置存的数据。</font>例如`a[0]`其实是数组a的第一位。

从本质上而言，数组其实就是一种指针的应用方式。

## 指针 

语言里面指针是核心精髓所在，所谓指针，就是指向数据储存的地方。

我们平常声明一个变量，a=100，大概就相当于在白纸上划了一部分叫做a区，然后往里面记录了一个数字100,100写在a这个地方。

这块a区所在的位置就叫做a的地址

c语言里面和地址有关的有两个符号，一个是`&`，取地址符，一个是`*`，是指针的标记(也是解引用符号)

我们平常创建变量的时候是这样的

```c
int a;
```

我们创建指针变量时候是这样的

```c
int* p;
```

标上一个\*

表示这个变量p存储的是int变量的地址。

**注意：指针和指针变量是不同的 ！指针是地址，指针变量是存放指针的变量**

我们平常给变量赋值的方法是这样的

```c
a=100;
```

我们给指针变量赋值的时候是这样的

```c
p=&a;
```

这里的&a的意思就是取得a的地址

看到这里大家可能会有所疑惑，这个指针和变量到底有什么区别呢？

这里举一个简单的例子来说明指针和直接调用变量的区别

坐在隔壁的小明想要抄你的试卷，他看了一下a区，把数字抄走了，这是b=a，

坐在隔壁的小明，他拿走了你的试卷，这是b=&a

前者只是拿走了数据，后者拿走的数据的地址，那么这会导致什么呢？  

当小明（b）想要修改卷子的时候——  

前者，b修改了卷子上的数值，a的卷子没事，因为b只是修改了抄走的一个数据

后者，b修改了卷子上的数值，a惊叫一声：卧槽你把我卷子改了干啥！！！

这就是使用变量的值和使用变量的指针的区别

**BUT！！！为什么我们需要使用指针呢？？？？**

当然是因为**<font color=red>有些时候我们必须使用指针!</font>**

举个例子！比如我们想要在某个函数里修改函数外的值的时候，我们就必须得使用指针。

这里我们需要涉及一点关于函数调用、形参与实参的知识。

有些时候我们要用的函数会需要传入一些数值，比如我们写一个求和的函数

```c
int sum(int a,int b)  //声明一个返回值是整数的函数，使用时需要传入整数a和整数b
{
	return a+b; //返回a+b的值
}
```

然后我们在使用的时候就需要传入两个值

```c
sum(x,y) //x和y是之前已经弄好的存了值的变量
sum(10,20)//或者这样直接给两个数值
```

这里面的a和b就叫形参，也可以简单粗略地理解为参数所需的形式，x和y就是实参，可以简单粗略的理解为实际传入的参数。

在程序运行到调用sum函数的时候就会创建两个临时的变量a和b，然后把x和y的值传给a和b。

这里就出现了不使用指针无法解决的问题——如果我们想要把存储的结果还存在x里呢？

当然，这样其实还能解决，`x=sum(x,y)`就行了，但是如果我们要存到的地方不确定呢？比如根据sum计算结果的不同存到不同的地方，是不是没有办法啦！

而使用指针就可以很方便（才怪）的解决这个问题，我们不传入值，而是把地址传过去，小明不久可以直接修改你的试卷了吗？

```c
#include <stdio.h>
int main(int argc, char const *argv[])
{
	int x=1;
	int y=2;
	sum (&x,&y);
	return 0;
}

int sum(int* a,int* b)
{
	*a=*a+*b;
	return 0;
}
```



这样就完美的解决了这个问题。

那么这里出一道小题，如果想要修改指针变量int* a的值应该怎么办呢？

```c
int sum(int** a,int** b)
{
	*a=*a+*b;
	return 0;
}
```



 当然是传入指针的指针！

指针是可以一层套一层的，`int************************************************ a`都可以！（当然，没什么大病的话一般是不会写太多层的）

回到刚才，讲指针的时候我们有说数组是指针的一种应用，是什么意思呢？现在我们就可以解答这个问题了，数组就是根据你的需求创建了好多挨在一起的空间，然后数组的名字就是一个指针，指向第一块地方，然后数组[]里跟不同的数字就是把指针往后移不同的长度，指到不同的地方。

比如a[3]，其实也就是\*(a+3)的意思，你把a[3]写成`*(a+3)`,`*(3+a)`,`3[a]`都可以的，都是一样的东西。

这里也解答了为什么数组前面要加int double之类的类型的问题，因为声明数组的时候需要创建出一些地方来存数据，不同类型数据大小不一眼需要的空间也不一样，数组前面的数据类型是用来标识每一块地方多大的。

追求技术是一场漫长的旅程，很高兴在这里与你们相逢。

