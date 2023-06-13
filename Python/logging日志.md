# Logging日志模块

logging和print的差异
日志：给程序员看的，请将日志信息用于类似的目的（如字典值的内容）
print：给用户看的，如“文件未找到”、“无效输入，请输入一个数字”
二者是有所差异的，并非相互替代，而是各行其功能的

1. 导入logging模块
```python
import logging
```

2. 设定打印日志信息的格式
```python
logging.basicConfig(level=logging.DEBUG, format=" %(asctime)s - %(levelname)s - %(message)s")
```
* level：日志级别的关键词参数
	* 日志级别
		* DEBUG：最低级别。用于小细节。通常在诊断问题时才会关心这些信息
		* INFO：用于记录程序中一般事件的信息，或确认一切工作正常
		* WARNING：用于表示可能的问题，它不会阻止程序的工作，但将来可能会
		* ERROR：用于记录错误，它导致程序做某事失败
		* CRITICAL：最高级别，用于表示致命的错误，它导致或将要导致程序完全停止工作
	* 在logging.basicConfig()中设置的级别决定显示日志信息的内容。如果传入“logging.DEBUG”作为参数，将显示所有日志级别的消息。日志消息将显示传入参数及其以上级别的内容
* format：日志的格式
	* %(asctime)s：日志的记录时间
	* %(levelname)s：日志的级别
	* %(message)s：需要打印的消息

3. 打印日志信息
```python
logging.debug()  # 方法内为字符串，调用这个方法相当于调用print
```
* 打印debug级别的信息，就使用logging.debug()，打印其他级别的信息需进行相应的更换：
	* logging.info()
	* logging.warning()
	* logging.error()
	* logging.critical()
* 打印的信息为字符串。该字符串会被传入logging.basicConfig()中的**“%(message)s”**中
* 可以在程序开始和结束时打印 ```Start of program``` 和 ```End of program``` 作为日志的开始和结束

4. 禁止日志调用
```python
logging.disable()  # 方法内需传入一个日志级别
```
* 将禁用与传入的日志级别相同或更低的的日志信息。如果传入“logging.DEBUG”，则DEBUG的信息会被禁用，传入“logging.INFO”，则DEBUG和INFO的信息会被禁用
* 由于logging.disable()会禁用它之后的所有消息，所以可将其添加到程序的最前方，这样既容易找到它，可依据需要注释或取消注释从而禁用或启用日志信息

5. 将日志记录到文件
```python
logging.basicConfig(filename="LogFile.log", encoding="utf-8", level=logging.DEBUG, format=" %(asctime)s - %(levelname)s - %(message)s")
```
* filename：传入保存文件的完整路径即可保存文件
* encoding：传入文件的编码类型，通常为utf-8