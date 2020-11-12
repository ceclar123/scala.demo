---
title: Scala修饰符
date: 2019-10-24 10:29:00
tags: [修饰符]
category: Scala
---

Scala 访问修饰符基本和Java的一样，分别有：private，protected，public。但是一些细节不太一样。默认缺省情况下，Scala对象的访问级别是public。

- 不写（default）默认：包内可访问
- private私有：类内可访问
-  protected保护：包内和子类可访问
- public公有：都可访问

## 私有private

> 带有此标记的成员仅在包含了成员定义的类或对象内部可见。他们不能被子类继承，也不能覆盖父类中的定义

- Scala中

```scala
class ScalaOuter {
    class ScalaInner {
        private def hello() {
            println("hello")
        }

        class ScclaInnerInner {
            hello()
        }
    }

    private def world(): Unit = {
        (new ScalaInner).hello() //错误，方法不可见
    }
}    
```

- Java中

```java
class JavaOuter {
    class JavaInner {
        private void hello() {
            System.out.println("hello");
        }

        class JavaInnerInner {
            private void world() {
                hello();
            }
        }
    }

    private void world() {
        // Java中能够访问
        new JavaInner().hello();
    }
}
```

## 保护protected

> 只允许保护成员在定义了该成员的的类的子类中被访问。而在`Java`中，用protected关键字修饰的成员，除了定义了该成员的类的子类可以访问，同一个包里的其他类也可以进行访问

- Scala中

```scala
package a {

  class ScalaOuter {
    protected val helloWorld = "Hello World"

    protected def hello() {
      println("hello")
    }

    class ScalaInner {
      hello()
    }
  }
}

package b {
  class ScalaOuterChild extends a.ScalaOuter {
    hello()

    private def world(): Unit = {
      hello()
      println(helloWorld)
    }
  }
}
```

- Java中

  ```java
  package a;
  public class JavaOuter {
      protected String helloWorld = "Hello World";
  
      protected void hello() {
          System.out.println("hello");
      }
  }
  /* 子类中能和谐访问 */
  package b;
  
  import a.JavaOuter;
  
  public class JavaOuterChild extends JavaOuter {
      private void world() {
          this.hello();
          System.out.println(helloWorld);
      }
  }
  
  /* 同一个包中，能和谐访问 */
  package a;
  
  public class JavaOuterSamePackage {
      private void world() {
          JavaOuter outer = new JavaOuter();
          // 正常访问
          outer.hello();
          // 正常访问
          System.out.println(outer.helloWorld);
      }
  }
  
  /* 不同包中，不能访问 */
  package b;
  
  import a.JavaOuter;
  
  public class JavaOuterDiffPackage {
      private void world() {
          JavaOuter outer = new JavaOuter();
          // 不能访问
          outer.hello();
          // 不能访问
          System.out.println(outer.helloWorld);
      }
  }
  ```

## 公共public

> 如果没有指定任何的修饰符，则默认为 public。这样的成员在任何地方都可以被访问



## 作用域保护

> 1、Scala访问修饰符可以通过使用限定词强调，private[x]：这个成员除了对[...]中的类或[...]中的包中的类及他们的伴生对象可见外，对其他的类都是private。
>
> 2、有了这个，可以针对一些private对象跨包访问
>
> 3、private[x]其中x可以是包也可以是类
>
> 4、private[this]比较特殊，只能从该成员定义的对象内访问

```scala
private[x] 
或 
protected[x]
```

### 修饰类与伴生对象

```scala
package demo.a {

  import demo.a.b.{PrivateUser, PrivateUserA, PrivateUserA_B}
  package b {

    /**
     * package demo.a中可见
     */
    private[a] class PrivateUserA {

    }

    /**
     * package demo.a.b中可见
     */
    private[b] class PrivateUserA_B {

    }

    /**
     * 没有指定，默认所在包package demo.a.b中可见
     */
    private class PrivateUser {}

    class Hello {
      // 正常访问
      val userA: PrivateUserA = new PrivateUserA()
      // 正常访问
      val userB: PrivateUserA_B = new PrivateUserA_B()
      // 正常访问
      val user: PrivateUser = new PrivateUser()
    }

    object Hello {
      // 正常访问
      val userA: PrivateUserA = new PrivateUserA()
      // 正常访问
      val userB: PrivateUserA_B = new PrivateUserA_B()
      // 正常访问
      val user: PrivateUser = new PrivateUser()
    }

  }

  class Hello {
    // 正常访问
    val userA: PrivateUserA = new PrivateUserA()
    // 不能访问
    val userB: PrivateUserA_B = new PrivateUserA_B()
    // 不能访问
    val user: PrivateUser = new PrivateUser()
  }

  object Hello {
    // 正常访问
    val userA: PrivateUserA = new PrivateUserA()
    // 不能访问
    val userB: PrivateUserA_B = new PrivateUserA_B()
    // 不能访问
    val user: PrivateUser = new PrivateUser()
  }

  package c {

    import demo.a.b.{PrivateUser, PrivateUserA, PrivateUserA_B}

    class Hello {
      // 正常访问
      val userA: PrivateUserA = new PrivateUserA()
      // 不能访问
      val userB: PrivateUserA_B = new PrivateUserA_B()
      // 不能访问
      val user: PrivateUser = new PrivateUser()
    }

    object Hello {
      // 正常访问
      val userA: PrivateUserA = new PrivateUserA()
      // 不能访问
      val userB: PrivateUserB = new PrivateUserB()
      // 不能访问
      val user: PrivateUser = new PrivateUser()
    }

  }

}
```

### 修饰方法

```scala
package demo.a {

  import demo.a.b.{PrivateUser, PrivateUserA, PrivateUserA_B}
  package b {

    /**
     * package demo.a中可见
     */
    private[a] class PrivateUserA {
      private[a] def sayHelloA(text: String): Unit = {
        println(text)
      }

      private[b] def sayHelloA_B(text: String): Unit = {
        println(text)
      }
    }

    /**
     * package demo.a.b中可见
     */
    private[b] class PrivateUserA_B {
      private[a] def sayHelloA(text: String): Unit = {
        println(text)
      }

      private[b] def sayHelloA_B(text: String): Unit = {
        println(text)
      }
    }

    /**
     * 没有指定，默认所在包package demo.a.b中可见
     */
    private class PrivateUser {
      private[a] def sayHelloA(text: String): Unit = {
        println(text)
      }

      private[b] def sayHelloA_B(text: String): Unit = {
        println(text)
      }
    }

    class Hello {
      // 正常访问
      val userA: PrivateUserA = new PrivateUserA()
      userA.sayHelloA("helloA")
      userA.sayHelloA_B("helloB")

      // 正常访问
      val userB: PrivateUserA_B = new PrivateUserA_B()
      userB.sayHelloA("helloA")
      userB.sayHelloA_B("helloB")

      // 正常访问
      val user: PrivateUser = new PrivateUser()
      user.sayHelloA("helloA")
      user.sayHelloA_B("helloB")
    }

    object Hello {
      // 正常访问
      val userA: PrivateUserA = new PrivateUserA()
      userA.sayHelloA("helloA")
      userA.sayHelloA_B("helloB")

      // 正常访问
      val userB: PrivateUserA_B = new PrivateUserA_B()
      userB.sayHelloA("helloA")
      userB.sayHelloA_B("helloB")

      // 正常访问
      val user: PrivateUser = new PrivateUser()
      user.sayHelloA("helloA")
      user.sayHelloA_B("helloB")
    }

  }

  class Hello {
    // 正常访问
    val userA: PrivateUserA = new PrivateUserA()
    userA.sayHelloA("helloA")
    //不能访问
    userA.sayHelloA_B("helloB")

    // 不能访问
    val userB: PrivateUserA_B = new PrivateUserA_B()
    userB.sayHelloA("helloA")
    userB.sayHelloA_B("helloB")

    // 不能访问
    val user: PrivateUser = new PrivateUser()
    user.sayHelloA("helloA")
    user.sayHelloA_B("helloB")
  }

  object Hello {
    // 正常访问
    val userA: PrivateUserA = new PrivateUserA()
    userA.sayHelloA("helloA")
    //不能访问
    userA.sayHelloA_B("helloB")

    // 不能访问
    val userB: PrivateUserA_B = new PrivateUserA_B()
    userB.sayHelloA("helloA")
    userB.sayHelloA_B("helloB")

    // 不能访问
    val user: PrivateUser = new PrivateUser()
    user.sayHelloA("helloA")
    user.sayHelloA_B("helloB")
  }

  package c {

    import demo.a.b.{PrivateUser, PrivateUserA, PrivateUserA_B}

    class Hello {
      // 正常访问
      val userA: PrivateUserA = new PrivateUserA()
      userA.sayHelloA("helloA")
      //不能访问
      userA.sayHelloA_B("helloB")

      // 不能访问
      val userB: PrivateUserA_B = new PrivateUserA_B()
      userB.sayHelloA("helloA")
      userB.sayHelloA_B("helloB")

      // 不能访问
      val user: PrivateUser = new PrivateUser()
      user.sayHelloA("helloA")
      user.sayHelloA_B("helloB")
    }

    object Hello {
      // 正常访问
      val userA: PrivateUserA = new PrivateUserA()
      userA.sayHelloA("helloA")
      //不能访问
      userA.sayHelloA_B("helloB")

      // 不能访问
      val userB: PrivateUserA_B = new PrivateUserA_B()
      userB.sayHelloA("helloA")
      userB.sayHelloA_B("helloB")

      // 不能访问
      val user: PrivateUser = new PrivateUser()
      user.sayHelloA("helloA")
      user.sayHelloA_B("helloB")
    }

  }

}
```

