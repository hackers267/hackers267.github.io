---
title: Redis 哈希(hash)命令
tags:
- Redis
- hash
- 哈希
categories:
- 数据库
- Redis
date: 2020-02-24 22:54:22
---
# Redis 哈希(hash)命令

## 前言

大概浏览了一下Redis中哈希*Hash*命令，发现当你对前边的字符串*String*，列表*List*和集合*Set*命令掌握的八九不离十的情况下，哈希*Hash*还是挺简单的。废话不多说，我们这就开始。我们从最基础的开始。

## 基础命令

### HSet

*HSet*是*Hash Set*的缩写。这个命令将哈希表*key*中的字段*field*的值设为*value*。其命令为：`HSet key field value`。需要说明的是，这个命令在哈希表存在的时候，会将*field*字段的值设置为*value*。当哈希表不存在的时候，首先初始化一个哈希表，然后再执行*HSet*操作。返回值：

+ 如果字段是在哈希表中新建的字段并且值设置成功，则返回**1**。
+ 如果该字段已经存在于哈希表中，并且字段的原值成功被新值覆盖，则返回**0**。

### HGet

*HGet*是*Hash Get*的缩写。这个命令用来读取哈希表中指定的字段值。如果哈希表中不存在该字段，则返回*nil*。

和*HGet*相似，存在一个命令，用来判断指定的字段是否存在于哈希表中，这个命令就是*HExists*。

### HExists

*HExists*是*Hash Exists*的缩写，用来判断指定的字段是否存在于哈希表中。其命令为：`HExists key field`。其返回值表示如下：

+ **0** -> 哈希表中不存在指定的字段，或者*key*不存在
+ **1**-> 哈希表中存在指定的字段。

### HLen

*HLen*是*Hash Len*的缩写，用来获取哈希表中字段的数量。其命令为：`HLen key`。返回值为哈希表中字段的数量。当*key*不存在的时候，返回**0**。

### HDel

*HDel*是*Hash Del*的缩写，用来删除哈希表中一个或者多个指定的字段，如果指定的字段不存在于哈希表中。那么，这个字段将会被忽略。其命令为：`HDel key field1 … fieldN`。返回值为被成功删除的字段。

## 升级命令

我们已经大致上了解了Redis中对哈希操作的一些基础命令。那么现在就让我们先更多的了解一下稍微*高级*的命令吧。

### HMSet

*HMSet*是*Hash Multi Set*的缩写，这个命令可以将多个*field-value*对设置到哈希表中。有了这个命令，就可以让我们多次调用*HSet*来给哈希表赋值了。其命令为：`HMSet key field1 value1 ... fieldN valueN`。如果命令执行成功，返回OK。

### HMGet

*HMGet*是*Hash Multi Get*的缩写，这个命令可以同时获取哈希表中的多个字段的值，其最后的返回值和字段是相对应的，如果某个字段在哈希表中不存在，则该字段对应的返回值为*nil*。其命令：`HMGet key field1 … fieldN`。

### HGetAll

*HGetAll*是*Hash Get All*的缩写。这个命令用来获取哈希表中的所有字段和所有的值，其返回格式也是一一对应的，一个字段后跟着一个值。命令很简单：`HGetAll key`。返回值是以列表形式存在的。

### HKeys

*HKeys*是*Hash Keys*的缩写。这个命令可以获取哈希表中的所有字段名。其命令为：`HKeys key`。返回值为一个包含哈希表中所有字段的列表。

### HVals

*HVals*是*Hash Values*的缩写。这个命令是和*HKeys*相对应的命令，用来获取哈希表中的所有字段值。其命令为：`HVals key`。返回值为一个包好哈希表中所有字段值的列表。

### HSetNX

*HSetNX*是*Hash Set if Not Exists*的缩写。其命令是用来创建哈希表中的字段值，当字段已经存在哈希表中的时候，不会执行操作。设置成功，返回**1**，否则返回**0**。

### HIncrBy

*HIncrBy*是*Hash Increment By*的缩写，其命令用来为哈希表中某个字段的整数值增加某个量。其命令为：`HIncrBy key field number`。此增量可为正数，也可为负数。当为负数的时候，相当于从原值减去此值。如果指定的字段不存在，则默认此字段为0.返回值为执行命令后，哈希表中该字段的值。

### HIncrByFloat

*HIncrByFloat*是*Hash Increment By Float*的缩写。命令为：`HIncrByFloat key field number`。和*HIncrBy*的区别在于*number*值可是小数。

## 总结

了解完了Redis中的哈希表，我们就已经了解了Redis中*五种*数据结构中的四种(*String*，*List*，*Set*和*Hash*)。最后剩下的就只有有序集合*Sorted Set*了。让我们明天再一起了解一下吧。
