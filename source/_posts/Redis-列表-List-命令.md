---
title: Redis 列表(List)命令
date: 2020-02-23 12:53:08
tags:
- Redis
- List
- 列表
categories:
- 数据库
- Redis
---
# Redis 列表(List)命令

## 前言

因为`List`列表类型和`String`类型有所不同，所以讲解顺序和方法也会有所改变。在我们对列表命令进行预览的时候，我们发现，列表命令的首字母分别是：**L**、**R**和**B**。这三者分别代表了什么呢？简单来说，这三个命令分别是*list*、*right*和*block*的缩写。意味着，这些命令的操作顺序是*从左开始*,*从右开始*以及命令具有*阻塞*的。因为这个特性，我们对命令进行梳理的时候，会对这些特殊的“前缀”做大写处理。

考虑到对命令进行分门别类有利于记忆，以及由浅入深有利于理解。我们对列表命令进行的分类和处理。

基本分为下面几大类：

+ L代表List的命令

+ 升级版命令

+ 阻塞命令



## 基础命令

### Llen

首先出场的是，简单的`Llen`命令，见名知意，是*List Len*的缩写，马上就会明白这个命令是统计某*key*所对应的列表的长度。如果列表并不存在，则返回0.用法如下：`Llen key`

### Lindex

*Lindex*根据索引获取列表中的元素，如果索引不在列表的区间范围，则返回空值(*nil*).是*List Index*的缩写。用法如下：`Lindex key index`。

我们在了解了如何获取了列表中的某个元素，我们应该如果获取列表中某个区间，甚至是全部元素呢？下面，就该`Lrange`出场了。

### Lrange

*Lrange*同样也是根据索引获取列表中的元素，是*List Range*的缩写。但是请注意，*Lrange*是通过*start*和*end*两个索引来获取两个索引中的全部元素的，其返回值还是一个列表，即原列表的子列表。用法：`Lrange key start end`。如果想获取全部元素，只需要将*start*和*end*分别设置为*0*和*-1*即可。

当我们想在某个位置插入元素的时候，Redis为我们提供了很多命令，我们首先介绍`Linsert`，因为它只出现在**L**分组中，而且简单。

### Linsert

是*List Insert*的缩写。*Linsert*是在列表中的某个元素后插入一个元素。用法：`Linsert key [before|after] name value`请注意几点：

+ 必须是列表，否则报错
+ *name*在列表中必须存在，否则返回0，对列表不进行操作
+ 操作成功后，返回列表长度

当我们需要修改列表中某个位置上的元素的时候，我们就需要使用`Lset`命令了。

### Lset

是*List Set*的缩写*Lset*根据索引，将列表中索引位置的元素设置为新值。`Lset key index value`。如果索引超过范围或者列表为空列表，则会返回一个错误。

### Lrem

是*List Remove*的缩写，意为从列表中删除元素。其命令为：`Lrem key count value`。命令的解读为，从*key*的列表中删除*count*个值为*value*的元素，并返回被移除元素的数量。其中，需要注意的一点是*count*的取值，*count*的大小为要删除的元素的个数，其正负性为从列表中开始查找的方向：

+ 正值： 从列表首向尾查找
+ 负值：从列表尾向首查找
+ 0：全部删除

需要了解到，虽然返回值为移除元素的数量，而*count*是期望删除的数量，但这两者并不一定相等，不相等的情况，一般有：

+ 列表中不存在和*value*相等的元素，返回值总为 0
+ 期望删除的元素个数比列表中存在的元素个数多
+ 期望参数值为*0*，且元素存在于列表中

### Ltrim
是*List Trim*的缩写，意为只保留列表中的区间元素，将其它部分删除。其命令为：`Ltrim key start end`。

### Lpush
是*List Push*的缩写，意为将一个或者多个元素插入到列表头部。其命令为：`Lpush key value1 ... valueN`.如果*key*不存在，则创建一个空列表，并执行*Lpush*操作。返回值为 操作完成后，列表的长度。

### Rpush
是*list Rigit Push*的缩写，这里将*list*省略了。命令和**Lpush**相似，只是元素的插入位置不再是列表头部，而是在列表尾部，即列表的**最右边**(*right*)。其命令为:`Rpush key value1 … valueN`。

### Lpop
是*List Pop*的缩写，意为取出列表中的第一个元素，其返回值为 列表的第一个元素。其命令为：`Lpop key`

### Rpop
是*list Right Pop*的缩写，意为取出列表中的最后一个(最右边第一个)元素，其返回值为 列表最后一个元素。其命令为：`Rpop key`.

## 升级版命令
### Lpushx
是*List Push if eXist*的缩写，是*Lpush* 的升级版，意为只有当列表存在时候才会执行操作。语法和*Lpush*一样：`Lpushx key value1 … valueN`。与*Lpush*的区别：

+ *Lpush* 当列表不存在的时候，创建空列表，然后执行*Lpush*操作
+ *Lpushx*当列表不存在的时候，不执行操作

### Rpushx

是*List Right Push if eXists*的缩写，同样是*Rpush*的升级版。是*Lpushx*对应的从列表尾插入的方法。其命令为：`Rpushx key value1 … valueN`。

### Rpoplpush

是*List Right Pop List Push*的缩写，意为从列表尾部取出一个元素，并添加到另一个列表的头部。其命令为`Rpoplpush source destation`;返回值为**移动的元素**。

当*source*和*destation*为同一个*key*值时，相当于将一个元素从列表尾部移动到列表头部。

## 含有（B）阻塞的命令

### BLpop,BRpop,BRpopLpush

分别是*Block List Pop*、*Block list Right Pop*和*“Block Right list Pop List Push*的缩写。为*Lpop*、*Rpop*和*RpopLpush*的**可阻塞**版。以*BLpop*为例，其命令为：`BLpop List1 List2 … ListN timeout`。Redis 会从尝试从所列的列表中弹出其中存在的第一元素，如果没有元素可以弹出，则会阻塞列表，直到有元素可以弹出。或者超时。

其中，阻塞列表需要满足*所列的列表中，任何列表中此时都没有元素*。

阻塞停止的条件满足下面任何一个即可：

+ 超时
+ 有操作向所列的列表中添加了元素，使得所列的列表中有元素可以弹出

返回值：

+ 超时： nil
+ 非超时：一个含有**2个**元素的列表
  1. 被弹出元素所属的 *key*
  2. 被弹出元素的值

*BRpop*的命令：`BRpop List1 List2 … ListN timeout`。返回值：同**BLpop**

*BRpopLpush*的命令：`BRpopLpush source destation timeout`。返回值：

+ 超时： nil
+ 非超时：一个含有**2个**元素的列表
  1. 被弹出的元素的值
  2. 等待时长

## 总结

我们可以从数据的**增**，**删**，**改**，**查**对*List*命令进行一下分类：

|   增    | 删    | 改   | 查     | 其它       |
| :-----: | ----- | ---- | :----- | ---------- |
|  Lpush  | LPop  | Lset | Lindex | BRpopLpush |
|  Rpush  | RPop  |      | Lrange | RpopLpush  |
| Lpushx  | BLpop |      | Llen   |            |
| Linsert | BRPop |      |        |            |
| Rpushx  | Lrem  |      |        |            |
|         | Ltrim |      |        |            |
|         |       |      |        |            |

通过上边的讲解和最后的分类汇总，我相信，大部分人已经对 Redis 的List有了初步的了解。下一篇文章，我们会对Set集合或者对key命令做一定的简单了解。欢迎大家指导。
