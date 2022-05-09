我们都知道null是没有值，undefined是未定义

undefined是未定义，那么它也是没有值，所以它们的区别是什么呢？为什么需要null和undefined两个来表示无值



首先，我们先区分一下两者的 无值 的概念，null无值，它指的是可能无值；undefined无值，它指的是一定无值。

我们先来从作者设计JavaScript开始说起，当初设计JavaScript时，其实借鉴了Java的设计，Java有null，JavaScript也设计了null，但是null可以被隐式转换为0`(typeof(null) === 0)`，这个问题容易导致很严重的后果，最后作者为了填补null的设计缺陷，就新增了undefined。

所以undefined的出现，实质上是弥补了null的不足。

`typeof(undefined) ==> NaN`