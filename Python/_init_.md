```python
class student(self, name, score):
	self.name = name
	self.score = score

stu = student()
```
通过class定义的类，就像一个模具，可以定义作为一学生应该具有的特点：名字、分数
而在类名 ```student``` 后加圆括号 ```()``` 组成一个函数常用形式，则执行了一个实例化的过程，即依据类定义好的规则，创建一个包含具体数据的学生对象（实例）

在定义类的时候，若是添加"\_init\_"方法，那么在创建类的实例的时候，实例就会自动调用这个方法。一般用于对实例的属性进行初始化
举个栗子

```python
class testClass:
	def _init_(self, name, gender):
		self.Name = name
		self.Gender = gender
		print("hello!")

testman = testClass("neo", "male")
>>>hello!
testman.Name
>>>"neo"
testman.Gender
>>>"male"
```
* def \_init\_(self, name, gender)：定义 \_init\_ 方法
	* self：创建类的实例时，被创建的类本身（即后续的testmen）；self可由其他字母替代
	* self.name = name：等号左边的Name是实例的属性，右边则是 \_init\_ 的参数
	* self.gender = gender：等号左边的Gender是实例的属性，右边则是 \_init\_ 的参数
	* print("hello!")：用于说明在创建类的实例时，\_init\_ 方法立即被调用了
* testman = testClass("neo", "male")：创建了类testClass的一个实例testman
	* 此处通过调用类testClass创建了类的实例testman，类中有\_init\_ 方法，在创建类的实例时，就需要有与之匹配的参数，即self, name, gender
	* self指的是实例本身，故不用传入，因此向实例中传入“neo”和“male”两个参数赋予至实例的属性Name和Gender