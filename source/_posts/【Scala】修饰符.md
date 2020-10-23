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

> 带有此标记的成员仅在包含了成员定义的类或对象内部可见

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

