# python

## 0x1 数据结构和算法

### 1. 分解数据

1个包含N个元素的元组或序列，现在需要分解为N个单独的变量。

任何序列（或可迭代的数组）都可以通过一个简单的赋值来分解，唯一的要求是变量的总数和结构要与序列吻合

eg：

```python
>>> data = ["acme", 50, 91.1, (2012.12.21)]
>>> name, share, price, date = data
>>> name 
"acme"
>>> data
[50, 91.1, (2012.12.21)]
```

只要对象是可迭代的就可以执行分解操作，包括字符串、文件、迭代器以及生成器等。

如果分解时想丢弃部分特定值，通常可以选一个用不到的变量名，以此来作为要丢弃的值得名称。

eg:

```python
>>> data = ["acme", 50, 91.1, (2012.12.21)]
>>> _, share, price, _ = data
>>> share
50
>>> price
91.1
```

但是要确保使用的变量名没有在其他地方使用过

### 2 从任意长度的可迭代对象中分解数据

如果需要从可迭代对象中分解N个元素，但是可迭代对象长度可能超过N，就会出现**"分解值过多（too many values to unpack）"**异常。

**解决方案：**python的”*表达式“可以解决

eg:

```python
def drop_first_last(grades):
    first, *middle, last = grades
    return avg(middle)
```

分解操作和各种函数式语言中的列表处理功能有一定的相似性，比如实现递归算法：

```python
def sum(items):
	head, *tail = items
	return head + sum(tail) if tail else head

```

但是递归不是python强项，因为python有内置的递归限制，无太大的实践意义。

### 3 保留最后N个元素

如果是相对处理过程中最后几项记录做一个有限的记录保存。

**方法：**保存有限的历史记录可以算是collection.deque的完美应用场景。

eg: 对一系列文本行进行简单的匹配操作、

```python
from collection import deque

def serach(lines, pattern,history=5):
    previous_lines = deque(maxlen = history)
    for line in lines:
        if pattern in line:
            yield line, pervious_lines
        previous_lines.append(line)
        
# deque(maxlen = N)创建一个固定长度的队列。当有新纪录加入而队列已满时会自动移除掉最老的那条
>>> q = deque(maxlen = 3)
>>> q.append(1)
>>> q.append(2)
>>> q.append(3)
>>> q
deque([1,2,3], maxlen=3)
>>> q.append(4)
>>>q
deque([2,3,4], maxlen=3)
```

尽管可以在列表上手动完成这种操作（append，del），但队列这种解决方案更优雅且运行速度更快。

当需要一个队列结构时，deque也可以发挥作用，如果不指定队列大小，就得到一个无边界的队列，可以在两端执行push和pop操作：

```
>>> q = deque()
>>> q.append(1)
>>> q.append(2)
>>> q.append(3)
>>> q 
deque([1,2,3])
>>> q.appendleft(4)
>>> q
deque([4,1,2,3])
>>> q.pop()
3
>>> q.popleft()
4
```

从队列两端添加或者弹出元素复杂度都是O(1)。这种和列表不同，当从列表的头部插入或移除元素时，复杂度都是O(N)。

