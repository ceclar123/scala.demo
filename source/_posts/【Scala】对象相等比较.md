---
title: 对象相等比较
date: 2019-08-21 20:01:00
tags: [相等]
category: Scala
---

在 Scala 中，可以直接使用JDK的类库，好多功能语法相似，在Java中比较对象就用`==`与`equals`很简单。在Scala中可以使用`==`、`equals`、`eq`、`sameElements`

##  比较规则

- Java中比较引用或则基本数据类型用`==`，比较内容用`equals`方法
- Scala中直接使用`==`，内部首先判断左侧是否为`null`，如果不为`null`，直接调用`equals`方法，当然也可以调用`equals`方法
- Scala中如果要特别比较引用相等性可以用`eq`与`ne`方法，这个跟Java中的`==`一直

> https://www.scala-lang.org/api/current/scala/AnyRef.html

| final def==(arg0: Any): Boolean<br/>	The expression x == that is equivalent to if (x eq null) that eq null else x.equals(that).<br/>	如果x为null就that.eq(null)，如果x不为null就x.equals(that) |
| ------------------------------------------------------------ |
| final def eq(arg0: AnyRef): Boolean<br/>	Tests whether the argument (that) is a reference to the receiver object (this). |
| def equals(arg0: AnyRef): Boolean<br/>	The equality method for reference types. |

### Java举例

```java
class Person {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (!(o instanceof Person)) {
            return false;
        }
        Person person = (Person) o;
        return name.equals(person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }
}

public static void main(String[] args) {
    Person p1 = new Person("张三");
    Person p2 = new Person("张三");
    //引用比较
    System.out.printf("p1 == p2 -> %s\r\n", p1 == p2);
    System.out.printf("p1 != p2 -> %s\n", p1 != p2);
    //内容比较
    System.out.printf("p1.equals(p2) -> %s\n", p1.equals(p2));
}

/*
输出结果:
p1 == p2 -> false
p1 != p2 -> true
p1.equals(p2) -> true
*/
```

### Scala举例

```scala
class Person(val name: String) {
    private def canEqual(other: Any): Boolean = other.isInstanceOf[Person]

    override def equals(other: Any): Boolean = other match {
        case that: Person =>
        (that canEqual this) &&
        name == that.name
        case _ => false
    }

    override def hashCode(): Int = {
        val state = Seq(name)
        state.map(_.hashCode()).foldLeft(0)((a, b) => 31 * a + b)
    }
}

def main(args: Array[String]): Unit = {
      val p1 = new Person("张三")
      val p2 = new Person("张三")
      //内容比较
      printf("p1 == p2 -> %s\r\n", p1 == p2)
      printf("p1 != p2 -> %s\r\n", p1 != p2)
      printf("p1.equals(p2) -> %s\r\n", p1.equals(p2))
      //引用比较
      printf("p1.eq(p2) -> %s\r\n", p1.eq(p2))
      printf("p1.ne(p2) -> %s\r\n", p1.ne(p2))
 }  

/*
输出结果:
p1 == p2 -> true
p1 != p2 -> false
p1.equals(p2) -> true
p1.eq(p2) -> false
p1.ne(p2) -> true
*/
```

### Scala中使用Java类库注意事项

```scala
def main(args: Array[String]): Unit = {
    val i1 = new java.lang.Integer(Int.MaxValue)
    val l1 = new java.lang.Long(Int.MaxValue)

    # 这里比较特殊
    printf("i1 == l1 ->%s\r\n", i1 == l1)
    printf("i1.equals(l1) ->%s\r\n", i1.equals(l1))
    printf("i1.eq(l1) ->%s\r\n", i1.eq(l1))
}    
/*
输出结果:
i1 == l1 ->true
i1.equals(l1) ->false
i1.eq(l1) ->false
*/
```

### Scala集合中可能需要使用sameElements比较内容

```scala
def main(args: Array[String]): Unit = {
      //List推荐使用equals与eq
      val list1 = List("北京", "上海")
      val list2 = List("北京", "上海")
      printf("list1 == list2 -> %s\r\n", list1 == list2)
      printf("list1.equals(list2) -> %s\r\n", list1.equals(list2))
      printf("list1.sameElements(list2) -> %s\r\n", list1.sameElements(list2))
      printf("list1.eq(list2) -> %s\r\n", list1.eq(list2))

      //Array推荐使用sameElements与eq
      val array1 = Array("北京", "上海")
      val array2 = Array("北京", "上海")
      printf("array1 == array2 -> %s\r\n", array1 == array2)
      printf("array1.equals(array2) -> %s\r\n", array1.equals(array2))
      printf("array1.sameElements(array2) -> %s\r\n", array1.sameElements(array2))
      printf("array1.eq(array2) -> %s\r\n", array1.eq(array2))

      //Map推荐使用equals与eq
      val map1 = Map("北京" -> 1, "上海" -> 1)
      val map2 = Map("北京" -> 1, "上海" -> 1)
      printf("map1 == map2 -> %s\r\n", map1 == map2)
      printf("map1.equals(map2) -> %s\r\n", map1.equals(map2))
      printf("map1.sameElements(map2) -> %s\r\n", map1.sameElements(map2))
      printf("map1.eq(map2) -> %s\r\n", map1.eq(map2))

      //Set推荐使用equals与eq
      val set1 = Set("北京", "上海")
      val set2 = Set("北京", "上海")
      printf("set1 == set2 -> %s\r\n", set1 == set2)
      printf("set1.equals(set2) -> %s\r\n", set1.equals(set2))
      printf("set1.sameElements(set2) -> %s\r\n", set1.sameElements(set2))
      printf("set1.eq(set2) -> %s\r\n", set1.eq(set2))
}
/*
输出结果:
list1 == list2 -> true
list1.equals(list2) -> true
list1.sameElements(list2) -> true
list1.eq(list2) -> false

array1 == array2 -> false
array1.equals(array2) -> false
array1.sameElements(array2) -> true
array1.eq(array2) -> false

map1 == map2 -> true
map1.equals(map2) -> true
map1.sameElements(map2) -> true
map1.eq(map2) -> false

set1 == set2 -> true
set1.equals(set2) -> true
set1.sameElements(set2) -> true
set1.eq(set2) -> false
*/
```

