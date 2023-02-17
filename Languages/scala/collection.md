# 容器

## Traverable

所有的集合类都继承了 `Traverable` 接口，也就都具有相似的功能

![](../../Picture/Languages/scala/02.svg)

`Traversable` 是容器类的最高级别特性，它唯一的抽象操作是 `foreach`

```scala
def foreach[U](f: Elem => U)

/*
操作类型是 Elem => U
其中 Elem 是容器中元素的类型，U 是一个任意的返回值类型
*/
```

对 `f` 的调用仅仅是容器遍历的副作用，实际上所有函数 `f` 的计算结果都被 `foreach` 抛弃了

### 相加

`++` 把两个 `traversable` 对象附加在一起或者把一个迭代器的所有元素添加到 `traversable` 对象的尾部

### Map

通过对容器中的元素进行某些运算来生成一个新的容器

### 转换器

包括 `toArray`、`toList`、`toIterable`、`toSeq`、`toIndexedSeq`、`toStream`、`toSet` 和 `toMap`

### 拷贝

`copyToBuffer` 和 `copyToArray` 分别用于把容器中的元素元素拷贝到一个缓冲区或者数组里

### Size info

包括有 `isEmpty`、`nonEmpty`、`size`和 `hasDefiniteSize`

`Traversable` 容器有有限和无限之分

### 元素检索

有 `head`、`last`、`headOption`、`lastOption` 和 `find`

查找容器的第一个元素或者最后一个元素，或者第一个符合某种条件的元素

不是所有的容器都明确定义了什么是第一个或最后一个

### 子容器检索

有 `tail`、`init`、`slice`、`take`、`drop`、`takeWhilte`、`dropWhile`、`filter`、`filteNot` 和 `withFilter`

可以通过范围索引或一些论断的判断返回某些子容器

### 拆分

有 `splitAt`、`span`、`partition` 和 `groupBy`

用于把一个容器里的元素分割成多个子容器

### 元素测试

有 `exists`、`forall` 和 `count`

用一个给定论断来对容器中的元素进行判断

### 折叠

有 `foldLeft`、`foldRight`、`reduceLeft` 和 `reduceRight`