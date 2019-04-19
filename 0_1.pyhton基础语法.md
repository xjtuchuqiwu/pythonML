<h1> python基础语法</h1>
<!-- TOC -->

- [一: python内置函数](#一-python内置函数)
    - [1.基本函数](#1基本函数)
    - [2.高阶函数 map reduce filter sorted](#2高阶函数-map-reduce-filter-sorted)
- [二: 函数](#二-函数)
    - [1.函数的创建](#1函数的创建)
    - [2.默认参数](#2默认参数)
    - [3.可变参数](#3可变参数)
    - [4.关键字参数](#4关键字参数)
    - [5.参数组合](#5参数组合)
    - [6.匿名函数](#6匿名函数)
- [三: 切片](#三-切片)
    - [1.切 list tuple str](#1切-list-tuple-str)
    - [2.列表生成器 range -> list](#2列表生成器-range---list)
- [四: 生成器](#四-生成器)
- [五: Iterable Iterator](#五-iterable-iterator)
- [六: 类](#六-类)
    - [1.定义类](#1定义类)
    - [2.继承类](#2继承类)
    - [2.类实例](#2类实例)
    - [3.类的访问权限](#3类的访问权限)
    - [4.创建类属性](#4创建类属性)
    - [5.定义类方法](#5定义类方法)

<!-- /TOC -->
#### 一: python内置函数
##### 1.基本函数
abs max min int str hex 
```python
print(abs(-100))              #abs()求绝对值函数
print(abs(-12.34))            #也可以对小数求绝对值

print("max=",max(1,2,3,4,5))  #max函数传入多个参数，返回最大的那个

#python的类型转换
print(int(12.3))             #int()函数可以将其他类型转换为int类型,包括字符串,小数会截断
print(int("12"))
print(str(123))              #str()函数将其他类型转换为字符串
print(bool(0))               #bool函数将数字转换为true false,除了0都是true

n1=25
print(hex(n1))               #利用hex()函数将整数转换为十六进制字符串

#待补充
```
##### 2.高阶函数 map reduce filter sorted
map reduce filter sorted
```python
#map函数:获取每一个元素，调用函数
def f(x):
    return x*x
L=[1,2,3,4]
map(f,L)    #[1,4,9,16]    把一个 list 转换为另一个 list，只需要传入转换函数。

#假设用户输入的英文名字不规范，没有按照首字母大写，后续字母小写的规则，
#请利用map()函数，把一个list（包含若干不规范的英文名字）
#变成一个包含规范英文名字的list：
def format_name(s):
    return s[0].upper()+s[1:].lower()

print map(format_name, ['adam', 'LISA', 'barT'])
```
```python
#reduce函数:
def f(x, y):
    return x + y
reduce(f, [1, 3, 5, 7, 9])        #相当于求和

#求积
def prod(x, y):
    return x*y
print reduce(prod, [2, 4, 5, 7, 12])
```
```python
#filter函数
def is_odd(x)
	return x%2==1
filter(is_odd,[1,2,3,4,5,6,7,8])   #[1,3,5,7] 过滤掉偶数

#利用filter()，删除 None 或者空字符串
def is_not_None_Null(str):
    return str and len(str.strip())>0
L=['Tom','',' ','Paul']
filter(is_not_None_Null,L)

print [1,4,5,2,3,7,9,8].sort()
```
```python
#sorted
#定义一个倒序排列的函数   
print(sorted([1,4,2,3],reverse=True))

def reversed_cmp(x, y):
    if x > y:
        return -1
    if x < y:
        return 1
    return 0

sorted([36, 5, 12, 9, 21], reversed_cmp) #实现倒叙排列
sorted(['bob', 'about', 'Zoo', 'Credit'])   #['Credit', 'Zoo', 'about', 'bob']

#实现字符串忽略大小写排序的算法。
def cmp_ignore_case(s1, s2):
    ss1=s1.upper()
    ss2=s2.upper()
    if ss1>ss2:
        return 1
    if ss1<ss2:
        return -1
    return 0
print sorted(['bob', 'about', 'Zoo', 'Credit'], cmp_ignore_case)


#请编写一个函数calc_prod(lst)，它接收一个list，返回一个函数，
#返回函数可以计算参数的乘积。
def calc_prod(lst):
    def lazy_prod():
        def f(x,y):
            return x*y
        return reduce(f,lst,1)
    return lazy_prod
f = calc_prod([1, 2, 3, 4])
print f()
```


#### 二: 函数
##### 1.函数的创建
```python
def pop():
    pass     #pass语句什么都不做,用来作为占位符

def enroll(name,gender):
    print(name,":,gender")
```

##### 2.默认参数
```python
#默认参数一定要用不可变对象，如果是可变对象，程序运行时会有逻辑错误！
def enroll(name, gender, age=6, city='Beijing'):
    print('name:', name)
    print('gender:', gender)
    print('age:', age)
    print('city:', city)
#调用
enroll('Bob', 'M', 7)
enroll('Adam', 'M', city='Tianjin')   #不按顺序调用时，指明赋值的字段名

#默认参数的一个大坑
Python函数在定义的时候，默认参数L的值就被计算出来了，即[]，因为默认参数L也是一个变量，它指向对象[]，每次调用该函数，
如果改变了L的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的[]了。

#所以，如果要避免这个坑，想要传入一个list为空时
def add_end(L=None):
    if(L is None):
        L=[]
    L.append("end")
    return L
```

##### 3.可变参数
```python
# 可变参数就是传入的参数个数是可变的，可以是1个、2个到任意个，还可以是0个。
#*args是可变参数，args接收的是一个tuple
def calc(*numbers):       #在参数前加一个*即可,内部转换为一个tuple
    sum=0
    for n in numbers:
        sum = sum + n
    return sum
#调用
calc()
calc(1,2)

#利用已有的list或者tuple调用可变参数,直接加上一个 * 即可
>>> nums = [1, 2, 3]
>>> calc(*nums)
```

##### 4.关键字参数
```python
#关键字参数允许你传入0个或任意个含参数名的参数,这些关键字参数在函数内部自动组装为一个dict
#**kw是关键字参数，kw接收的是一个dict。
def person(name, age, **kw):              # **kw 就是关键字参数,关键字参数其实就是可选项
    print('name:', name, 'age:', age, 'other:', kw)

#调用
person("chu",20)
person("chu",20,gender="nan")
person("chu",20,gender="nan",job="xuesheng")    #other: {'gender': 'nan', 'job': 'xuesheng'}

#命名关键字
def person(name, age, *, city, job):
    print(name, age, city, job)
#调用 必须传入 city job
person('Jack', 24, city='Beijing', job='Engineer')
``` 

##### 5.参数组合
```python
#在Python中定义函数，可以用必选参数、默认参数、可变参数、关键字参数和命名关键字参数，这5种参数都可以组合使用。
#但是请注意，参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。
```

##### 6.匿名函数
```python
匿名函数 lambda x: x * x 实际上就是:
def f(x)
	return x*x
print map(lambda x:x*x,[1,2,3,4,5]) #[1,4,9,16,25]

sorted([1,3,9,5,0],lambda x,y:cmp(x,y))
```

#### 三: 切片
##### 1.切 list tuple str
```python
L[0]
L[0:3]
L[:3]
L[-2:]       #从-2个元素到最后,即最后3个元素
L[:]         #完全复制
L[::2]       #每隔两个取一个
L[2::3]      #从索引2开始取3的倍数
L[4:50:5]    #从4到50取5的倍数，不包括50
L->L  T->T   #把list换成tuple，切片操作完全相同，只是切片的结果也变成了tuple

#字符串同理

#接受一个字符串，然后仅把首字母转换为大写
def firstCharToUpper(str):
    return str[0:1].upper()+str[1:]  #利用upper函数讲首字母大写
print(firstCharToUpper("chu"))
```
##### 2.列表生成器 range -> list
```python
list = [x for x in range(1,10)]

list2 = [x*x for x in range(10) if x%2==0 ]

for m in 'ABC'
	for n in '123'
		L.append(m+n)
```

#### 四: 生成器
一边循环一边计算的机制，称为生成器：generator。
1.创建 generator
```python
#第一种方法很简单，只要把一个列表生成式的[]改成()，就创建了一个generator：
g = (x * x for x in range(10)) 

#一个一个打印出来,没有更多的元素时，抛出StopIteration的错误,太麻烦
next(g)

#for循环迭代
for n in g:
    print(n)

#定义generator的另一种方法。如果一个函数定义中包含yield关键字，那么这个函数就不再是一个普通函数，而是一个generator：    
def fib2(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
```


#### 五: Iterable Iterator
```python
小结
凡是可作用于for循环的对象都是Iterable类型；

凡是可作用于next()函数的对象都是Iterator类型，它们表示一个惰性计算的序列；

集合数据类型如list、dict、str等是Iterable但不是Iterator，不过可以通过iter()函数获得一个Iterator对象。

Python的for循环本质上就是通过不断调用next()函数实现的，例如：

程序:
# -*- coding:utf-8 -*-
from collections import Iterable
from collections import Iterator
#Iterable对象：可以直接作用于for循环的对象统称为可迭代对象：Iterable，比如:list,tuple,dict,set,str，generator等

#判断一个对象是不是Iterable对象,使用isinstance()
print(isinstance([],Iterable))  #判断一个list是不是Iterable对象
print(isinstance({},Iterable))  #判断dict
#。。。

#而生成器不但可以作用于for循环，还可以被next()函数不断调用并返回下一个值，直到最后抛出StopIteration错误表示无法继续返回下一个值了
#Itetator对象:可以被next()函数调用并不断返回下一个值的对象称为迭代器：Iterator。

#判断一个对象是不是Iterator对象,仍然使用isinstance()
print(isinstance((x for x in range(5)),Iterator))

#生成器都是Iterator对象，但list、dict、str虽然是Iterable，却不是Iterator。

#把list、dict、str等Iterable变成Iterator可以使用iter()函数
print(isinstance(iter([]),Iterator))    #True

# 为什么list、dict、str等数据类型不是Iterator？
# 这是因为Python的Iterator对象表示的是一个数据流，Iterator对象可以被next()函数调用并不断返回下一个数据，
# 直到没有数据时抛出StopIteration错误。可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，
# 只能不断通过next()函数实现按需计算下一个数据，所以Iterator的计算是惰性的，只有在需要返回下一个数据时它才会计算。

# Iterator甚至可以表示一个无限大的数据流，例如全体自然数。而使用list是永远不可能存储全体自然数的

for x in [1,2,3,4,5]:
    pass

#首先获取到Iterator对象
iter=iter([1,2,3,4,5])
while True:
    try:
        x=iter.__next__()   #可以一直获取下一个元素
    except StopIteration as e:   #可以为StopIteration取一个别名
        break    #设置循环的终止条件
#上面的两个函数实际上是等同的

```

#### 六: 类
##### 1.定义类
```python
class Person(object):
    def __init__(self, name, gender):
        self.name = name
        self.gender = gender
```

##### 2.继承类
*   python中继承一个类,一定要用 super(Student, self).__init__(name, gender) 
  去初始化父类,否则，继承自 Person 的 Student 将没有 name 和 gender。
*   注意:self参数已在super()中传入，在__init__()中将隐式传递，不需要写出（也不能写）。
```python
#定义Student类时，只需要把额外的属性加上，例如score：
class student(Person):     #括号里的Person就是Student继承的父类
    def __init__(self, name, gender, score):
        super(Student, self).__init__(name, gender)  #在__init__()中 用super(Student,self).__init__(name,score) 初始化父类
        self.score = score
```

##### 2.类实例
```python
xiaoming=Person()

#实例变量赋值
xiaoming.name='Xiao Ming'
xiaoming.gender='Male'
xiaoming.birth='1990-1-1'

p1 = Person()
p1.name = "Paul"

#类排序
L1 = [p1, p2, p3]
L2 = sorted(L1,lambda p1,p2:cmp(p1.name,p2.name)) 
```

##### 3.类的访问权限
*   Python对属性权限的控制是通过属性名来实现的，如果一个属性由双下划线开头(__)，
  该属性就无法被外部访问。
*   如果一个属性以"__xxx__"的形式定义，那它又可以被外部访问了，
  以"__xxx__"定义的属性在Python的类中被称为特殊属性，
  有很多预定义的特殊属性可以使用，通常我们不要把普通属性用"__xxx__"定义。

##### 4.创建类属性
```python
class Person2(object):
	address='Earth'      #类属性
	def __init__(self,name)
		self.name=name
print Person2.address    #直接通过类名访问类属性
 
xiaowu=Person2('XiaoWu')
print xiaowu.address
print xiaowu.name

#由于Python是动态语言，类属性也是可以动态添加和修改的：
Person2.address2='Sun'
print xiaowu.address2

#给Person 类添加一个类属性 count，每创建一个实例，count 属性就加 1，
#这样就可以统计出一共创建了多少个 Person 的实例。
class Person(object):
    count=0
    def __init__(self,name):
        self.name=name
        Person.count=Person.count+1

p1 = Person('Bob')
print Person.count
p2 = Person('Alice')
print Person.count
p3 = Person('Tim')
print Person.count
```

##### 5.定义类方法
```python
class Person3(object)；
	def __init__(self,name):    #第一个方法永远是__init__,它的第一个参数是self
		self.__name=name
	def get_name(self):
		return self.__name
        
#调用实例方法必须在实例上调用：
p3=Person3('Bob')
print p3.get_name()    #self是不需要显示传入的
```