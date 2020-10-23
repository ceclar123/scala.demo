---
title: Scala基础
date: 2019-10-20 14:29:00
tags: [语法,基础]
category: Scala
---

Scala是一种纯面向对象的语言，每个值都是对象。基于JVM，兼容现有的Java程序。对于有Java基础的很容易掌握，同时有一些细节也需要特别关注。

## 注意事项

- 大小写敏感，与Java一样

- 类名，首字母大写，单词首字母大写，与Java一样

  ```scala
  class HelloWorld {
  	//
  }
  ```

- 方法名称，首字母小写，单词首字母大写，与Java一样

  ```scala
  class HelloWorld {
    def sayHello(txt: String): Unit = {
      println(s"hello ${txt}")
    }
  }
  ```

- 文件名称，程序文件的名称应该与对象名称完全匹配(新版本不需要了)

## 标识符

> 用于对象，类，变量和方法的名称称为标识符。关键字不能用作标识符，标识符区分大小写。

- 字母数字标识符

  - 以字母或下划线开始，之后可以跟字母、数字或下划线
  - `$`字符也被当作是字母，但被保留作为scala编译器产生的标识符之用。用户程序里的标识符不应该包含`$`字符，尽管能够编译通过，最好不要这样用
  - Scala遵循java的驼峰式标识符习惯，尽管下划线在标识符内是合法的，但最好不要这样用，因为在Scala中有其他用法。

- 运算符标识符

  - 由一个或多个符号组成，例如：

    ```scala
    + ++ ::: < ?> :->
    ```

  - Scala编译器将在内部将操作符标识符转换成具有嵌入式`$`字符的合法Java标识符。例如，标识符`:->`将被内部表示为`$colon$minus$greater`

- 混合标识符

  - 由字母数字组成，后面跟着下划线和一个操作符标识符

    ```scala
    val user_+ = "张三"
    val user_# = "李四"
    println(user_+, user_#)
    ```

- 字面识别符

  - 文字标识符是用反引号`括起来的任意字符串，即使scala保留字，这个规则也有效

    ```scala
    val `province` = "四川"
    val `object` = "成都"
    println(`province`, `object`)
    // 在 Scala 中 Thread. yield()非法的，因为 yield 是保留字。但是可以这样调用：Thread.`yield`()
    ```

## 注释

- 与Java类似，支持单行与多行注释，多行注释注意嵌套

  ```scala
  /*
  * 这里是多行注释
  * /*多行注意嵌套*/
  */
  val hello = "hello"
  // 单行注释1
  // 单行注释2
  var world = "world"
  
  /**
  * 文档注释
  *
  * @param args 参数
  */
  def main(args: Array[String]): Unit = {
      //
  }
  ```

## 换行符

- Scala是面向行的语言，语句可以用分号（;）结束或换行符

- 语句末尾的分号通常是可选的。如果一行里仅 有一个语句可不写。如果一行里写多个语句那么分号是必需的

  ```scala
  val a = "张三"
  val b = "张三"; val c = "李四"
  ```

## 关键字

|           |          |          |         |
| --------- | -------- | -------- | ------- |
| abstract  | case     | catch    | class   |
| def       | do       | else     | extends |
| false     | final    | finally  | for     |
| forSome   | if       | implicit | import  |
| lazy      | match    | new      | null    |
| object    | override | package  | private |
| protected | return   | sealed   | super   |
| this      | throw    | trait    | try     |
| true      | type     | val      | var     |
| while     | with     | yield    |         |
| -         | :        | =        | =>      |
| <-        | <:       | <%       | >:      |
| #         | @        |          |         |



