# 面向对象
## 类 属性 方法 对象 实例
- 方法  
    方法也是属性，动态属性
- 实例  
    类实例化后的实例就是对象，对象只有一种作用，即属性引用。方法的调用就是动态属性的引用
- 实例化  
    - 首先开辟内存空间，调用`__init__`函数，对象名（self）指向该内存空间的地址
    - `__init__`把传入的参数（属性值）存储在self的空间中
    - `__init__`没有return语句，self指向的地址会作为返回值返回给实例

```python
class Car:
    def __init__(self, price, car_type):
        self.price = price
        self.car_type = car_type

    def start_engin(self):
        print('start engin')

    def speed_up(self):
        print('speed up')     
        
```

- 类的加载顺序
    - 开辟内存空间
    - 执行类里面的代码，包括创建类变量、类属性、类方法（？？？类变量和类属性是同一个东西吗）
    - 类名A指向内存地址
- 实例化加载顺序
    - 开辟内存空间
    - 对象名a指向该内存地址
    - 对象名a会有一个指针指向类A的地址
- 实例方法的调用
    - 对象a先通过指针找到类A
    - 再找到类A的方法func

```python
class A:
    """docstring for A"""
    x = 0 # 类属性，静态属性


    def __init__(self, y):
        self.y = y # 实例属性

    def func(self):
        print(self) # 打印实例的地址

a = A(3)

a.func() # 等价于A.func(a)

a.x = 1 # 不会更新静态变量的值，因为这个赋值操作，会在对象的命名空间内创建对象属性x
print(A.x)
print(a.x)
```


## 继承
### super方法
```python
class A:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def hahaha(self):
        print('A')

    def func(self, a):
        print(f'A:{a}')

class B(A):
    def __init__(self, x, y, z):
        super().__init__(x, y)
        self.z = z

    def hahaha(self):
        super().hahaha() # 与以下两者等价
        super(B, self).hahaha()
        A.hahaha(self) # 通过类名调用方法，不会自动传对象，需要手动加入self，这里的self是子类的self，是子类的去实现父类的同名方法，当然指向子类的对象
        print('B')

    def func(self, a):
        super().func(a)
        print(f'B:{a}')

a = A(1, 2)
a.hahaha()
a.func(11)
b = B(1, 2, 3)
b.hahaha()
b.func(11)
```


#### 练习
```python
class A:
    lst = []
    def func(self):
        self.lst.append(1)

class B(A):
    lst = []
    def func(self):
        self.lst.append(2)

b = B()
b.func()
print(A.lst) # []
print(B.lst) # [2]


class A:
    lst = []
    def func(self):
        self.lst.append(1)

class B(A):
    def func(self):
        self.lst.append(2)

b = B()
b.func()
print(A.lst) # [2]
print(B.lst) # [2]
```


## 绑定方法 vs 函数
```python
from types import FunctionType, MethodType

class Animal:pass
class Dog(Animal):pass

wangcai = Dog()
print(type(wangcai) is Dog) 
print(type(wangcai) is Animal) # 类型只能判断一层，无法判断继承关系

print(isinstance(wangcai, Dog))
print(isinstance(wangcai, Animal)) # isinstance 能判断继承关系


class A:
    def func():
        print('in func')

a = A() # 叫函数还是绑定方法，关键是看谁调用了它
print(A.func) # 类调用时叫函数
print(a.func) # 实例调用时叫绑定方法

print(isinstance(A.func, FunctionType))
print(isinstance(A.func, MethodType))
print(isinstance(a.func, FunctionType))
print(isinstance(a.func, MethodType))
```

## 多继承
### 新式类 vs 经典类
- 只要继承object类的都是新式类
    - python3中所有的类都继承object类，所以都是新式类，故python3不存在经典类
- 不继承object类的都是经典类
    - python2中那些不继承object类的都是经典类
    
#### 继承顺序
- 新式类通过继承，按照广度优先的顺序，向父类寻找自己未定义的属性或方法。即当可以横向也可以纵向寻找时，优先去横向寻找
    - 所谓的广度优先，不是单纯先横向，再纵向，会采用c3算法(通过mro方法查看继承顺序)做优化，减少指针移动的次数
- 经典类采用深度优先
- super方法是按照mro顺序寻找父类的

```python
class A:
    def func(self):
        print('A')

class B:
    def func(self):
        print('B')

class C(A):
    pass

class D(B, C):
    pass

print(D.mro()) # 新式类，用mro方法用c3算法查看寻址顺序


class A:
    def func(self):
        print('A')

class B(A):
    def func(self):
        super().func()
        print('B')

class C(A):
    def func(self):
        super().func()
        print('C')

class D(B, C):
    def func(self):
        super().func()
        print('D')


D().func() # 新式类，实例化后调用方法，用mro方法用c3算法查看寻址顺序
```


#### 练习
**？？？未完成**

```python
# 创建一个Student类，实例化，并打印这个实例是第几个被实例化的对象。
class Student:
    count = 0
    _count = []
    _name = []

    def __init__(self, name):
        self.name = name
        Student.count += 1
        Student._count.append(Student.count)
        Student._name.append(self.name)

    def number_instantiated(self):
        dic = dict(zip(Student._name, Student._count))
        return dic[self.name]

ryan = Student('ryan')
oligen = Student('oligen')
isaac = Student('isaac')
print(isaac.number_instantiated())

# 登录，且能修改密码

class User(object):
    """docstring for User"""
    def __init__(self, username, password):
        self.username = username
        self.password = password

    def change_pwd(self):
        pass

name = input('user: ')
pwd = input('password: ')




```

### 私有属性（方法）

```python
class Foo:
    def __init__(self):
        self.func()

    def func(self):
        print('in foo')

class Son(Foo):
    def func(self):
        print('in son')

Son() # in son ，实例化时调用方法func函，在类Son的命名空间内存在func


class Foo:
    def __init__(self):
        self.__func()

    def __func(self):
        print('in foo')

class Son(Foo):
    def __func(self):
        print('in son')

Son() # in Foo ，实例化时调用私有方法__func，在类Son的命名空间内没有_Foo__func方法，所以去父类里找

        
```

#### 练习
1.使用pickle存取自定义类的对象的方式
2.P130 0-33'' 作业
3.P135作业
4.P138 38''30' pickle
5.P140 作业讲解
6.队列Queue，数据遵循FIFO原则；栈Stack，数据遵循LIFO原则，有put、get方法。用面向对象实现上述两个类，使用继承简化代码
7.自定义实现Mypickle类，借助pickle模块，实现load、dump方法
8.自定义实现Myjson类，实现多次load、dump方法
9.P146 13''09'-38''作业，38''后总结

### 多态
python中一切皆对象，处处是多态。java必须通过继承，使用共同父类来实现多态

### 鸭子类型

### 封装
- 广义的封装：把属性和方法装在类里，外部不能直接调用，要通过类的名字来调用
- 狭义的封装：把属性和方法藏起来，外部不能调用，只能在类内部调用

```python
import hashlib

class User:
    __Country = 'China' # 私有变量，外部不能调用
    def __init__(self, name, pwd):
        self.user = name
        self.__pwd = pwd # 私有实例变量（私有对象属性）

    def __get_md5(self):
        '''私有方法'''
        md5 = hashlib.md5(self.user.encode('utf8'))
        md5.update(self.__pwd.encode('utf8'))
        return md5.hexdigest()

    def get_pwd(self):
        '''用户只能看加密后的密码'''
        return self.__get_md5()

ryan = User('ryan', '123')
print(ryan.__pwd) # 报错
print(ryan.get_pwd())

```

- 封装的语法
    + 私有的静态变量
    + 私有的实例变量
    + 私有的绑定方法
- 封装的原理
    + `class A: __Color = 'red'`
    + `a = A(); print(a._A__Color) # 私有变量可以通过_类名__私有变量名 访问`














