---
title: 2019.3.3竞赛总结
date: 2019-03-07 10:31:28
categories: note
tags: 思路总结
---
竞赛当中学习到的思路总结
<!--more-->
### collections总结
This module implements specialized container datatypes providing alternatives to Python’s general purpose built-in containers, dict, list, set, and tuple. 

| 函数名 | | 函数描述 |
| ---- | - | ---- |
| namedtuple | | factory function for creating tuple subclasses with named fields |
| deque | | list-like container with fast appends and pops on either end |
| ChainMap | | dict-like class for creating a single view of multiple mappings |
| Counter | | dict subclass for counting hashable objects | 
| OrderedDict | | dict subclass that remembers the order entries were added |
| defaultdict | | dict subclass that calls a factory function to supply missing values |
| UserDict | | wrapper around dictionary objects for easier dict subclassing |
| UserList | | wrapper around list objects for easier list subclassing |
| UserString | | wrapper around string objects for easier string subclassing |

### 滑动窗口思想
有两个指针构成一个区间，通过往同一个方向不停的移动，来找到满足条件的区间。 

滑动窗口关键的是想想初始时候，两个指针在哪，在滑动的时候，两个指针基于什么条件移动，移动后的位置又在哪里。

### 动态规划
dp算法需满足以下性质
1. 最优子结构性质。如果问题的最优解所包含的子问题的解也是最优的，我们就称该问题具有最优子结构性质（即满足最优化原理）。最优子结构性质为动态规划算法解决问题提供了重要线索。  

2. 子问题重叠性质。子问题重叠性质是指在用递归算法自顶向下对问题进行求解时，每次产生的子问题并不总是新问题，有些子问题会被重复计算多次。动态规划算法正是利用了这种子问题的重叠性质，对每一个子问题只计算一次，然后将其计算结果保存在一个表格中，当再次需要计算已经计算过的子问题时，只是在表格中简单地查看一下结果，从而获得较高的解题效率。 

当我们已经确定待解决的问题需要用动态规划算法求解时，通常可以按照以下步骤设计动态规划算法:
1. 分析问题的最优解，找出最优解的性质，并刻画其结构特征 

2. 递归地定义最优值 

3. 采用自底向上的方式计算问题的最优值 

4. 根据计算最优值时得到的信息，构造最优解 

在python中可通过`@functools.lru_cache(None)`实现重复计算直接取值