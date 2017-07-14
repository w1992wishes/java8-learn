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
        return name;
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
> flatMap将流扁平化为一个流的特性在这里

---

