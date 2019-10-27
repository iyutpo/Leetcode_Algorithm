# Class

类是面向对象编程中最重要的概念之一（另一个是“实例”\(Instance\)）。**类是抽象的模板，比如Student类，而实例是根据类创建出来的一个个具体的“对象”**，每个对象都拥有相同的方法，但各自的数据可能不同。

```python
class Student(object):
    pass
```

`class`后面紧接着是类名，即`Student`，类名通常是大写开头的单词，紧接着是`(object)`，表示该类是从哪个类**继承**下来的。通常，如果没有合适的继承类，就使用`object`类，这是所有类最终都会继承的类。

定义好了`Student`类，就可以根据`Student`类创建出`Student`的实例，创建实例是通过`类名+()`实现的：`s1 = Student()`。

由于类可以起到模板的作用，因此，可以在创建实例的时候，把一些我们认为必须绑定的属性强制填写进去。通过定义一个特殊的`__init__`方法，在创建实例的时候，就把`name`，`score`等属性绑上去：

```python
class Student(object):
    def __init__(self, name, score):
        self.name = name
        self.score = score
```

如果熟悉C++，我们可以将`self.name`和`self.score`理解为`Student`类内部的公有变量\(`public`\)。

如果Student类内还有其他函数print\_score，我们需要调用`self.name`和`self.score`：

```python
class Student(object):
    def __init__(self, name, score):
        self.name = name
        self.score = score
    def print_score(self):
        print('%s: %s' % (self.name, self.score))

s1 = Student('john', 60)
s1.print_score()
>>> john: 60
```

#### 设置访问权限

我们刚刚提到了类内的`public`成员变量是可以被外部访问的。与之相反，`private`成员变量就是不能被外部访问的。private成员变量以双下划线开头：`__varname`。所以我们对上面的例子略微修改如下：

```python
class Student(object):
    def __init__(self, name, score):
        self.__name = name
        self.score = score
    def print_score(self):
        print('%s: %s' % (self.__name, self.score))

s1 = Student('john', 60)
s1.print_score()
>>> john: 60

# 从外部进行访问score 和 __name：
print(s1.score)
>>> 60
print(s1.__name)
>>> Traceback (most recent call last):
  File "main.py", line 14, in <module>
    print(s1.__name)
AttributeError: 'Student' object has no attribute '__name'
```

你可能会问为什么要设置权限？

还是上面的例子，假设我们在开发一个学校学生分数的数据库，我们只希望内部的人进行分数记录，而不希望给外部的人或学生权限，否则外部人就能自由改动自己的分数。此时我们就有必要将`name`和`score`都变为`private`变量。













