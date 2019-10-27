# Decorator

修饰器\(decorator\)

假设我们有一个函数 `add(x, y)`，我们想实现：每次调用`add(x, y)`时，都能在执行该函数之前，将该函数的函数名"add"打印出来。注意，我们不希望在`add(x, y)`函数内部进行改动！如：

```python
add(1, 2)
>> call add():
>> 3
```

我们可以在add\(x, y\)之前加一个修饰器。修饰器Syntax: `@decorator_name`

```python
# 我们先定义修饰器“log”，所以：
def log(func):
    def wrapper(*args, **kw):
        print('call %s(): ' % func.__name__)
        return func(*args, **kw)
    return wrapper

def add(x, y):
    return x + y

@log
def add(x, y):
    return x + y

print(add(1, 2))
>>> call add:
>>> 3
```

解释一下上面的Code Block:

首先定义一个修饰器“`log”`，`“log”`吃一个变量，该变量是一个函数`“func”`。然后在`wrapper`函数内，我们打印出`“call” + “func的函数名”`，并返回`“func”`函数。























