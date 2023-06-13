## try方法

try/except：主要是用于处理程序正常执行过程中出现的一些异常情况，如语法错、数据除零错误、从未定义的变量上取值等
try/finally：用于在无论是否发生异常情况，都需要执行一些代码的场合
```python
# try/except格式
try:
	Normal execution block
except A:
	Exception A handle
except B:
	Exception B handle
except:
	Other exception handle
else:
	if no exception, get here
finally:
	print("finnaly")
```
说明：
* 正常执行的程序在try下面的Normal execution block执行块中执行，在执行过程中如果发生了异常，则**中断当前在Normal execution block中的执行**，跳转到**对应的异常处理块**中开始执行
* 在出现异常后，将从**第一个except X开始查找**，如果**找到了对应的exception类型则进入其提供的exception handle中进行处理**，如果**没有找到则直接进入except块处进行处理**
* except是可选项，如果没有此项，则在未找到exception类型时将进行默认处理，即**终止应用程序并打印提示信息**
* 而**无论是否发生异常**，只要提供了finally语句，则以上try/except/else/finally代码块执行的**最后一步总是执行finally所对应的代码块**

注意：
* 在完整语句中try/except/else/finally所出现的顺序必须是**try-->except X-->except-->else-->finally**，即所有的except必须在else和finally之前，else（如果有的话）必须在finally之前，而except X必须在except之前。否则会出现语法错误
* else和finally都是可选，而非必须的，但是如果存在的话else必须在finally之前，finally（如果存在的话）必须在整个语句的最后位置
* else语句的存在必须**以except X或者except语句为前提**，如果在没有except语句的try block中使用else语句会引发语法错误。也就是说else不能与try/finally配合使用