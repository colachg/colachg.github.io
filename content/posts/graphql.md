---
title: "Graphql"
date: 2020-01-07T14:53:31+08:00
draft: false
---

A Query Language for APIs.

## Queries

简而言之，GraphQL 适用于查询对象的属性。基本概念如下:

- **Fields**(属性)：每个对象都有一个或多个自己的属性，查询你所需的。
- **Arguments**(参数)：在查询时，可以通过参数指定对象。
- **Aliases**(别名): 当你在查询同一个属性，但参数不同时，它就排上用场了。
- **Fragments**(片段)：可以理解成可复用的单元，当你在查询不同对象，相同的属性时可以用上。
- **Operation name**(操作名)：语义化的操作名字，指示操作的目的。
- **Variables**(变量)：在程序运行时，Arguments(参数)是动态的,这里可以通过环境变量来传递参数。
- **Directives**(指令)：指令可以附着在属性和片段上，通过变量来控制是否需要查询相应的属性和片段。

## Mutations

修改服务端数据的方式。  
查询服务器端数据是并行的，这样速度会快；修改服务器端的数据则是串行的，这样有利于保证数据的一致性。

## Inline Fragments

当查询返回的结果类型不能确定时，可以用 Inline Fragments 来指定具体不同类型的返回值。类似于 switch 结构。

## Meta fields

有时候当你不知道服务器会返回什么类型的对象，而你必须要在客户端处理这些只时。GraphQL 允许你请求`__typename`,这个源属性。比如：你在搜索名字中有`an`时，服务器返回多个不同类型的对象，这是通过`__typename`你就可以知道这些对象的类型。

## Object types and fields

Graphql 最基本的组成部分是对象类型。

```graphql
type Character {
  name: String!
  appearsIn: [Episode!]!
}
```

- **Character**: 对象本身
- **name** and **appearsIn**： 对象的属性
- **String**： Graphql 的基本内置数据类型
- **String!**: 表示该属性为非空，服务器总是会返回该值，在你查询的时候。
- **[Episode!]!**：表示 Episode 数组

## 基本数据类型

- Int
- Float
- String
- Boolean
- ID

自定义类型：

`scalar Date`

```graphql
enum food {
  apple
  orange
  grape
}
```

## Interfaces

和大多数类系统一样，接口是一些具体属性的抽象。实现接口必须包含对应的属性。

## Union types

和接口类似，但是他们不需要制定任何相同的属性。(具体参看下面链接)

## Input types

通过`input`关键字来指定的类型，和普通的类型相似。可以用来创建和更新对应的对象。

```graphql
input CreateFood {
    name: apple
    price: 1
}
```

## Links

- https://graphql.org/learn
