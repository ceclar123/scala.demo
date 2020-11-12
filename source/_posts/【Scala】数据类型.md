---
title: Scala数据类型
date: 2019-10-23 20:29:00
tags: [数据类型]
category: Scala
---

Scala完全面向对象语法，像Java中的基本数据类型（例如：int）是没有。同时Java的包装类型（例如：Integer）在Scala中都有对应的类型，并且可以隐式转换。

## 数据类型

| 数据类型 | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| Byte     | 8位有符号补码整数。数值区间为 -128 到 127                    |
| Short    | 16位有符号补码整数。数值区间为 -32768 到 32767               |
| Int      | 32位有符号补码整数。数值区间为 -2147483648 到 2147483647     |
| Long     | 64位有符号补码整数。数值区间为 -9223372036854775808 到 9223372036854775807 |
| Float    | 32 位, IEEE 754 标准的单精度浮点数                           |
| Double   | 64 位 IEEE 754 标准的双精度浮点数                            |
| Char     | 16位无符号Unicode字符, 区间值为 U+0000 到 U+FFFF             |
| String   | 字符序列                                                     |
| Boolean  | true或false                                                  |
| Unit     | 表示无值，和其他语言中void等同。用作不返回任何结果的方法的结果类型。Unit只有一个实例值，写成()。 |
| Null     | Null类是null引用对象的类型，它是每个引用类（继承自AnyRef的类）的子类。Null不兼容值类型 |
| Nothing  | Nothing类型在Scala的类层级的最底端；它是任何其他类型的子类型。 |
| Any      | Any是所有其他类的超类                                        |
| AnyRef   | AnyRef类是Scala里所有引用类(reference class)的基类           |
| AnyVal   | AnyVal是Scala里所有值类型的基类                              |

## 类关系

- Any
  - AnyVal
    - Byte
    - Short
    - Int
    - Long
    - Float
    - Double
    - Unit
    - Char
    - Boolean
- Scala.Nothing
  - AnyRef
  - Scala.Null
    - Scala.Nothing
  
  ![](http://qjo63pkr5.hd-bkt.clouddn.com/Scala%E7%B1%BB%E7%BB%A7%E6%89%BF%E5%85%B3%E7%B3%BB.png)

## 字面量

- 整形字面量

  > 整型字面量用于 Int 类型，如果表示 Long，可以在数字后面添加 L 或者小写 l 作为后缀。Byte、Short需要显示声明并且不要越界。
  >
  > Scala不支持八进制字面量和以0开头的整数字面量

  ```scala
  val age1 = 1
  val age2 = 1L
  val age3 = 0x1254512
  val age4 = 0x1254512L
  val age5: Short = 100
  val age6: Byte = 12
  println(age1.getClass, age1)
  println(age2.getClass, age2)
  println(age3.getClass, age3)
  println(age4.getClass, age4)
  println(age5.getClass, age5)
  println(age6.getClass, age6)
  
  printf("Byte[%d,%d]\n", Byte.MinValue, Byte.MaxValue)
  printf("Short[%d,%d]\n", Short.MinValue, Short.MaxValue)
  printf("Int[%d,%d]\n", Int.MinValue, Int.MaxValue)
  printf("Long[%d,%d]\n", Long.MinValue, Long.MaxValue)
  ```

- 浮点型字面量

  > 浮点型默认是Double类型，Float类型需要在数字后面加后缀F

  ```scala
  val price1 = 12.34
  val price2 = 12.34F
  
  println(price1.getClass, price1)
  println(price2.getClass, price2)
  
  printf("Float[%f,%f]\n", Float.MinValue, Float.MaxValue)
  printf("Double[%f,%f]\n", Double.MinValue, Double.MaxValue)
  ```

- 布尔型字面量

  ```scala
  val b1 = true
  val b2 = false
  
  println(b1.getClass, b1)
  println(b2.getClass, b2)
  ```

- 符号字面量

  > 符号字面量被写成： **'<标识符>** ，这里 **<标识符>** 可以是任何字母或数字的标识（注意：不能以数字开头）。这种字面量被映射成预定义类scala.Symbol的实例

  - 属于基本类型，被映射成scala.Symbol
  - 当两个Symbol值相等时，指向同一个实例
  - 作为不可变的字符串，同时不必重复地为相同对象创建实例，节省资源

  ```scala
  val s1 = 'hello
  val s2 = scala.Symbol("hello")
  
  println(s1 == s2)
  println(s1.getClass, s1)
  println(s2.getClass, s2)
  ```

- 字符字面量

  > 用单引号**'**来操作

  ```scala
  val ch1 = 'a'
  val ch2 = '\u0041'
  val ch3 = '\n'
  val ch4 = '\t'
  val ch5 = '\f'
  val ch6: Char = 97
  
  println(ch1.getClass, ch1)
  println(ch2.getClass, ch2)
  println(ch3.getClass, ch3)
  println(ch4.getClass, ch4)
  println(ch5.getClass, ch5)
  println(ch6.getClass, ch6)
  ```

- 字符串字面量

  > 用双引号**"**表示，多行字符串用三引号**"""**表示

  ```scala
  val str1 = "hello wolrd"
  val str2 = """hello 多行文本,
          第二行文本，
          第三行文本"""
  
  println(str1.getClass, str1)
  println(str2.getClass, str2)
  ```

- 转义字符

  | 转义字符 | Unicode | 描述                                |
  | :------- | :------ | :---------------------------------- |
  | \b       | \u0008  | 退格(BS) ，将当前位置移到前一列     |
  | \t       | \u0009  | 水平制表(HT) （跳到下一个TAB位置）  |
  | \n       | \u000a  | 换行(LF) ，将当前位置移到下一行开头 |
  | \f       | \u000c  | 换页(FF)，将当前位置移到下页开头    |
  | \r       | \u000d  | 回车(CR) ，将当前位置移到本行开头   |
  | \"       | \u0022  | 代表一个双引号(")字符               |
  | \'       | \u0027  | 代表一个单引号（'）字符             |
  | \\       | \u005c  | 代表一个反斜线字符 '\'              |

