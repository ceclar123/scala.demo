---
title: Scala变量
date: 2020-10-22 11:29:00
tags: [变量]
category: Scala
---

在 Scala 中，使用关键词 `var` 声明变量，使用关键词 `val` 声明常量

## 变量声明

> ```
> var VariableName : DataType [=  Initial Value]
> val VariableName : DataType [=  Initial Value]
> ```

```scala
var city: String = "北京"
val age: Int = 10
println(city, age)
```

## 类型推断

> 当分配一个初始值给一个变量，Scala编译器可以计算出根据分配给它的值的变量类型。

```scala
var city1 = "北京"
val city2 = "上海"
println(city1, city2)
```

## 多个变量声明

```scala
var city1, city2 = "北京"
println(city1, city2)

val user1, user2 = "张三"
println(user1, user2)
```

## 变量与常量

```scala
// var变量
var city = "北京"
city = "上海"
println(city)

// val常量
val user = "张三"
// 不能重新赋值
//user = "李四"
println(user)

// 有一种特殊场景，虽然不能重新对val变量userInfo赋值，但是可以修改里面的内容
class UserInfo(var userName: String, var age: Int) {
    override def toString: String = {
        s"${userName},${age}"
    }
}
val userInfo: UserInfo = new UserInfo("李四", 20)
// 不能重新赋值
//userInfo = new UserInfo("李四", 20)

//修改内容
userInfo.userName = "张三"
userInfo.age = 10
println(userInfo)
```

