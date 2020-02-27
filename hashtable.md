### 哈希函数

可以设想一下，如果有一个公司，需要存储员工信息，包括姓名，手机号等等，你会用什么数据结构去储存。

我们第一反应是数组。

```javascript
var arr = [
  {
    name: '张三',
    phone: '1832045345'
  },
  {
    name: '李四',
    phone: '1832045545'
  }，
  ...
]
```
每来一个新员工，我们就添加一个进去。

但是有一天hr想要知道李四的电话，这下怎么办？我们只能循环数组，找到这个name，再给出电话。看来数组的方案有点缺点。

那能不能用链表呢，很明显，并不能解决这个问题。

思考一下，既然我们可以通过下标很快的拿到数组元素，那我们能不能将姓名转成下标，这样只要知道姓名，就能知道下标，也就能很快知道电话呢？
这就是最初的**哈希函数**了，将单词转成数字。

一个好的哈希函数需要满足1. 快速的计算，所以尽量避免乘除法，2. 均匀的分布，将元素均匀映射，不形成聚集

霍纳法则

### 如何实现的

我们可以设想一个很简单的字母转数字，asscII码，我们就可以将每个字母转成的数字相加之和和这个单词对应起来，就是很简单的哈希转化了。

比如cats => 3+1+20+19=43

但是这个只适用于英文，而且可以知道，会有很多单词的转化数值是相等的。

比如was/tin/give...等等

我们有没有办法能减小这种冲突呢。一种普世方法就是幂的连乘
cats => 3*27^3 + 1*27^2 + 20*27 + 19 = 60337
这样就能基本避免冲突

### 还有什么问题

上面的方法还是有问题的，可能有的单词很长，那我们转化出来的数值就会比较大，申请的内存也会很多，先不说能不能有那么大的内存，一个很明显的问题就是，实际上很多的空间我们是用不到的，那我们需要一个办法来压缩数值。这就是**哈希化**了

### 哈希化
比如我们要存100个数据，但是我只想给0-9的下标，那我们的下标值就是 index = 100 % 10，index一定在0-9之间
比如 135 % 10 = 5，164 % 10 = 4
这样我们就将空间节省下来。可以看到**哈希化**就是将大数字转化成小数字

### 哈希表
上面一系列操作后，将这个数据插入我们的数组，对整个结构的封装，我们就称为**哈希表**

### 又有什么问题
上面的问题很明显，125，115，等等的转化数字都会一样，我们没办法区分到底是哪个。这就是**哈希冲突**了。

### 解决冲突

#### 链地址法（拉链法）
我们将数组的每个值存为链表，这样有冲突的元素，先通过下标找到链表，再添加进链表就可以了

#### 开放寻址法
还是上面的例子，我们要存在第二个位子，发现已经有值了，那我们向下找第三个，如果有空就存下来。

至于如果找到空白位置，有三种方法

开放寻址法随着添加，填充影子增大，填充效率指数降低，所以一般用链地址法
##### 线性探测

一直向下找，知道找到空白。会引起聚集，即一连串填充单元，会影响性能

##### 二次探测

对步长做优化，比如每次递增。但是还是会造成步长不一的聚集

##### 再哈希法

依赖关键字的探测序列。即将关键字再做一次哈希化，将其作为步长，这样不同的关键字就有不同的步长

step = constant - （key % constant），constant为质数，且小于数组容量

### 哈希化效率

探测长度，填充因子

填充因子 = 总数据项/整个哈希表长度， 开放地址法的填充因子最大是1，链地址法大于1