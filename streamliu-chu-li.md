### 一、前言

有一个池塘，池塘里面有各种各样的鱼虾，假如我想找到所有大于2斤小于3斤的草鱼的名字（假如鱼有名字）。

这个需求很简单，映射到数据库中可以这样查询：

SELECT name FROM tbl\_fishes WHERE kind = 'grass carp' AND weight &lt; 3 AND WHERE weight &gt; 2

这条SQL语句简单明了，剩下的就交给数据库搞定了，那用java如何实现呢？

给定一个集合，一遍一遍的迭代，似乎别无他法，而且还要考虑并行，实在是头疼，但是java 8流处理出来后，问题就简单多了。

```
fishes.stream()
    .filter(f -> f.getWeight < 3)
    .filter(f -> f.getWeight > 2)
    .filter(f -> f.getKind().equals("grass carp")
    .map(f -> f.getName())
```

而且还可以并行处理。

### 二、流简介

#### 2.1、什么是流

流到底是什么呢？简短的定义就是“**从源生成的支持数据处理操作的元素序列**”。

* 元素序列——就像集合一样，流也提供了一个接口，可以访问特定元素类型的一组有序值。因为集合是数据结构，所以它的主要目的是以特定的时间/空间复杂度存储和访问元素（如ArrayList 与 LinkedList）。但流的目的在于表达计算，比如、filter、sorted和map。集合讲的是数据，流讲的是计算。

* 源——流会使用一个提供数据的源，如集合、数组或输入/输出资源。 请注意，从有序集合生成流时会保留原有的顺序。由列表生成的流，其元素顺序与列表一致。

* 数据处理操作——流的数据处理功能支持类似于数据库的操作，以及函数式编程语言中的常用操作，如filter、map、reduce、find、match、sort等。流操作可以顺序执行，也可并行执行。

上java 8实战中的代码说明：

```
public class Test1
{
    public static void main(String[] args) {
        List<Dish> menu = Arrays.asList(
                new Dish("pork", false, 800, Dish.Type.MEAT),
                new Dish("beef", false, 700, Dish.Type.MEAT),
                new Dish("chicken", false, 400, Dish.Type.MEAT),
                new Dish("french fries", true, 530, Dish.Type.OTHER),
                new Dish("rice", true, 350, Dish.Type.OTHER),
                new Dish("season fruit", true, 120, Dish.Type.OTHER),
                new Dish("pizza", true, 550, Dish.Type.OTHER),
                new Dish("prawns", false, 300, Dish.Type.FISH),
                new Dish("salmon", false, 450, Dish.Type.FISH) );

        List<String> threeHighCaloricDishNames =
                menu.stream()
                        .filter(d -> d.getCalories() > 300)
                        .map(Dish::getName)
                        .limit(3)
                        .collect(toList());
        System.out.println(threeHighCaloricDishNames);
    }
}

class Dish {
    private final String name;
    private final boolean vegetarian;
    private final int calories;
    private final Type type;
    public Dish(String name, boolean vegetarian, int calories, Type type) {
        this.name = name;
        this.vegetarian = vegetarian;
        this.calories = calories;
        this.type = type;
    }
    public String getName() {
        return name;
    }
    public boolean isVegetarian() {
        return vegetarian;
    }
    public int getCalories() {
        return calories;
    }
    public Type getType() {
        return type;
    }
    @Override
    public String toString() {
        return name;
    }
    public enum Type { MEAT, FISH, OTHER }
}
```

数据**源**是菜肴列表，它给流提供一个**元素序列**。接下来，对流应用一系列**数据处理操作**：filter、map、limit和collect。

#### 2.2、流的好处

* 声明性——更简洁，更易读
* 可复合——更灵活
* 可并行——性能更好

#### 2.3、流与集合的区别

粗略地说，集合与流之间的差异就在于什么时候进行计算。

* 集合是一个**内存中的数据结构**，**它包含数据结构中目前所有的值**——集合中的每个元素都得先算出来才能添加到集合中。可以往集合里加东西或者删东西，但是不管什么时候，集合中的每个元素都是放在内存里的，元素都得先算出来才能成为集合的一部分。

* 流则是在概念上固定的数据结构（不能添加或删除元素），其元素则是按需计算的。

举个例子：

我的电脑E盘有一个音乐的文件夹，里面下载了很多歌曲，那么这个文件夹就像是java里的集合，歌曲就像集合中的元素，我想要听哪首歌曲，戴上耳机就可以享受音乐了，但前提是这里面的歌曲都下载好了，就像前面说的元素得先计算出来。

我一直很喜欢赵雷，老早就听说他出了新专辑--《无法长大》，可是我的文件夹下面又没有这些歌曲，怎么办呢？第一个方法当然是下载下来，存放到文件夹下面，就像往集合中增加元素；另一种呢，我可以选择在线收听，我不必下载，网易云会自动帮我缓存好我将要播放的部分，这时同样可以欣赏赵雷忧郁的歌声。

不必下载，缓存将要听的部分就好比流，是按需计算的。

#### 2.4、流的特点

* 只能遍历一次：和迭代器类似，流只能遍历一次。遍历完之后，这个流已经被消费掉了，再次遍历，就会抛出异常。
* 内部迭代：使用Collection接口需要用户去做迭代（比如用for-each），这称为外部迭代。 相反，
  Streams库使用内部迭代——它帮把迭代做了，还把得到的流值存在了某个地方，只要给出一个函数说要干什么就可以了。
  ![](/assets/2.png)

使用for-each需要明确指定先做什么，再做什么，比如去菜地里摘菜，先摘韭菜，再摘豆角，然后摘西红柿，但内部迭代不一样，只要声明去摘菜，那么可以先摘离得远一点的西红柿，再摘中间的豆角，最后摘韭菜，也可以先摘一些韭菜，再去摘西红柿，然后又回过来摘韭菜。

内部迭代时，项目可以透明地并行处理，或者用更优化的顺序进行处理。Streams库的内部迭代可以自动选择一种适合硬件的数据表示和并行实现。

#### 2.5、流的操作

java.util.stream.Stream中的Stream接口定义了许多操作。它们可以分为两大类。

```
menu.stream()
    .filter(d -> d.getCalories() > 300)
    .map(Dish::getName)
    .limit(3)
    .collect(toList());
```

* filter、map和limit可以连成一条流水线；

* collect触发流水线执行并关闭它。

可以连接起来的流操作称为**中间操作**，关闭流的操作称为**终端操作**。

![](/assets/3.png)

##### 2.5.1、中间操作

诸如filter或sorted等中间操作会返回另一个流。这让多个操作可以连接起来形成一个查询。重要的是，除非流水线上触发一个终端操作，否则中间操作不会执行任何处理——它们很懒。这是因为中间操作一般都可以合并起来，在终端操作时一次性全部处理。

##### 2.5.2、终端操作

终端操作会从流的流水线生成结果。

### 三、使用流

空谈许久，现在上路吧，来看看Stream API支持的方便的操作。

#### 3.1、筛选和切片

##### 3.1.1、用Predicate筛选

Streams接口支持filter方法，该操作会接受一个Predicate作为参数，并返回一个包括所有符合谓词的元素的流。

例如，筛选出所有素菜，创建一张素食菜单：

```
List<Dish> vegetarianMenu = menu.stream()
                                .filter(Dish::isVegetarian)
                                .collect(toList());
```

##### 3.1.2、distinct

流还支持一个叫作distinct的方法，它会返回一个元素各异（根据流所生成元素的hashCode和equals方法实现）的流。发现没，同SQL中的distinct一样。

##### 3.1.3、截断流

流支持limit\(n\)方法，该方法会返回一个不超过给定长度的流。所需的长度作为参数传递给limit。如果流是有序的，则最多会返回前n个元素。

##### 3.1.4、跳过流

流还支持skip\(n\)方法，返回一个扔掉了前n个元素的流。如果流中元素不足n个，则返回一个空流。limit\(n\)和skip\(n\)是互补的！



