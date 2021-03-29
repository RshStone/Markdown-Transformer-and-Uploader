# 									Nand to Tetris

# Week2

## 为什么学这章的内容及学习目标

围绕着电脑计算展开，现在的我们知道CPU是计算机大量计算的关键部分，而在CPU内最关键处理计算的Chip是一个叫ALU(Arithmetic Logic Unit)中文名叫算术逻辑单元这样的一个东东，它的作用是处理字节运算和逻辑运算。Week2这章的内容目的是设计一个简单的ALU(属于From Nand to Tetris的一个ALU)，这个ALU是专门属于你的，也是From Nand to Tetris这门课想要带给你,它被大量简化，只有加减法的计算功能。所以，难度适中，不用担心学不会的问题，但学习这个过程中带给你的爽感就像你真的设计了一个ALU，乃至未来的PC。

## 这章的内容以什么形式呈现

既然是计算，最简单的模型莫过于，两个分别为1个bit的信息进行相加计算了。这个模型抽象具体化为In a, b    Out sum,carry

输出的结果分别为1bit的内容，carry代表的是进位，sum表示二进制相加的most significant的一位。我们将这个模型抽象为一个叫做HalfAdd的Chip。

计算不可能只计算一位吧，我们计算的内容可能是很多位进行相加。那么想想会遇到什么情况呢？当只有1位进行相加的时候，我们发现是HalfAdd的情况，But,wait.如果有很多位呢？我们将多位的情况先从最简单的方式进行思考，也就是两位的情况，会发生什么呢？我们发现如果carry为0的时候，就是HalfAdd的模型，但是当carry等于1的时候，那就相当于是有三个分别为1bit的内容进行加法计算了。

于是，有了FullAdd的模型Chip。In a,b,c Out sum, carry 

有了FullAdd，你就可以进行设计多个bits的计算了。这里引入Add16，因为我们这门课最终目标是设计一个简单的16位的Hack computer.

以上都是加法运算，我们从小学就知道人手动计算的时候，我们经常将减法简化成加法进行计算，在这里，我们采用类似的思想，但是计算机的减法如何实现呢？难不成我还需要设计Subtract的Chip吗？

一个很有意思的技巧是我们将多个Bits的数的第一位作为符号位，0表示正数，1表示负数，这样对原先为n个Bits的Chip,它的取值范围为0~2^n-1。再引入符号位的概念后，它的取值范围就变成了-2^(n-1) ~ 2^(n-1)-1这样的一个取值范围。不过呢，这样表示还是有一点问题，举个例子，对于4-bit binary system ,1 + (-2)的结果该怎么表示呢？如果直接按照取负的思想，相加的结果为1011，对应的数为-3，不过我们都知道这个算术计算的结果应该为-1，也就是1001，问题出在了哪里？如何解决这个问题？

这里引入了一个2's complement method(补码)的一个思想方法，这里的补码可以理解为取负的意思，注意和Bitwise negation区别开来。

<img src="https://raw.githubusercontent.com/RshStone/Markdown-Transformer-and-Uploader/mynote/Notes/ALU/001.png" alt="2's complement method" style="zoom:100%;" />



这样问题就迎刃而解了，具体为什么，如果你对着感兴趣的话，可以自行学习下。

之后再看一看ALU的功能

<img src="https://raw.githubusercontent.com/RshStone/Markdown-Transformer-and-Uploader/mynote/Notes/ALU/002.png" alt="ALU" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/RshStone/Markdown-Transformer-and-Uploader/mynote/Notes/ALU/003.png" alt="Function of ALU" style="zoom:100%;" />

最后还有一个Inc16的chip(这里的Inc其实是Incrementor的缩写),其功能是在16bits输入的情况下，输出In+1的情况。

## ALU实现的思路：

zx,nx和zy,ny模块其实是类似等价的。nx模块用Not16 chip,zx module用 Mux16(a = , b = false,sel = zx,out =  )来实现；

f的条件选择如何实现呢？ 我想到了Mux16(a = x & y, b= x + y, sel = f, out= )

no module 用Not16 chip和 Mux16进行选择判断

最后根据out的结果再选择判断对zr , ng 进行输出

## 总结

这节最好玩的地方在于，构建的Logic gate难度加大了，HalfAdd, FullAdder, Adder16,Inc,ALU大概是这些了。难度加大，能不能不重复造轮子，利用第一次作业里的基本的chip很关键，比如说Mux,Xor等。

我自己在Bitwise negation这里卡了下，有时候我下意识地将Bitwise negation当作取负了，有时候没有，概念有点混乱。

还是挺有意思的，虽然一个人在图书馆啊~。今天科三过了，hahahaha~.

