# functools --- 高阶函数和可调用对象上的操作
functools 模块应用于高阶函数，即——参数或（和）返回值为其他函数的函数。通常来说，此模块的功能适用于所有可调用对象。
- [partial](#partial)
- [partialmethod](#partialmethod)
- [@wraps](#wraps)
- [cmp_to_key](#cmp_to_key)
- [@lru_cache](#lru_cache)
- [@total_ordering](#total_ordering)
- [@singledispatch](#singledispatch)
- [reduce](#reduce)

# partial
functools.partial的作用就是，把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单。
```python
import functools
int2 = functools.partial(int, base=2)

print(int2)
# functools.partial(<class 'int'>, base=2)
print(int2('1100'))
# 12
```

# partialmethod
和 partial 差不多，只不过用在类的method上。  
会自动加self，并且可以不指明形参  
```python
from functools import partialmethod
class Cell(object):
     def __init__(self):
         self._alive = False
     @property
     def alive(self):
         return self._alive
     def set_state(self, state):
         self._alive = bool(state)
     set_alive = partialmethod(set_state, True)
     set_dead = partialmethod(set_state, False)

c = Cell()
print(c.alive) # False
c.set_alive()
print(c.alive) # True
```

# cmp_to_key
用于排序时的两个元素比较,代替cmp  
```python
from functools import cmp_to_key

def compare(ele1,ele2):
    if ele1 > ele2: return 1
    elif ele1 == ele2: return 0
    else: return -1

a = [2,3,1]
print(sorted(a, key = cmp_to_key(compare))) # [1, 2, 3]
```


# [wraps](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014318435599930270c0381a3b44db991cd6d858064ac0000 )
用作装饰器，相当于`partial(update_wrapper, wrapped = wrapped, assigned = assigned,updated = updated)`  
可以解决使用闭包时，原始函数的__name__等属性被替换的问题
```python
def log(text):
    def decorator(func):
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator

@log('execute')
def now():
    print('2015-3-25')

print(now)
# <function log.<locals>.decorator.<locals>.wrapper at 0x0000000003AA51E0>
```
利用 functool.warps 后
```python
import functools
def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator

@log('execute')
def now():
    print('2015-3-25')

print(now)
# <function now at 0x0000000003AA51E0>
```
# lru_cache
@functools.lru_cache(maxsize=128, typed=False)  
一个提供缓存功能的装饰器，包装一个函数，缓存其*maxsize*组传入参数，在下次以相同参数调用时直接返回上一次的结果。用以节约高开销或I/O函数的调用时间。  

由于使用了字典存储缓存，所以该函数的固定参数和关键字参数必须是可哈希的。

不同模式的参数可能被视为不同从而产生多个缓存项，例如, f(a=1, b=2) 和 f(b=2, a=1) 因其参数顺序不同，可能会被缓存两次。  

如果*maxsize*设置为``None``,LRU功能将被禁用且缓存数量无上限。 maxsize 设置为2的幂时可获得最佳性能。  

如果*typed*设置为true，不同类型的函数参数将被分别缓存。例如， f(3) 和 f(3.0) 将被视为不同而分别缓存。  

```python
import time
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)

tstart = time.time()
for n in range(30):
    fib(n)
print(time.time()-tstart) # 0.52900
```
利用缓存机制
```python
from functools import lru_cache
import time
@lru_cache(maxsize=None)
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)

tstart = time.time()
for n in range(30):
    fib(n)
print(time.time()-tstart) # 0.0
```

# total_ordering
这是一个类装饰器，给定一个类，这个类定义了一个或者多个比较排序方法，这个类装饰器将会补充其余的比较方法，减少了自己定义所有比较方法时的工作量；  

被修饰的类必须至少定义 `__lt__()`， `__le__()`，`__gt__()`，`__ge__()`中的一个，同时，被修饰的类还应该提供 `__eq__()`方法。  
```python
import functools
@functools.total_ordering
class Student:
    def _is_valid_operand(self, other):
        return (hasattr(other, "lastname") and
                hasattr(other, "firstname"))
    def __eq__(self, other):
        if not self._is_valid_operand(other):
            return NotImplemented
        return ((self.lastname.lower(), self.firstname.lower()) ==
                (other.lastname.lower(), other.firstname.lower()))
    def __lt__(self, other):
        if not self._is_valid_operand(other):
            return NotImplemented
        return ((self.lastname.lower(), self.firstname.lower()) <
                (other.lastname.lower(), other.firstname.lower()))
```

# [singledispatch](https://docs.python.org/zh-cn/3.7/library/functools.html#functools.singledispatch)
用于方便的编写一个能够处理不同类型输入参数的函数  



# reduce
返回一个值，相当于
```python
def reduce(function, iterable, initializer=None):
    it = iter(iterable)
    if initializer is None:
        value = next(it)
    else:
        value = initializer
    for element in it:
        value = function(value, element)
    return value
```



