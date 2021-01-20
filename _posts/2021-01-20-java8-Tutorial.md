## java8 Tutorial

---

#### 接口的默认方法

关键字：default

功能：可以让接口添加非抽象方法的实现

举例：

``` java
interface IMath{

    double add(int a);

    default double sqrt(int a) {
        return Math.sqrt(a);
    }
}
```

实现IMath的类只需要实现抽象方法 add就可以，默认方法sqrt可以直接使用



#### Lambda表达式

java8之前：

```java
List<String> names = Arrays.asList("peter", "anna", "mike", "xenia");

Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return b.compareTo(a);
    }
});
```

只需要给静态方法` Collections.sort` 传入一个 List 对象以及一个比较器来按指定顺序排列。通常做法都是创建一个匿名的比较器对象然后将其传递给 `sort` 方法。

java8:

```java
Collections.sort(names, (String a, String b) -> {
    return b.compareTo(a);
});
```

更简短写法：

```java
Collections.sort(names, (String a, String b) -> b.compareTo(a));
```

再简短(List类本身就有一个sort方法，并且java编译器可以自动推导出参数类型，所以可以不需要写参数类型)：

```java
names.sort((a, b) -> b.compareTo(a));
```

