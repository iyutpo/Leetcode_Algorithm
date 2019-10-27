# Map Reduce Lambda

Python内部的`map()`和`reduce()`函数

#### map\(\)函数

Syntax: map\(函数, Iterable variable\)。`map()`函数吃两个参数，第一个是函数名，第二个是“可迭代变量”。举例来看，如果我们想求 f\(x\) = x^2

我们可以先实现一个求平方的函数 `foo(x)`：

```python
def foo(x):
    return x * x
```

`map()`函数能够将`foo(x)`函数依次作用到Iterable variable中的每个元素上。现在假设该Iterable variable为一个tuple，所以我们可以这样写代码：

```python
def foo(x):
    return x * x

res = map(foo, (1, 2, 3, 4, 5))
for i in res:
    print(i)
>>> 1
>>> 4
>>> 9
>>> 16
>>> 25

# res 的数据类型是 <map>
print(type(res))
>>> 'class <map>'
```



#### reduce\(\)函数

Syntax: reduce\(函数名, Iterable variable\)

假设现在有一个函数`f`如下：

```python
from functools import reduce
def f(x, y):
    return x + y
    
reduce(f, [1, 4, 6, 8])
>>> 19
```

reduce\(\)函数的实现过程如下：

`reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)`

所以上面Code Block实现的就是对于一个Array  List的求和。虽然我们可以用`sum()`对一个Array  List求和，但是如果我们想将一个Array  List转换为一个大整数（如将\[1, 4, 6, 8\]变为1468），我们就可以这么写函数：

```python
from functools import reduce
def f(x, y):
    return x * 10 + y

reduce(f, [1, 4, 6, 8])
>>> 1468
```

上面Code Block的执行过程如下：

```text
reduce(f, [x1, x2, x3, x4])
= f(f(f(x1, x2), x3), x4)

= f(f(10 * x1 + x2), x3), x4)
= f(100 * x1 + 10 * x2 + x3, x4)
= 1000 * x1 + 100 * x2 + 10 * x3 + x4
= 1000 * 1 + 100 * 4 + 10 * 6 + 8
= 1468
```

另一个例子是，将一个字符串变成整数（如 '12345' --&gt; 12345）

```python
from functools import reduce
def f(x, y):
    return x * 10 + y

def char2num(s):
    digits = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
    return digits[s]

reduce(f, map(char2num, '12345'))
>>> 12345
```

上面Code Block的执行过程是：

```text
先执行map(char2num, '12345')：
map(char2num, '12345')
'1', '2', '3', '4', '5'
 |    |    |    |    |
 v    v    v    v    v
 1    2    3    4    5

然后执行 reduce(f, map object)：
注意，map object也是一个Iterable variable。
此时map object中有1，2，3，4，5这几个元素
reduce(f, map object)
= f(f(f(f(x1, x2), x3), x4)
, x5)
= f(f(f(10 * x1 + x2, x3), x4), x5)
= f(f(100 * x1 + 10 * x2 + x3, x4), x5)
= f(1000 * x1 + 100 * x2 + 10 * x3 + x4, x5)
= 10000 * x1 + 1000 * x2 + 100 * x3 + 10 * x4 + x5
= 12345
```

#### lambda\(\)函数

Syntax: `lambda var1, var2, ..., varN: f(var1, var2, ..., varN)`

例子：假设我们想用lambda函数简化下面的函数`f`：

```python
def f(x, y):
    return x * 10 + y
```

那我们能写成：

```python
lambda x, y: 10 * x + y
```

对于上面的“字符串转整数”的函数`str2int(s)`（如下）

```python
from functools import reduce
def str2int(s):
    def f(x, y):
        return x * 10 + y
    def char2num(s):
        digits = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
        return digits[s]
    return reduce(f, map(char2num, s))
```

我们可以将`str2int(s)`用`lambda`转换为：

```python
from functools import reduce
digits = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
def char2num(s):
    return digits[s]
def str2int(s):
    return reduce(lambda x, y: x * 10 + y, map(char2num, s))

# 上面的 lambda x, y: x * 10 + y  就相当于 f(x, y)函数
```













