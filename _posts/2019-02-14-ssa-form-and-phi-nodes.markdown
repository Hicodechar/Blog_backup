---
layout: post
title: "SSA Form and PHI Nodes"
date: 2019-02-14T20:26:29+08:00
categories: [LLVM, Tech]
author: cdz
excerpt_separator: <!--more-->
---


本文主要简单介绍 LLVM 中的 `PHINode`。


<!--more-->

最近接触到追踪一个变量在程序中的使用。当对 LLVM IR 中 instruction 进行分析的时候，遇到类似如下的代码：
```llvm
%19 = phi i1 [ false, %entry ], [ true, %land.rhs ]
```

该指令为 `PHINode`，为 llvm 中的一个指令类，继承于 `llvm::Instruction`，具体继承关系和 llvm 源码见：[PHINode 类][phi_node] <br>


<br/>
### （一）、SSA 简介
---
<br/>
在正式介绍 PHI Nodes 之前首先需要了解 `SSA` ，因为所有的 LLVM IR 指令都是使用 `SSA` 形式表示。

<br/>
#### （1）、SSA 
<br/>
`SSA (static single assignment form)` ：**静态一次性赋值**；意思是说**每个变量（虚拟寄存器）都只能被赋值一次**；这样做的好处是可以方便优化代码。

举个例子：
```llvm
 y := 1
 y := 2
 x := y
```
显然，在以上代码中我们能够一眼看出第一行代码是多余的，因为在第三行使用的 $y$ 的值是来自第二行对 $y$ 的赋值，而不是来自第一行对 $y$ 的赋值。而对于程序来说并不能直观的得出第一行是多余的，需要做数据流分析才能判断第一行是多余的。$SSA$ 恰好能够解决该问题，以上代码用 $SSA$ 形式来表示的话：
```llvm
 y1 := 1
 y2 := 2
 x1 := y2
```
很明显，用以上表示方法程序不需要做数据流分析就能够知道第三行使用的 $y$ 是来自第二行的定义。当然，使用 $SSA$ 还能做很多别的优化，我也没有细看，这里就不再赘述了，有兴趣的话可以参考：[维基百科上的介绍][wiki_ssa]

<br/>
#### （2）、生成 SSA 形式的指令
<br/>
我们可以通过探究 $SSA$ 指令的生成知道为什么要引入 `PHI Nodes`。

 把一个普通程序转化为 $SSA$ 形式，最简单的做法就是把每一次的赋值都赋值给一个新的变量（或者说是同一个变量的不同版本），同时在使用变量的时候，使用能够到达该程序点对应的"版本"。
举个例子，在下图的控制流图中，左边图一是原程序流图，右图为按照上述方法生成的 SSA 形式的程序流图。

图一：原程序流图           |  图二：SSA 形式流图（中间版本）
:-------------------------:|:-------------------------:
![](https://upload.wikimedia.org/wikipedia/commons/7/73/SSA_example1.1.png)  |  ![](https://upload.wikimedia.org/wikipedia/commons/f/f7/SSA_example1.2.png)


我们可以发现在右图最底下的基本块中，对于 $y$ 的使用可能来自 $y_1$ 也可能来自 $y_2$，取决于程序是从哪条路径到达该程序点的。为了解决该问题，引入了一条新的语句：$\phi$，该语句会生成一个新的 $y$ 的定义 $y_3$，$y_3$ 的值会根据控制流是从哪条路径到达而选择不同的值 $y_1$ 或 $y_2$:

图三：SSA 形式流图           |  
:-------------------------:|
![](https://upload.wikimedia.org/wikipedia/commons/8/84/SSA_example1.3.png)  |  

上述的 $\phi$ 函数就是我们所要介绍的 `PHI Node`。
<p align="center">
$ y_3 \leftarrow \phi (y_1, y_2) $
</p>
PHI node 根据控制流是从哪一个 block ( $y_1\leftarrow x_2 * 2$ 或 $y_2\leftarrow x_2 -3$) 到达，来决定使用 $y_1$ 或 $y_2$ 赋值给 $y_3$。

<br/>
### （二）、PHI Nodes 简介
---
<br/>
在上一节中我们已经介绍了为什么要引入 `PHI Nodes`；本节主要通过一个例子来讲解 PHI Nodes。

回顾一下 PHI Nodes：
> + PHI Nodes 会记录是从哪个 control-flow 过来的，从而使用相应的值（类似多路复用器）。
> + 并没有实际实现，只是编译器会保证相应的 virtual registers 映射到了同一个 physical register。

举个例子，以下左图为原程序，右图为相应的 control-flow。
<br/>
<p><img src="/image/LLVM/PHINode/phinode_no_ssa.png" width="700"></p>

<br/>
当引入 SSA 形式：
<p><img src="/image/LLVM/PHINode/phinode_ssa.png" width="700"></p>


<br>
对于一个 PHI node 可以表示为：<br>

`%result = phi i32 [value1, BB label1], [value2, BB label2]` <br>

当从 label1 所对应的 basic block(BB) 路径到达 PHI node, 则 result 的值为 value1; 同理,当从 label2 所对应的 basic block路径到达 PHI node, 则 result 的值为 value2。

<br>
### （三）、参考链接
---
[http://www.llvmpy.org/llvmpy-doc/dev/doc/llvm_concepts.html#ssa-form-and-phi-nodes][ref_1] <br>
[https://en.wikipedia.org/wiki/Static_single_assignment_form][ref_2] <br>
[https://cs.uni-paderborn.de/fileadmin/informatik/fg/hit/teaching/SS2017/HWSW-Codesign/02-Compiler-LLVM.pdf][ref_3] <br>
[https://llvm.org/doxygen/classllvm_1_1PHINode.html][ref_4] <br>


[phi_node]: https://llvm.org/doxygen/classllvm_1_1PHINode.html
[wiki_ssa]: https://en.wikipedia.org/wiki/Static_single_assignment_form
[ref_1]: http://www.llvmpy.org/llvmpy-doc/dev/doc/llvm_concepts.html#ssa-form-and-phi-nodes 
[ref_2]: https://en.wikipedia.org/wiki/Static_single_assignment_form
[ref_3]: https://cs.uni-paderborn.de/fileadmin/informatik/fg/hit/teaching/SS2017/HWSW-Codesign/02-Compiler-LLVM.pdf 
[ref_4]: https://llvm.org/doxygen/classllvm_1_1PHINode.html 