## list
Python内置的一种数据类型是列表：list。list是一种有序的集合，可以随时添加和删除其中的元素。

``` list = ['a','b','c']```

获取个数 `len(list)` 

获取最后一个 `list(-1)`

追加元素到末尾 `list.append('d')`

追加到指定位置 `list.insert(1,'e')`

删除末尾 ` list.pop() `

删除指定位置 `lsit.pop(1)`


## tuple

另一种有序列表叫元组：tuple。tuple和list非常类似，但是tuple一旦初始化就不能修改.

` tuple = ('a','b','c')`

现在，classmates这个tuple不能变了，它也没有append()，insert()这样的方法。其他获取元素的方法和list是一样的，你可以正常地使用classmates[0]，classmates[-1]，但不能赋值成另外的元素。

不可变的tuple有什么意义？因为tuple不可变，所以代码更安全。如果可能，能用tuple代替list就尽量用tuple。

