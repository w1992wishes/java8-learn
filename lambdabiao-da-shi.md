### 一、什么是Lambda表达式？

可以把Lambda表达式理解为**简洁**地表示**可传递的匿名函数的一种方式**：它没有名称，但它有参数列表、函数主体、返回类型，可能还有一个可以抛出的异常列表。

* 匿名——它不像普通的方法那样有一个明确的名称

* 函数——和方法一样，Lambda有参数列表、函数主体、返回类型，还可能有可以抛出的异常列表

* 传递——Lambda表达式可以**作为参数传递给方法或存储在变量**中

* 简洁——无需像匿名类那样写很多模板代码

---

### 二、为什么要用Lambda表达式？

想要回答这个问题，应该先了解一下什么是函数式编程。下面是摘自知乎工程师的回答：

```
编程范式
函数式编程是一种编程范式，我们常见的编程范式有命令式编程（Imperative programming），函数式编程，逻辑式编程，常见的面向对象编程是也是一种命令式编程。
命令式编程是面向计算机硬件的抽象，有变量（对应着存储单元），赋值语句（获取，存储指令），表达式（内存引用和算术运算）和控制语句（跳转指令），一句话，命令式程序就是一个冯诺依曼机的指令序列。
而函数式编程是面向数学的抽象，将计算描述为一种表达式求值，一句话，函数式程序就是一个表达式。
函数式编程的本质
函数式编程中的函数这个术语不是指计算机中的函数（实际上是Subroutine），而是指数学中的函数，即自变量的映射。也就是说一个函数的值仅决定于函数参数的值，不依赖其他状态。比如sqrt(x)函数计算x的平方根，只要x不变，不论什么时候调用，调用几次，值都是不变的。
在函数式语言中，函数作为一等公民，可以在任何地方定义，在函数内或函数外，可以作为函数的参数和返回值，可以对函数进行组合。
纯函数式编程语言中的变量也不是命令式编程语言中的变量，即存储状态的单元，而是代数中的变量，即一个值的名称。变量的值是不可变的（immutable），也就是说不允许像命令式编程语言中那样多次给一个变量赋值。比如说在命令式编程语言我们写“x = x + 1”，这依赖可变状态的事实，拿给程序员看说是对的，但拿给数学家看，却被认为这个等式为假。
函数式语言的如条件语句，循环语句也不是命令式编程语言中的控制语句，而是函数的语法糖，比如在Scala语言中，if else不是语句而是三元运算符，是有返回值的。
严格意义上的函数式编程意味着不使用可变的变量，赋值，循环和其他命令式控制结构进行编程。
从理论上说，函数式语言也不是通过冯诺伊曼体系结构的机器上运行的，而是通过λ演算来运行的，就是通过变量替换的方式进行，变量替换为其值或表达式，函数也替换为其表达式，并根据运算符进行计算。λ演算是图灵完全（Turing completeness）的，但是大多数情况，函数式程序还是被编译成（冯诺依曼机的）机器语言的指令执行的。
```

可以参考下面两个链接：

[https://www.zhihu.com/question/28292740](https://www.zhihu.com/question/28292740)

[http://janfan.cn/chinese/2015/05/18/functional-programming.html](http://janfan.cn/chinese/2015/05/18/functional-programming.html)

---


