# 现场编程题

题一和题四要求候选人独立阅读完成，候选人可以提问题，面试官回答。但面试官不讲解整个题目。

# Tournament

Tally the results of a small football competition.

Tally the results of a small football competition. Based on an input file
containing which team played against which and what the outcome was
create a file with a table like this:

```
Team                           | MP |  W |  D |  L |  P
Devastating Donkeys            |  3 |  2 |  1 |  0 |  7
Allegoric Alaskans             |  3 |  2 |  0 |  1 |  6
Blithering Badgers             |  3 |  1 |  0 |  2 |  3
Courageous Californians        |  3 |  0 |  1 |  2 |  1
```

What do those abbreviations mean?

- MP: Matches Played
- W: Matches Won
- D: Matches Drawn (Tied)
- L: Matches Lost
- P: Points

A win earns a team 3 points. A draw earns 1. A loss earns 0.

The outcome should be ordered by points, descending. In case of a tie, teams are ordered alphabetically.

###

Input

Your tallying program will receive input that looks like:

```
Allegoric Alaskans;Blithering Badgers;win
Devastating Donkeys;Courageous Californians;draw
Devastating Donkeys;Allegoric Alaskans;win
Courageous Californians;Blithering Badgers;loss
Blithering Badgers;Devastating Donkeys;loss
Allegoric Alaskans;Courageous Californians;win
```

The result of the match refers to the first team listed. So this line

```
Allegoric Alaskans;Blithering Badgers;win
```

Means that the Allegoric Alaskans beat the Blithering Badgers.

This line:

```
Courageous Californians;Blithering Badgers;loss
```

Means that the Blithering Badgers beat the Courageous Californians.

And this line:

```
Devastating Donkeys;Courageous Californians;draw
```

Means that the Devastating Donkeys and Courageous Californians tied.

Your program should only process input lines that follow this format.
All other lines should be ignored:
If an input contains both valid and invalid input lines,
output a table that contains just the results from the valid lines.

# 2 Ordered Link List

根据输入的数组中每项的 before/after/first/last 规则，输出一个新排好序的数组或者链表。要求，多解的情况可以只求一解，如果无解要求程序能检测出来。**注意输入数组是无序的**，before 和 after 规则不需要**紧邻**着指定的元素，只要满足是 before/after 即可。

示例 Input:

```
[
    {id: 1},
    {id: 2, before: 1}, // 这里 before 的意思是自己要排在 id 为 1 的元素前面
    {id: 3, after: 1},  // 这里 after 的意思是自己要排在 id 为 1 元素后面 
    {id: 5, first: true},
    {id: 6, last: true},
    {id: 7, after: 8}, // 这里 after 的意思是自己要排在 id 为 8 元素后面
    {id: 8},
    {id: 9},
]
```

# 3 Create Tree from flat data

将输入的数组组装成一颗树状的数据结构，时间复杂度越小越好。要求程序具有侦测错误输入的能力。**注意输入数组是无序的**。

示例 Input:

```
[
    {id:1, name: 'i1'},
    {id:2, name:'i2', parentId: 1},
    {id:4, name:'i4', parentId: 3},
    {id:3, name:'i3', parentId: 2},
    {id:8, name:'i8', parentId: 7}
]
```

# 4 Dominoes

Make a chain of dominoes.

Compute a way to order a given set of dominoes in such a way that they form a
correct domino chain (the dots on one half of a stone match the dots on the
neighbouring half of an adjacent stone) and that dots on the halfs of the stones
which don't have a neighbour (the first and last stone) match each other.

For example given the stones `21`, `23` and `13` you should compute something
like `12 23 31` or `32 21 13` or `13 32 21` etc, where the first and last numbers are the same.

For stones 12, 41 and 23 the resulting chain is not valid: 41 12 23's first and last numbers are not the same. 4 != 3

Some test cases may use duplicate stones in a chain solution, assume that multiple Domino sets are being used.

Input example:
(1, 2), (5, 3), (3, 1), (1, 2), (2, 4), (1, 6), (2, 3), (3, 4), (5, 6)

# 5 设计事件总线

请设计一个事件总线工具，要求满足以下条件，请尽量完成能完成的最高难度。

难度一：可以是 class 或者 非 class 的形式。有基本的监听、卸载监听、多重监听、触发等基本功能，可以传递参数。以下只是示例代码，可以自行设计 api。

```javascript
const bus = new Bus()
bus.listen('testEvent', (...argv) => { 
  console.log('event callback')
})
bus.trigger('testEvent', 1, 2)
```

难度二：在 listener 中可以继续触发事件，要求在总线对象内部要保持正确的事件调用栈(树形)，并能提供接口打印出来，例如:
```javascript
bus.listen('testEvent', function callback1(){
  // do something
  this.trigger('testEvent2')
})

bus.listen('testEvent2', function callback2(){
    // do something
})

bus.trigger('testEvent')
/** 设计 api 和数据结构并打印出这次 trigger 内部所有发生的事件和监听信息
 * 期望得到的结果例如：
 * event: testEvent
 * --callback: callback1
 * ----event: testEvent2
 * ------callback: callback2
 * 
 * 注意，bus.trigger 应该可以执行多次，每一次trigger 都应该得到一个独立的事件栈。
 */
```

难度三：增加对 async callback 的支持，并要求仍然能够正确打印出调用栈。增加对无线循环调用事件的判断。
