# builtins.py 中的函数和类
链接：http://www.runoob.com/python3/python3-built-in-functions.html  

- [all()](#alliterable)
- [any()](#anyiterable)
- [bin()](#binint)
- [ord() 和 chr()](#ord和chr)
- [eval(), exec(), complie()](#eval和exec)
- [setattr(), getattr(), hasattr(), delattr()](http://www.runoob.com/python/python-func-setattr.html)
- [@property](http://www.runoob.com/python/python-func-property.html)
- [locals(), globals(), vars()](http://www.runoob.com/python/python-func-locals.html)
- [map(), filter()]()
- [hash()](http://www.runoob.com/python/python-func-hash.html)
- [format()](http://www.runoob.com/python/att-string-format.html)
- [classmethod()](#classmethod)
- [staticmethod()](#staticmethod)



# all(iterable)
相当于
```python
def all(iterable):
    for element in iterable:
        if not element:
            return False
    return True
```
# any(iterable)  
相当于
```python
def any(iterable):
    for element in iterable:
        if element:
            return True
    return False
```
# bin(int)
```python
def int2bstr(n):
    return bin(n&((1<<32)-1))[2:].zfill(32)
def bstr2int(s):
    return int(s[1:], 2) - int(s[0])*(1<<31)
def unsigned2signed(n):
    MASK = (1 << 32) - 1
    MAX = (1 << 31) - 1
    return n if n<=MAX else ~(n^MASK)
```
# ord() 和 chr()
```python
print(ord('傻')) # 20667
print(chr(20667)) # 傻
```

# eval() 和 exec()
```python
str = "for i in range(0,10): print(i, end=' ')"
c = compile(str,'123','exec')
exec(c)
print()
print(eval('str'))
```
# classmethod()
classmethod是类方法，@classmethod 修饰符对应的函数不需要实例化，不需要 self 参数，但第一个参数需要是表示自身类的 cls 参数，可以来调用类的属性，类的方法，实例化对象等。  
http://www.runoob.com/python/python-func-classmethod.html   

# staticmethod()
staticmethod是函数静态方法,和classmethod区别在于没有绑定类自身
```python
class A(object):
    def foo(self, x):
        print("executing foo(%s,%s)" % (self, x))
        print('self:', self)
    @classmethod
    def class_foo(cls, x):
        print("executing class_foo(%s,%s)" % (cls, x))
        print('cls:', cls)
    @staticmethod
    def static_foo(x):
        print("executing static_foo(%s)" % x)  
```

