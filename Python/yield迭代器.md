## 先粗浅的了解一下yield
带有 ```yield``` 的函数不再是一个函数，而是一个**生成器**，其可用于进行迭代
* 生成器迭代原理：生成器有一个 ```next``` 方法，其原理就是重复调用 ```next``` 方法，直到捕获一个异常
* 生成器可以用于循环中，通常用在 ```for...in...``` 的in后面
* 使用生成器的优势：
	* 不会将所有数据取出来存入内存中；而是返回了一个对象；可以通过对象获取数据；用多少取多少，可以节省内容空间
	* 除了能返回一个值，还不会终止循环的运行
* yield：一个类似return的关键字，迭代一次遇到yield时就返回yield后面（右边）的值，并记住这个返回的位置，在下一次迭代时，就从上一次迭代遇到yield后面（下一行）的代码开始执行
* 生成器有两个方法：next和send
	* next：调用next方法不会从函数的最开始执行，其始于上一次next停止的地方，直到遇到yield返回要输出的结果为止
	* send：send方法开始的地方同next，其将一个参数传入到上一个next终止的地方（**即yield表达式**），**强行修改上一次yield返回的值**，之后**执行next方法**，直到再次遇到yield
* 在第一次调用的时候，必须先执行 ```next()``` 或者 ```send(None)```，否则会报错

yield的next方法
```python
def foo():
	print("Starting!")
	while True:
		res = yield 4
		print("res = ", res)

# 注意，在有了yield后，第一次调用foo()时，函数并不会真的执行（因此没有打印Starting），而是创造了一个生成器(generator)，print(g)时即可发现，g是一个generator对象
g = foo()
print(g)
>>> <generator object foo at 0x0000024A5087EC10>

# 当第一次执行print(next(g))时，首先调用next方法，此时foo函数正式开始执行，因此打印了“Starting”（即第一行结果），并进入到while循环
# 进入while循环后，程序遇到了yield，此处将yield当作是return，在yield返回了一个4后，程序停止，此时并未执行将4赋值给res的操作。而print(next(g))则将next(g)中yield返回的“4”打印出来（即第二行结果）。直到这里，语句执行完成
print(next(g))
>>> "Starting"
>>> 4

# print“*”号作为分隔符
print("*"*20)
>>> "********************"

# 再次执行print(next(g))，与第一次执行不同的是，此次程序开始的地方，是上一次调用foo()时终止步骤（即yield返回了一个4）下一步的操作，即res的赋值操作
# 但需要注意的是，此时本应赋值给res的4，在上一次调用next(g)时，已经return出去了，因此赋值操作等号右侧没有值，为None，因此打印“res: None”（即第一行结果）
# 打印结束后，继续执行while循环，再次遇到yield，yield返回4后，程序再次停止，print(next(g))则将next(g)中yield返回的“4”打印出来（即第二行结果）
print(next(g))
>>> "res: None"
>>> 4
```
yield的send方法
```python
g = foo()
print(next(g))
print("*"*20)

# send函数发送了一个参数给res
# send函数，接在上一次调用foo()时终止步骤（即yield返回了一个4）执行，此时send将7赋值给res，因次打印“res: 7”（即第一行结果），然后再执行next方法，继续执行while循环，再次遇到yield，yield返回4并打印（即第二行结果）
print(g.send(7))
>>> "res: 7"
>>> 4
```
补充：
* 通常yield都是放在一个函数中，该函数就变成生成器函数，即一个迭代器
* 生成器函数一般通过**for循环**调用，for循环自带next方法
```python
def foo(num):
	print("Starting")
	while num < 10:
		num = num + 1
		yield num

for i in foo(0):  # 将生成器作为迭代对象，传入生成器的数字用于限制迭代次数
	print(i)

# 生成斐波那契数列
def fib(num):
    prev, curr = 0, 1
    while num < 10:  # 限制迭代次数为10；如果设置为while True，取消num对生成次数的限制，则进行无限迭代
        yield curr
        curr, prev = prev + curr, curr
        num += 1

for i in fib(0):
	print(i)
```
