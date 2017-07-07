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

    函数式编程是一种编程范式，我们常见的编程范式有命令式编程（Imperative programming），函数式编程，逻辑式编程，常见的面向对象
编程也是一种命令式编程。
    命令式编程是面向计算机硬件的抽象，有变量（对应着存储单元），赋值语句（获取，存储指令），表达式（内存引用和算术运算）和控制语
句（跳转指令），一句话，命令式程序就是一个冯诺依曼机的指令序列。而函数式编程是面向数学的抽象，将计算描述为一种表达式求值，一句话，
函数式程序就是一个表达式。

    函数式编程的本质

    函数式编程中的函数这个术语不是指计算机中的函数（实际上是Subroutine），而是指数学中的函数，即自变量的映射。也就是说一个函数的
值仅决定于函数参数的值，不依赖其他状态。比如sqrt(x)函数计算x的平方根，只要x不变，不论什么时候调用，调用几次，值都是不变的。
    在函数式语言中，函数作为一等公民，可以在任何地方定义，在函数内或函数外，可以作为函数的参数和返回值，可以对函数进行组合。
    纯函数式编程语言中的变量也不是命令式编程语言中的变量，即存储状态的单元，而是代数中的变量，即一个值的名称。变量的值是不可变的
（immutable），也就是说不允许像命令式编程语言中那样多次给一个变量赋值。比如说在命令式编程语言我们写“x = x + 1”，这依赖可变状态的
事实，拿给程序员看说是对的，但拿给数学家看，却被认为这个等式为假。
    函数式语言的如条件语句，循环语句也不是命令式编程语言中的控制语句，而是函数的语法糖，比如在Scala语言中，if else不是语句而是
三元运算符，是有返回值的。
    严格意义上的函数式编程意味着不使用可变的变量，赋值，循环和其他命令式控制结构进行编程。
    从理论上说，函数式语言也不是通过冯诺伊曼体系结构的机器上运行的，而是通过λ演算来运行的，就是通过变量替换的方式进行，变量替换为
其值或表达式，函数也替换为其表达式，并根据运算符进行计算。λ演算是图灵完全（Turing completeness）的，但是大多数情况，函数式程序
还是被编译成（冯诺依曼机的）机器语言的指令执行的。
```

可以参考下面两个链接：

[https://www.zhihu.com/question/28292740](https://www.zhihu.com/question/28292740)

[http://janfan.cn/chinese/2015/05/18/functional-programming.html](http://janfan.cn/chinese/2015/05/18/functional-programming.html)

大致了解了函数式编程后，就能理解Lambda表达式的优势。

* 函数式编程是技术的发展方向，而Lambda是函数式编程最基础的内容，所以，Java 8中加入Lambda表达式本身是符合技术发展方向的。

* 通过引入Lambda，最直观的一个改进是，不用再写大量的匿名内部类。事实上，还有更多由于函数式编程本身特性带来的提升。比如：代码的可读性会更好、高阶函数引入了函数组合的概念。

* 此外，因为Lambda的引入，集合操作也得到了极大的改善。比如，引入stream API，把map、reduce、filter这样的基本函数式编程的概念与Java集合结合起来。在大多数情况下，处理集合时，Java程序员可以告别for、while、if这些语句。

* 随之而来的是，map、reduce、filter等操作都可以并行化，在一些条件下，可以提升性能。

---

### 三、lambda表达式的结构 {#lambda表达式的结构}

* 参数可以是零个或多个
* 参数类型可指定，可省略（根据表达式上下文推断）
* 参数包含在圆括号中，用逗号分隔
* 表达式主体可以是零条或多条语句,包含在花括号中
* 表达式主体只有一条语句时，花括号可省略
* 表达式主体有一条以上语句时，表达式的返回类型与代码块的返回类型一致
* 表达式只有一条语句时，表达式的返回类型与该语句的返回类型一致

```
//零个
 ()-> System.out.println("no argument");

//一个
  x->x+1

//两个
  (x,y)->x+y


//返回类型是表达式主体语句的返回类型int
  (int x)->x+1

//表达式中有多条语句
  (Apple appple) -> {
    System.out.println(apple.getWeight());
    System.out.println(apple.getColor());
  }
```

---

### 四、哪里使用及如何使用Lambda表达式

对于java 8，应该在函数式接口上使用Lambda表达式。

#### 4.1、什么是函数式接口？

函数式接口就是只定义一个抽象方法的接口。（PS: java 8中接口有一点变化，就是接口中可以有除了抽象方法之外的默认方法，用default修饰）

```
public interface Comparator<T> {
int compare(T o1, T o2);
}

public interface Runnable{
void run();
}
```

函数式接口可以干什么？

Lambda表达式允许直接以内联的形式为函数式接口的抽象方法提供实现，并把整个表达式作为函数式接口的实例（具体说来，是函数式接口一个具体实现的实例。用匿名内部类也可以完成同样的事情，只不过比较笨拙：需要提供一个实现，然后再直接内联将它实例化。

看个例子：

```
//使用Lambda表达式
Runnable r1 = () -> System.out.println("Hello World 1");     

//使用内部类
Runnable r2 = new Runnable(){
    public void run(){
        System.out.println("Hello World 2");
    }
};
```

#### 4.2、函数描述符

如何从函数式接口到Lambda表达式，这需要引入函数描述符的概念。

函数式接口的抽象方法的签名基本上就是Lambda表达式的签名。我们将这种抽象方法叫作函数描述符。

具体分析一下：

Runnable只有一个run\(\)方法，具体形式是 void run\(\)，代表不接受参数也不返回值，将这个函数描述符和Lambda表达式的结构对应起来，\(\)代表没有参数，System.out.println\("Hello World 1"\)表示不返回值，所以可以得到：

```
Runnable r1 = () -> System.out.println("Hello World 1");
```

Comparator有一个compare\(\)方法，具体形式是int compare\(T o1, T o2\)，代表接受两个参数，返回一个整形，将这个函数描述符和Lambda表达式的结构对应起来，\(T o1, T o2\)代表参数列表，{if \(o1.value &gt; o2.value\){ retrun 1} else{return -1} }，可以得到：

```
Comparator comp = (T o1, T o2) -> { if (o1.value > o2.value){ retrun 1} else{return -1} }
```

#### 4.3、java 8新引入的函数式接口

##### 4.3.1、Predicate

java.util.function.Predicate&lt;T&gt;接口定义了一个名叫test的抽象方法，它接受泛型T对象，并返回一个boolean。

```
//Predicate接口
public interface Predicate<T>{
    boolean test(T t);
}

public static <T> List<T> filter(List<T> list, Predicate<T> p) {
    List<T> results = new ArrayList<>();
    for(T s: list){
        if(p.test(s)){
        results.add(s);
        }
    }
    return results;
}

Predicate<String> nonEmptyStringPredicate = (String s) -> !s.isEmpty();
List<String> nonEmpty = filter(listOfStrings, nonEmptyStringPredicate);
```

##### 4.3.2、Consumer

java.util.function.Consumer&lt;T&gt;定义了一个名叫accept的抽象方法，它接受泛型T的对象，没有返回（void）。

如果需要访问类型T的对象，并对其执行某些操作，就可以使用这个接口。比如，可以用它来创建一个forEach方法，接受一个Integers的列表，并对其中每个元素执行操作。

```
// Consumer接口
public interface Consumer<T>{
    void accept(T t);
}

public static <T> void forEach(List<T> list, Consumer<T> c){
    for(T i: list){
        c.accept(i);
    }
}

forEach(Arrays.asList(1,2,3,4,5),(Integer i) -> System.out.println(i));
```

##### 4.3.3、Function

java.util.function.Function&lt;T, R&gt;接口定义了一个叫作apply的方法，它接受一个泛型T的对象，并返回一个泛型R的对象。如果需要定义一个Lambda，将输入对象的信息映射到输出，就可以使用这个接口（比如提取苹果的重量，或把字符串映射为它的长度）。

```
//Function接口
public interface Function<T, R>{
    R apply(T t);
}

public static <T, R> List<R> map(List<T> list,Function<T, R> f) {
    List<R> result = new ArrayList<>();
    for(T s: list){
            result.add(f.apply(s));
        }
    return result;
}

// [7, 2, 6]
List<Integer> l = map(Arrays.asList("lambdas","in","action"),(String s) -> s.length());
```

上面的函数式接口只适用于引用对象，对于基本数据类型，java 8引入了IntPredicat、DoublePredicat、IntConsume、IntFunctio等函数式接口，可以避免进行装箱拆箱的操作。

PS：请注意，任何函数式接口都不允许抛出受检异常（checked exception）。如果需要Lambda表达式来抛出异常，有两种办法：定义一个自己的函数式接口，并声明受检异常，或者把Lambda包在一个try/catch块中。

---

### 五、类型检查、类型推断以及限制

#### 5.1、类型检查和类型推断

Lambda表达式可以为函数式接口生成一个实例。然而，Lambda表达式本身并不包含它在实现哪个函数式接口的信息，也就是说同一个Lambda表达式可以是多个函数式接口的实例，这里面就涉及到了类型检查和类型推断。

编译器负责推导 lambda 表达式类型。它利用 lambda 表达式所在上下文所期待的类型进行推导，这个**被期待的类型被称为目标类型**。通过一个例子看看。

```
public class Test1 {

    public static void main(String[] args) {
        List<String> strs = new ArrayList<String>();
        strs.add("aaa");
        strs.add("bbbb");
        strs.add("ccccc");
        strs.add("dddddd");

        //长度小于5的str
        List<String> shortStr = filter(strs, (String str) -> str.length() < 5);

        // [aaa, bbbb]
        System.out.println(shortStr);
    }

    public static List<String> filter(List<String> strs, Predicate<String> p){
        List<String> filterStr = new ArrayList<>();
        for (String str : strs){
            if (p.test(str)){
                filterStr.add(str);
            }
        }
        return filterStr;
    }
}
```

#### ![](/assets/1.png)5.2、同样的Lambda，不同的函数式接口

有了目标类型的概念，同一个Lambda表达式就可以与不同的函数式接口联系起来，因为同一个Lambda表达式可以推导出不同的类型，只要类型检查通过，它们的抽象方法签名能够兼容。

比如，Callable和PrivilegedAction，这两个接口都代表着什么也不接受且返回一个泛型T的函数。 因此，下面两个赋值是有效的：

```
Callable<Integer> c = () -> 42;

PrivilegedAction<Integer> p = () -> 42;
```

第一个赋值的目标类型是Callable&lt;Integer&gt; ， 第二个赋值的目标类型是PrivilegedAction&lt;Integer&gt;。

#### 5.3、Lambda表达式参数类型推断

Java编译器不仅可以从上下文（目标类型）推断出用什么函数式接口来配合Lambda表达式，它也可以推断出适合Lambda的签名。

这样做的好处在于，编译器可以了解Lambda表达式的参数类型，这样就可以在Lambda语法中省去标注参数类型。换句话说，Java编译器可以推断Lambda的参数类型。

```
List<String> shortStr = filter(strs, (str) -> str.length() < 5);

Comparator<String> c = (str1, str2) -> str1.length().compareTo(str2.length());
```

但并不是说省略参数类型就是好的，有时候显式写出类型更易读，有时候去掉它们更易读。这需要判断。

### 六 、Lambda表达式作用域

先看一段代码：

```
package com.wan.test

public class Test2 {
    Runnable r1 = () -> System.out.println(this);
    Runnable r2 = () ->  System.out.println(toString());
    Runnable r3 = new Runnable() {
        @Override
        public void run() {
            System.out.println(this);
        }
    };
    Runnable r4 = new Runnable() {
        @Override
        public void run() {
            System.out.println(toString());
        }
    };
    public String toString() {  return "Hello, world"; }
    public static void main(String... args) {
        new Test2().r1.run();
        new Test2().r2.run();
        new Test2().r3.run();
        new Test2().r4.run();
    }
}
```

结果是：

```
Hello, world
Hello, world
com.wan.test.Test2$1@41629346
com.wan.test.Test2$2@404b9385
```

Lambda表达式和内部类呈现出不同的结果，是因为：

* 内部类中通过继承得到的成员（包括来自`Object`的方法）可能会把外部类的成员掩盖（shadow），此外未限定（unqualified）的`this`引用会指向内部类自己而非外部类。
* 相对于内部类，lambda 表达式的语义就十分简单：它不会从超类（supertype）中继承任何变量名，也不会引入一个新的作用域。lambda 表达式基于词法作用域，也就是说 lambda 表达式函数体里面的变量和它外部环境的变量具有相同的语义（也包括 lambda 表达式的形式参数）。此外，’this’ 关键字及其引用在 lambda 表达式内部和外部也拥有相同的语义。



