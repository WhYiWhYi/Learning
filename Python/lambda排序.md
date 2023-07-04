# lambda排序
列表（使用列表的sort.()方法）
```python
lsa = [[5, 8], [5, 3], [3, 1]]

# 对单个变量排序
lsa.sort(key=lambda x: x[1])
>>> [[3,1], [5, 3], [5, 8]]

# 对多个变量升序/降序排列
lsa.sort(key=lambda x: (x[1], x[0]))  # 先考虑x[1]，后考虑x[0]，升序
>>> [[3,1], [5, 3], [5, 8]]
lsa.sort(key=lambda x: (x[1], x[0]), reverse=True)  # 先考虑x[1]，后考虑x[0]，降序
>>> [[5, 8], [5, 3], [3, 1]]

# 对多个变量分别升序/降序排列
lsa.sort(key=lambda x: (x[1], -x[0]))  # 先按照x[1]升序，再按照x[0]降序
>>> [[3, 1], [5, 3], [5, 8]]
```
字典
```python
dict = {"a":5, "b":4, "c":3, "d":2, "e":1}

# 按key排序
dict_s = sorted(dict.items(), key=lambda item:item[0])  # 升序
>>> [('a', 5), ('b', 4), ('c', 3), ('d', 2), ('e', 1)]

# 按value排序
dict_s = sorted(dict.items(), key=lambda item:item[1])  # 升序
>>> [('e', 1), ('d', 2), ('c', 3), ('b', 4), ('a', 5)]
```
列表&字典嵌套
```python
# 字典中嵌套字典，以内部字典某个key对应的值进行排序
dict = {'a': {'x': 3, 'y': 2, 'z': 1},
        'b': {'x': 2, 'y': 1, 'z': 3},
        'c': {'x': 1, 'y': 3, 'z': 2}}

dict_s = sorted(dict, key=lambda x: dict[x]["x"])  # 式子中的dict为字典的变量名
>>> ["c", "a", "b"]

# 列表中嵌套字典，使用列表的sort方法，按age升序
students = [{"name": "TOM", "age":20},
			{"name": "ROSE", "age":19},
			{"name": "JACK", "age":22}]

students.sort(key=lambda x: x["age"])
>>> [{'name': 'ROSE', 'age': 19},
	 {'name': 'TOM', 'age': 20},
	 {'name': 'JACK', 'age': 22}]

# 字典中嵌套列表，以列表中的某个索引处的值进行排序
dict = {'a': [1, 2, 3],
        'b': [2, 1, 3],
        'c': [3, 1, 2]}
dict_s = sorted(dict, key=lambda x: dict[x][1])  # 以列表中的第二项排序
>>> ["b", "c", "a"]
```