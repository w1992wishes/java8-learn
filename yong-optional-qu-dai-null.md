### 一、引言

先假设有三个类，Student, Bag, Book：

```
class Student{
    private Bag bag;

    public Student(Bag bag) {
        this.bag = bag;
    }

    public Bag getBag() {
        return bag;
    }

    public void setBag(Bag bag) {
        this.bag = bag;
    }
}

class Bag{
    private Book book;

    public Bag(Book book) {
        this.book = book;
    }

    public Book getBook() {
        return book;
    }

    public void setBook(Book book) {
        this.book = book;
    }
}

class Book{
    private String name;

    public Book(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

现想要查找某个学生的书包中的书的名字，在java 8前是这样的：

```
public String getBookName(Student student){
        if (student != null){
            Bag bag = student.getBag();
            if (bag != null){
                Book book = bag.getBook();
                if (book != null){
                    return  book.getName();
                }
            }
        }
        return "Unknown";
    }
```

可以看到一堆判断，代码可读性很差，而且扩展性也不好。

再看看java 8中的改造和实现：

```
class Student{
    private Optional<Bag> bag;

    public Student(Optional<Bag> bag) {
        this.bag = bag;
    }

    public Optional<Bag> getBag() {
        return bag;
    }

    public void setBag(Optional<Bag> bag) {
        this.bag = bag;
    }
}

class Bag{
    private Optional<Book> book;

    public Bag(Optional<Book> book) {
        this.book = book;
    }

    public Optional<Book> getBook() {
        return book;
    }

    public void setBook(Optional<Book> book) {
        this.book = book;
    }
}

class Book{
    private String name;

    public Book(String name) {
        this.name = name;
    }

    public String getName() {
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

接下来这样就可以了：

```
public String getBookName(Optional<Student> student){
        return student
                .flatMap(Student::getBag)
                .flatMap(Bag::getBook)
                .map(Book::getName)
                .orElse("Unknown");
    }
```

这里有两个明显的好处：

* 用Optional明确说明了可能为空，比java 8之前更具表述性；

* 另外代码的可读性也更强。

> PS：这里需要注意的是，student .flatMap\(Student::getBag\)....中用了flatMap代替map，是因为map只是把流中的元素映射为流中另一个元素，而这里Student.getBag\(\)返回的是一个Optional&lt;Bag&gt;，如果这里用map，就会报错，无法编译，getBook\(\)是Bag的方法，Optional显然没有。
>
> flatMap将流扁平化为一个流的特性在这里则正好合适，不理解flatMap的可以再看看上一章节。

---

### 二、简介

看了上面的例子，对Optional应该有了一个简单的了解。

Optional是java 8引入的用于代替null的一个工具类，目的是为了改善代码的可读性，同时减少烦人的NullPointException。

### 三、使用Optional

#### 3.1、创建Optional对象

##### 3.1.1、Optional.empty

创建一个空的Optional对象。

##### 3.1.2、Optional.of

创建一个非空的Optional对象，如下：

Optional&lt;Bag&gt; optBag = Optional.of\(bag\);

如果bag是一个null，这段代码会立即抛出一个NullPointerException，而不是等到访问car的属性值时才返回一个错误。

##### 3.1.3、Optional.ofNullable

创建一个允许为空的Optional对象，如下：

Optional&lt;Bag&gt; optBag = Optional.of\(bag\);

如果bag是null，那么得到的Optional对象就是个空对象。这时如果试图用optBag .get\(\)方法会报错，可见Optional的使用是有技巧的。



