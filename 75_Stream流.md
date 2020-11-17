---
title: Stream流
date: 2020-11-07 21:04:38
tags:
	- Java
	- Lambda
	- 基础知识
categories:
	- Java
	- 复习
cover:
	https://gitee.com/jasonM4A1/pictureHost/raw/master/20201107210640.jpg
---

# Stream流

说到Stream便容易想到I/O Stream，而实际上，谁规定“流”就一定是“IO流”呢？在Java 8中，得益于Lambda所带 来的函数式编程，引入了一个全新的Stream概念，用于解决已有集合类库既有的弊端。

## 引言

### 传统集合的多步遍历代码

几乎所有的集合（如 Collection 接口或 Map 接口等）都支持直接或间接的遍历操作。而当我们需要对集合中的元 素进行操作的时候，除了必需的添加、删除、获取外，最典型的就是集合遍历。例如：

~~~java
import java.util.ArrayList;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("Jason");
        list.add("Charlie");
        list.add("June");
        for (String s: list) {
            System.out.print(s + "; ");//Jason; Charlie; June; 
        }
    }
}
~~~

这是一段非常简单的集合遍历操作：对集合中的每一个字符串都进行打印输出操作。

### 循环遍历的弊端

Java 8的Lambda让我们可以更加专注于做什么（What），而不是怎么做（How），这点此前已经结合内部类进行了对比说明。现在我们仔细体会一下上例代码，可以发现：

+ for循环的语法就是“怎么做”
+ for循环的循环体才是“做什么”

为什么使用循环？因为要进行遍历。但循环是遍历的唯一方式吗？遍历是指每一个元素逐一进行处理，而并不是从 第一个到最后一个顺次处理的循环。前者是目的，后者是方式。

试想一下，如果希望对集合中的元素进行筛选过滤：

1. 将集合A根据条件一过滤为子集B；
2. . 然后再根据条件二过滤为子集C。

那怎么办？在Java 8之前的做法可能为：

~~~java
import java.util.ArrayList;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("Jason");
        list.add("Charlie");
        list.add("June");

        List<String> list1 = new ArrayList<>();

        // 将集合A根据条件一过滤为子集B；
        for (String name: list) {
            if (name.startsWith("J")) {
                list1.add(name);
            }
        }

        List<String> list2 = new ArrayList<>();

        // 然后再根据条件二过滤为子集C。
        for (String name: list1) {
            if(name.length() == 5) {
                list2.add(name);
            }
        }

        //遍历最后得到的集合C
        for (String name: list2) {
            System.out.println(name);//Jason
        }
    }
}
~~~

这段代码中含有三个循环，每一个作用不同：

1. 首先筛选所有姓名中有J的人； 
2.  然后筛选名字长度为5的人；
3. 最后进行对结果进行打印输出。

每当我们需要对集合中的元素进行操作的时候，总是需要进行循环、循环、再循环。这是理所当然的么？不是。循环是做事情的方式，而不是目的。另一方面，使用线性循环就意味着只能遍历一次。如果希望再次遍历，只能再使用另一个循环从头开始。

那，Lambda的衍生物Stream流能给我们带来怎样更加优雅的写法呢？

### Stream的更优写法

下面来看一下借助Java 8的Stream API，什么才叫优雅：

~~~java
import java.util.ArrayList;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("Jason");
        list.add("Charlie");
        list.add("June");

        list.stream()
                .filter( s -> s.startsWith("J"))
                .filter(s -> s.length() == 5)
                .forEach(System.out::println);//Jason
    }
}
~~~

直接阅读代码的字面意思即可完美展示无关逻辑方式的语义：获取流、过滤姓张、过滤长度为3、逐一打印。代码 中并没有体现使用线性循环或是其他任何算法进行遍历，我们真正要做的事情内容被更好地体现在代码中。

## 流式思想概述

> 注意：请暂时忘记对传统IO流的固有印象！

整体来看，流式思想类似于工厂车间的“生产流水线”。

当需要对多个元素进行操作（特别是多步操作）的时候，考虑到性能及便利性，我们应该首先拼好一个“模型”步骤 方案，然后再按照方案去执行它。

这张图（图丢了）中展示了过滤、映射、跳过、计数等多步操作，这是一种集合元素的处理方案，而方案就是一种“函数模 型”。图中的每一个方框都是一个“流”，调用指定的方法，可以从一个流模型转换为另一个流模型。而最右侧的数字 3是最终结果。 这里的 filter 、 map 、 skip 都是在对函数模型进行操作，集合元素并没有真正被处理。只有当终结方法 count 执行的时候，整个模型才会按照指定策略执行操作。而这得益于Lambda的延迟执行特性。

> 备注：“Stream流”其实是一个集合元素的函数模型，它并不是集合，也不是数据结构，其本身并不存储任何 元素（或其地址值）。

Stream（流）是一个来自数据源的元素队列.

1. 元素是特定类型的对象，形成一个队列。 Java中的Stream并不会存储元素，而是按需计算。
2. 数据源 流的来源。 可以是集合，数组 等。

和以前的Collection操作不同， Stream操作还有两个基础的特征：

1. **Pipelining**: 中间操作都会返回流对象本身。 这样多个操作可以串联成一个管道， 如同流式风格（fluent style）。 这样做可以对操作进行优化， 比如延迟执行(laziness)和短路( short-circuiting)。
2. **内部迭代**： 以前对集合遍历都是通过Iterator或者增强for的方式, 显式的在集合外部进行迭代， 这叫做外部迭代。 Stream提供了内部迭代的方式，流可以直接调用遍历方法。

当使用一个流的时候，通常包含三个基本步骤：获取一个数据源（source）——> 数据转换——> 执行操作获取想要的结果。每次转换原有 Stream 对象不改变，返回一个新的 Stream 对象（可以有多次转换），这就允许对其操作可以 像链条一样排列，变成一个管道。

## 获取流

`java.util.stream.Stream<T>`是Java 8新加入的最常用的流接口（这并不是一个函数式接口）。

获取一个流非常简单，有以下几种常用的方式：

1. 所有的`Collection`集合都可以通过其默认方法`stream()`获取流；
2. `Stream`接口的静态方法`of()`可以获取数组对应的流。

### 根据Collection获取流

首先，`java.util.Collection`接口中加入了默认方法`stream`用来获取流，所以其所有实现类均可通过这个方法获取流。

~~~java
import java.util.*;
import java.util.stream.Stream;

public class Test {
    public static void main(String[] args) {
        //创建List集合
        List<String> list = new ArrayList<>();
        //向集合中添加元素
        Collections.addAll(list,"First","Second","Third");
        //获取流
        Stream<String> stream1 = list.stream();

        //创建set集合
        Set<String> set = new HashSet<>();
        //向集合中添加元素
        Collections.addAll(set,"First","Second","Third");
        //获取流
        Stream<String> stream2 = set.stream();
    }
}
~~~

### 根据Map获取流

`java.util.Map`接口不是`Collection`的子接口，且其K-V数据结构不符合流元素的单一特征，所以获取对应的流需要分key、value或entry等情况：

~~~java
import java.util.HashMap;
import java.util.Map;
import java.util.stream.Stream;

public class Test {
    public static void main(String[] args) {
        //创建Map集合
        Map<String,String> map = new HashMap<>();
        //向集合中添加元素
        map.put("First","Jason");
        map.put("Second","Charlie");
        map.put("Third","June");
        //获取keySet集合的流
        Stream<String> stream1 = map.keySet().stream();
        //获取valueSet集合的流
        Stream<String> stream2 = map.values().stream();
        //获取entrySet集合中的流
        Stream<Map.Entry<String, String>> stream3 = map.entrySet().stream();
    }
}
~~~

### 根据数组获取流

如果使用的不是集合或映射而是数组，由于数组对象不可能添加默认方法，所以`Stream`接口中提供了静态方法`of()`来获取流：

~~~java
import java.util.stream.Stream;

public class Test {
    public static void main(String[] args) {
        //创建一个字符串数组
        String[] array = { "Jason", "Charlie", "June"};
        //获取数组的流
        Stream<String> stream = Stream.of(array);
    }
}
~~~

> 备注： of 方法的参数其实是一个可变参数，所以支持数组。

## 常用方法

流模型的操作很丰富，这里介绍一些常用的API。这些方法可以被分成两种：

1. 延迟方法：返回值类型仍然是`Stream`接口自身类型的方法，因此支持链式调用。
2. 终结方法：返回值类型不再是`Stream`接口自身类型的方法，因此不再支持类似`StringBuilder`那样的链式调用。本小节中，终结方法包含`count()`和`forEach()`方法。

> 备注：本小节之外的更多方法，请自行参考API文档。

### 逐一处理：forEach

虽然方法名字为`forEach`，但是与for循环中的`for-each`不同。JDK源代码为：

~~~java
void forEach(Consumer<? super T> action);
~~~

该方法接收一个`Consumer`接口函数，会将每一个流元素交给该函数进行处理。

### 过滤：filter

可以通过`filter`方法将一个流转换成另一个子集流。方法签名：

~~~java
Stream<T> filter(Predicate<? super T> predicate);
~~~

该接口接收一个`Predicate`函数式接口参数（可以是一个Lambda或方法引用）作为筛选条件。

**基本使用**

`Stream`流中的`filter()`方法基本使用的代码如：

~~~java
import java.util.stream.Stream;

public class Test {
    public static void main(String[] args) {
        //传入多个参数，相当于获取字符串数组的流
        Stream<String> original = Stream.of("德玛西亚","德高望重","得意洋洋");
        //对刚获取的字符串数组的流进行过滤
        Stream<String> result = original.filter(s -> s.startsWith("德"));
    }
}
~~~

在这里通过Lambda表达式来指定了筛选的条件：必须含有“德”字。

### 映射：map

如果需要将流中的元素映射到另一个流中，可是使用`map`方法。方法签名：

~~~java
<R> Stream<R> map(Function<? super T, ? extends R> mapper);
~~~

该接口需要一个`Function`函数式接口参数，可以将当前流中T类型数据转换为另一种R类型的流。

**基本使用**

`Stream`流中的`map()`方法基本使用的代码如：

~~~java
import java.util.stream.Stream;

public class Test {
    public static void main(String[] args) {
        //创建一个字符串数组
        String[] strArr = { "10", "20", "30"};
        //获取字符串数组的流
        Stream<String> original = Stream.of(strArr);
        //使用map()将String类型的数据映射为Integer类型的数据
        Stream<Integer> result = original.map(s -> Integer.parseInt(s));
    }
}
~~~

这段代码中， map 方法的参数通过方法引用，将字符串类型转换成为了int类型（并自动装箱为 Integer 类对 象）。

### 统计个数：count

正如旧集合`Collection`当中的`size()`方法一样，Stream流提供了`count()`方法来计算流元素的个数：

~~~java
long count();
~~~

该方法返回一个`long`值代表元素个数（集合的size返回是int值）。

**基本使用**

~~~java
import java.util.stream.Stream;

public class Test {
    public static void main(String[] args) {
        //获取字符串数组的流
        Stream<String> original = Stream.of("First","Second","Threes");
        //计算流中元素的个数
        long num = original.count();
        System.out.println(num);//3
    }
}
~~~

### 取用前几个：limit

`limit()`方法可以对流进行截取，只取用前n个。方法签名：

~~~java
Stream<T> limit(long maxSize);
~~~

参数是一个long型，如果集合当前长度大于参数则进行截取；否则不进行操作。基本使用：

~~~java
import java.util.stream.Stream;

public class Test {
    public static void main(String[] args) {
        //获取字符串数组的流
        Stream<String> original = Stream.of("Jason","Charlie","count","fuck","refactor");
        //对流进行截取
        Stream<String> result = original.limit(3);
        //获取流中元素的个数
        long num = result.count();
        System.out.println(num);//3
    }
}
~~~

### 跳过前几个：skip

如果希望跳过前几个元素，可以使用`skip`方法获取一个截取之后的新流：

~~~java
Stream<T> skip(long n);
~~~

如果流的当前长度大于n，则跳过前n个；否则将会得到一个长度为0的空流。基本使用：

~~~java
import java.util.stream.Stream;

public class Test {
    public static void main(String[] args) {
        //获取字符串数组的流
        Stream<String> original = Stream.of("Jason","Stream","English","America","Charlie");
        //使用skip()跳到前几个
        Stream<String> result = original.skip(3);
        //计算流中的元素个数
        long num = result.count();
        System.out.println(num);//2
    }
}
~~~

### 组合：concat

如果有两个流，希望合并成为一个流，那么可以使用`Stream`接口的静态方法`concat`：

~~~java
static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b)
~~~

> 这是一个静态方法，与`java.lang.String`当中的`concat()`方法不同。

该方法的基本使用代码如下：

~~~java
import java.util.stream.Stream;

public class Test {
    public static void main(String[] args) {
        //获取字符串数组流A
        Stream<String> streamA = Stream.of("Jason","Charlie","June");
        //获取字符串数组流B
        Stream<String> streamB = Stream.of("America","English","France");
        //使用静态方法concat()合并两个流
        Stream<String> streamC = Stream.concat(streamA, streamB);
        //计算合并后得到的流的元素个数
        long num = streamC.count();
        System.out.println(num);//6
    }
}
~~~

### 练习一：集合元素处理（传统方法）

**题目**

现在有两个`ArrayList`集合存储队伍当中的多个成员姓名，要求使用传统的for循环（或增强for循环）依次进行以下若干操作步骤：

1. 第一个队伍只要名字为3个字的成员姓名；存储到一个新集合中。
2. 第一个队伍筛选之后只要前3个人；存储到一个新集合中。
3. 第二个队伍只要姓张的成员姓名；存储到一个新集合中。
4. 第二个队伍筛选之后不要前2个人；存储到一个新集合中。
5. 将两个队伍合并为一个新队伍；存储到一个新集合中。
6. 根据姓名创建`Person`对象；存储到一个新集合中。
7. 打印整个队伍的`Person`对象信息。

两支队伍：

~~~java
import java.util.ArrayList;
import java.util.Collections;

public class Test {
    public static void main(String[] args) {
        //第一支队伍
        ArrayList<String> firstTeam = new ArrayList<>();
        Collections.addAll(firstTeam,"迪丽热巴","宋远桥",
                "苏星河", "石破天","石中玉","老子","庄子","洪七公");

        //第二支队伍
        ArrayList<String> secondTeam = new ArrayList<>();
        Collections.addAll(secondTeam,"古力娜扎","张无忌",
                "赵丽颖","张三丰","尼古拉斯赵四","张天爱","张二狗");
    }
}
~~~

`Person`类：

~~~java
class Person {
    private String name;
    
    //constructor
    public Person() {}
    
    public Person(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                '}';
    }
    
    //setter and getter
    public String getName() {
       return name; 
    }
    
    public void setName() {
        this.name = name;
    }
}
~~~

**解答**

~~~java
import java.util.ArrayList;
import java.util.Collections;

class Person {
    private String name;

    //constructor
    public Person() {}

    public Person(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                '}';
    }

    //setter and getter
    public String getName() {
        return name;
    }

    public void setName() {
        this.name = name;
    }
}

public class Test {
    public static void main(String[] args) {
        //第一支队伍
        ArrayList<String> firstTeam = new ArrayList<>();
        Collections.addAll(firstTeam,"迪丽热巴","宋远桥",
                "苏星河", "石破天","石中玉","老子","庄子","洪七公");

        //第二支队伍
        ArrayList<String> secondTeam = new ArrayList<>();
        Collections.addAll(secondTeam,"古力娜扎","张无忌",
                "赵丽颖","张三丰","尼古拉斯赵四","张天爱","张二狗");

        //第一个队伍只要名字为3个字的
        ArrayList<String> firstTeam02 = new ArrayList<>();
        for (String s: firstTeam) {
            if (s.length() == 3) {
                firstTeam02.add(s);
            }
        }
        //第一支队伍只要筛选后的前三个人
        ArrayList<String > firstTeam03 = new ArrayList<>();
        for (int i = 0; i < 3; i++) {
            firstTeam03.add(firstTeam02.get(i));
        }

        //第二支队伍只要姓张的人
        ArrayList<String> secondTeam02 = new ArrayList<>();
        for (String name: secondTeam) {
            if (name.contains("张")) {
                secondTeam02.add(name);
            }
        }
        //第二支队伍不要筛选后的前两个人
        ArrayList<String> secondTeam03 = new ArrayList<>();
        for (int i = 2; i < secondTeam02.size(); i++) {
            secondTeam03.add(secondTeam02.get(i));
        }

        //合并筛选后的两支队伍
        ArrayList<String> team = new ArrayList<>();
        for (String name: firstTeam03) {
            team.add(name);
        }
        for (String name: secondTeam03) {
            team.add(name);
        }

        //根据姓名创建Person对象，并添加到新的集合中
        ArrayList<Person> perList = new ArrayList<>();
        for (String name: team) {
            perList.add(new Person(name));
        }

        //打印整个队伍的Person对象信息
        for (Person p: perList) {
            System.out.println(p);
        }
    }
}
//结果：
//Person{name='宋远桥'}
//Person{name='苏星河'}
//Person{name='石破天'}
//Person{name='张天爱'}
//Person{name='张二狗'}
~~~

### 练习二：集合元素处理（Stream方式）

**题目**

将上一题当中的传统for循环写法更换为Stream流式处理方式。两个集合的初始内容不变，`Person`类的定义也不变。

**解答**

等效的`Stream`流式处理代码为：

~~~java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.stream.Stream;

class Person {
    private String name;

    //constructor
    public Person() {}

    public Person(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                '}';
    }

    //setter and getter
    public String getName() {
        return name;
    }

    public void setName() {
        this.name = name;
    }
}

public class Test {
    public static void main(String[] args) {
        //第一支队伍
        ArrayList<String> one = new ArrayList<>();
        Collections.addAll(one,"迪丽热巴","宋远桥",
                "苏星河", "石破天","石中玉","老子","庄子","洪七公");

        //第二支队伍
        ArrayList<String> two = new ArrayList<>();
        Collections.addAll(two,"古力娜扎","张无忌",
                "赵丽颖","张三丰","尼古拉斯赵四","张天爱","张二狗");

        // 第一个队伍只要名字为3个字的成员姓名；
        // 第一个队伍筛选之后只要前3个人；
        Stream<String> streamOne = one.stream().filter(s -> s.length() == 3).limit(3);

        // 第二个队伍只要姓张的成员姓名；
        // 第二个队伍筛选之后不要前2个人；
        Stream<String> streamTwo = two.stream().filter(s -> s.startsWith("张")).skip(2);

        // 将两个队伍合并为一个队伍；
        // 根据姓名创建Person对象；
        // 打印整个队伍的Person对象信息。
        Stream.concat(streamOne,streamTwo).map(Person::new).forEach(System.out::println);
    }
}
//结果：
//Person{name='宋远桥'}
//Person{name='苏星河'}
//Person{name='石破天'}
//Person{name='张天爱'}
//Person{name='张二狗'}
~~~

