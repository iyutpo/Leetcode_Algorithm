# Return\_a\_Function

## Return\_a\_Function

在Python中，我们可以将一个函数作为函数的返回值，如：

```python
def cal_sum(*args):
    ax = 0
    for n in args:
        ax = ax + n
    return ax

def f(x, y):
  return 10 * x + y

print(cal_sum(10, f(3, 2)))
>>> 42
```

如果我们不想立即求和，而是在之后的代码中根据需要再计算求和怎么办？这时候就需要只返回求和的函数，而不是求和的结果：

```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum        # 如果是 return sum() 就是返回求和的结果

# 然后我们调用lazy_sum，并使lazy_sum只返回求和函数：
f = lazy_sum(1,2,3,4)
print(f)
>>> <function lazy_sum.<locals>.sum at 0x7fc35149c3b0>

# 调用lazy_sum，并返回求和结果：
res = lazy_sum(1,2,3,4)()    # 注意最后的括号
print(res)
>>> 10
```

在这个例子中，我们在函数`lazy_sum`中又定义了函数`sum`，并且，内部函数`sum`可以引用外部函数`lazy_sum`的参数和局部变量，当`lazy_sum`返回函数`sum`时，相关参数和变量都保存在返回的函数中，这种称为“**闭包（Closure）**”的程序结构拥有极大的威力。

```python
# 请再注意一点，当我们调用lazy_sum()时，每次调用都会返回一个新的函数，即使传入相同的参数：
f1 = lazy_sum(1,2,3,4)
f2 = lazy_sum(1,2,3,4)
print(f1 == f2)
>>> False
# 我们可以看一下这两个函数 f1, f2：
print(f1)
print(f2)
>>> <function lazy_sum.<locals>.sum at 0x7f485cac54d0>
>>> <function lazy_sum.<locals>.sum at 0x7f485cac5440>

# 但是如果我们返回的是求和函数结果，那么等式前后就是相等的：
print(lazy_sum(1,2,3,4)() == lazy_sum(1,2,3,4)())
>>> True
```

**闭包\(Closure\)**

注意到返回的函数在其定义内部引用了局部变量`args`，所以，当一个函数返回了一个函数后，其内部的局部变量还被新函数引用，所以，闭包用起来简单，实现起来可不容易。

**另一个需要注意的问题是，返回的函数并没有立刻执行，而是直到调用了`f()`才执行。**我们来看一个例子：

```python
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()
```

在上面的例子中，每次循环，都创建了一个新的函数，然后，把创建的3个函数都返回了。

你可能认为调用`f1()，f2()`和`f3()`结果应该是1，4，9，但实际结果是：

```python
f1()
>>> 9
f2()
>>> 9
f3()
>>> 9
```

全部都是`9`！原因就在于返回的函数引用了变量`i`，但它并非立刻执行。等到3个函数都返回时，它们所引用的变量`i`已经变成了`3`，因此最终结果为`9`。

**返回闭包时牢记一点：返回函数不要引用任何循环变量，或者后续会发生变化的变量。**

如果一定要引用循环变量怎么办？方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变：

```python
def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs

# 结果是：
f1, f2, f3 = count()
f1()
>>> 1
f2()
>>> 4
f3()
>>> 9
```

缺点是代码较长，可利用lambda函数缩短代码。

