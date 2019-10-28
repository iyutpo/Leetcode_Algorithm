# Inheritance and Polymorphism

当我们定义一个class的时候，可以从某个现有的class继承，新的class称为子类（Subclass），而被继承的class称为基类、父类或超类（Base class、Super class）。

例如， 我们已经编写了一个名为`Animal`的class，有一个`run()`方法可以直接打印'Animal is running...'：

```python
class Animal(object):
    def run(self):
        print('Animal is running...')
```

假设我们另有`Dog`和`Cat`两个类，这两个类都属于`Animal`的子类，且`Dog`和`Cat`都有`run()`方法。那么我们就可以让`Dog`和`Cat`继承`Animal`类：

```python
class Dog(Animal):
    pass
class Cat(Animal):
    pass

dog = Dog()
dog.run()
cat = Cat()
cat.run()
>>> Animal is running...
>>> Animal is running...
```

我们看到`Dog`和`Cat`这两个结果都是Animal is running，但是我们想将结果中的"Animal"变为"Dog"和"Cat"：

```python
class Animal(object):
  def run(self):
    print('animal is running')
  
class Dog(Animal):
  def run(self):
    print(Dog.__name__ + ' is running')

class Cat(Animal):
  def run(self):
    print(Cat.__name__ + ' is running')

dog = Dog()
dog.run()
cat = Cat()
cat.run()
>>> Dog is running
>>> Cat is running
```

当子类和父类都存在相同的`run()`方法时，我们说，子类的`run()`覆盖了父类的`run()`，在代码运行的时候，总是会调用子类的`run()`。这样，我们就获得了继承的另一个好处：**多态（Polymorphism）**。  
要理解什么是多态，我们首先要对数据类型再作一点说明。**当我们定义一个class的时候，我们实际上就定义了一种数据类型。我们定义的数据类型和Python自带的数据类型，比如str、list、dict没什么两样。这就是多态（Polymorphism）**：

```python
a = list() # a是list类型
b = Animal() # b是Animal类型
c = Dog() # c是Dog类型
```

判断一个变量是否是某个类型可以用`isinstance()`判断：

```python
isinstance(a, list)
>>> True
isinstance(b, Animal)
>>> True
isinstance(c, Dog)
>>> True
```

看来`a`、`b`、`c`确实对应着`list`、`Animal`、`Dog`这3种类型。

另外，由于`Dog`是继承自`Animal`类的，所以我们能料到，下面代码的结果也是`True`：

```python
isinstance(c, Animal)
>>> True
```

要理解多态的好处，我们还需要再编写一个函数，我们将这个函数想想成是一个非常复杂的函数（虽然该例子并不复杂）。这个函数接受一个`Animal`类型的变量：

```python
class Animal(object):
  def run(self):
    print('animal is running')

class Dog(Animal):
  def run(self):
    print(Dog.__name__ + ' is running')

class Cat(Animal):
  def run(self):
    print(Cat.__name__ + ' is running')

def complex_func(animal):
  animal.run()
  animal.run()

complex_func(Dog())
>>> 
Dog is running
Dog is running

a1 = Animal()
complex_func(a1)
>>> 
animal is running
animal is running

complex_func(Animal())
>>>
Animal is running
Animal is running
```

你会发现，新增一个`Animal`的子类，不必对`complex_func()`做任何修改，实际上，任何依赖`Animal`作为参数的函数或者方法都可以不加修改地正常运行，原因就在于多态。

多态的好处就是，当我们需要传入`Dog`、`Cat`时，我们只需要接收`Animal`类型就可以了，因为`Dog`、`Cat`……都是`Animal`类型，然后，按照`Animal`类型进行操作即可。由于`Animal`类型有`run()`方法，因此，传入的任意类型，只要是`Animal`类或者子类，就会自动调用实际类型的`run()`方法，这就是多态的意思：

对于一个变量，我们只需要知道它是`Animal`类型，无需确切地知道它的子类型，就可以放心地调用`run()`方法，而具体调用的`run()`方法是作用在`Animal`、`Dog`还是`Cat`对象上，由运行时该对象的确切类型决定，这就是多态真正的威力：调用方只管调用，不管细节，而当我们新增一种`Animal`的子类时，只要确保`run()`方法编写正确，不用管原来的代码是如何调用的。这就是著名的“开闭”原则：

对扩展开放：允许新增`Animal`子类；

对修改封闭：不需要修改依赖`Animal`类型的`complex_func()`等函数。

我们可以将若干个有继承关系的`class`抽象为一棵树。以上面的`Animal`、`Dog`和`Cat`为例，树的根节点就应该是`Animal`，我们应该已经能知道，`class Animal(object):`中的`object`就相当于根节点。然后两个`Animal`的子节点就是`Dog`和`Cat`

由于Python是动态语言，根据类创建的实例可以任意绑定属性。

给实例绑定属性的方法是通过实例变量，或者通过`self`变量：

```python
class Student(object):
    def __init__(self, name):
        self.name = name

s = Student('Bob')
s.score = 90    # 是的，我们可以从外部为实例s 定义一个变量 score，并赋值
# 这相当于在 s 的 __init__ 内添加了新变量，而对Student类并没有影响
```

那我们如何给`Student`类自身添加一个变量呢？我们可以在`Student`中直接添加变量，这个变量叫 类属性，仅归`Student`类所有。

```python
class Student(object):
    name = 'student'

# 由于创建Student类的同时就定义了name这个类属性，所以 s 的 name 属性也是 'student'
s = Student()
print(s.name)
>>> student

# 同理，Student类的 name 也是 'student'
print(Student.name)
>>> student

# 如果我们更改 s 的 name：
s.name = 'john'
print(s.name, ', ', Student.name)
>>> john, student

# 如果我们删掉 s 的 name:
del s.name
print(s.name)
>>> student

# 但是如果我们对Student类的 name 做改动， 由于 s 是继承自 Student的，所以 s 的 name 也自动发生改变：
print(s.name)
Student.name = 'myname'
print(s.name, ', ', Student.name)
>>> student
>>> myname, myname
```





















