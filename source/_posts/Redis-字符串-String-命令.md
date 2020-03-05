---
title: Redis 字符串(String)命令
date: 2020-02-23 12:51:36
tags:
- Redis
- String
- 字符串
categories:
- 数据库
- Redis
---
# Redis 字符串(String)命令

## 基本命令

Redis中基本的字符串命令包括 `set`、`get`和`strlen`。其用法很简单。其中 get 的用法为 `get key`,`set`的基本用法为 `set key value`,而`strlen`的用法为`strlen key`,返回*key*的字符串长度。而在现实中，我们经常的需求往往并不是这么简单。那么在这个时候，我们就需要一些升级后的命令了。

## 升级后的命令

我们在实际的需求中，可能会遇到这样的需求，我们需要先取得某个 key 的值，然后将这个 key 赋值成为 我们期望的某个值。在这样的情况下，我们往往用基本命令也能完成这样的需求：

```shell
get key
set key value
```

为了操作简便，Redis为我们提供了升级后的命令：`getset`.顾名思义，这个值会设置某个键为新值，并返回原值。用法和`set`相似：`getset key value`;除了`getset`之外，Redis还为我们设置了像`incr`,`desc`,`incrby`,`descby`,`incrbyfloat`对值是*number*的*key*的值进行自增或自减的升级命令（注：返回值为自增或自减后的值）。

`mset`和`mget`则可以同时设置多个*key*的值，或者同时获取多个*key*的值。

`setex`和`psetex`是对`set`的升级，它的功能除了可以给*key*设定值之外，还会为*key*设定过期时间。其中的区别是：

+ setex的过期时间以**秒**为单位
+ psetex的过期时间以**毫秒**为单位

`setnx`也是对`set`的升级，它只在*key*存在的时候才会更新值。并且返回值直接和成功与否有关：

+ 成功->1
+ 失败->0

`append`则是当*key*存在的时候并且值为字符串时，将*value*直接追加到字符串末尾，而当`key`不存在时，则和`set`是一样的效果。

`msetnx`是对`mset`或者说是`setnx`的升级。它只有在所有*key*都存在的情况下，才对所有的`key`进行更新操作。返回值如下：

+ 成功->1
+ 失败->0

`getrange`和`setrange`是对`get`和`set`针对子字符串的升级操作，分别是获取字串和设置子串，用法为`getrange key start end`和`set key offset value`;

在使用`getrange`和`setrange`的时候，其中的`start end`和`offset`需要注意。其中，当`start`大于字符串最后一个字符的索引时，直接返回空字符串。当值为负数的时候，即为从字符串的最后一个字母开始向前数。当`offset`大于最后一个字符的索引的时候，在最后一个索引和`offset`之间会填充空格，表现形式为`\x00`.

## 高级命令

说是高级命令，其实是对字符串的非直接操作，而是对其对应的ASCII吗的操作。`getbit`是获取某个*key*的指定偏移量*位*(bit)上的值，而`setbit key offset value` 则指定*key*上的*offset*d 的值。
