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

### 二、什么是流

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

### 三、流的好处

* 声明性——更简洁，更易读
* 可复合——更灵活
* 可并行——性能更好



