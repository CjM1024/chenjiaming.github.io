---
layout: post
title: 'java8 Tutorial'
date: 2021-01-20
author: CJM1024
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: java
---
## java8 新特性

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



#### 函数式接口

定义：只包含一个抽象方法，但是可以有多个非抽象方法（默认[^default]方法）的接口

注解：*@FunctionalInterface*

举例：java.lang.Runnable` 与 `java.util.concurrent.Callable

提示：虽然这个注解不是必须的[^只要接口只包含一个抽象方法，虚拟机会自动判断该接口为函数式接口]，但还是建议在接口上使用该注解进行声明，这样的话，编译器如果发现你标注了这个注解的接口有多于一个抽象方法的时候会报错的。

示例：

```java
@FunctionalInterface
public interface Converter<F, T> {
  T convert(F from);
}
```

```java
    // TODO 将数字字符串转换为整数类型
    Converter<String, Integer> converter = (from) -> Integer.valueOf(from);
    Integer converted = converter.convert("123");
    System.out.println(converted.getClass()); //class java.lang.Integer
```



#### 方法和构造函数引用

Java 8允许您通过`::`关键字传递方法或构造函数的引用

引用对象方法举例：

```java
class Something {
    String startsWith(String s) {
        return String.valueOf(s.charAt(0));
    }
}
```

```java
Something something = new Something();
Converter<String, String> converter = something::startsWith;
String converted = converter.convert("Java");
System.out.println(converted);    // "J"
```

传递构造方法举例：

首先定义一个包含多个构造函数的简单类：

```java
class Person {
    String firstName;
    String lastName;

    Person() {}

    Person(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
}
```

指定一个创建Person对象的对象工厂接口：

```java
interface PersonFactory<P extends Person> {
    P create(String firstName, String lastName);
}
```

使用构造函数引用来将他们关联起来，而不是手动实现一个完整的工厂：

```java
PersonFactory<Person> personFactory = Person::new;
Person person = personFactory.create("Peter", "Parker");
```

我们只需要使用 Person::new 来获取Person类构造函数的引用，Java编译器会自动根据PersonFactory.create方法的参数类型来选择合适的构造函数。



#### Lamda表达式作用域

##### 
