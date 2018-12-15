# 了解bytes与str的区别
python3有两种表示字符序列的类型：bytes和str。前者的实例包含原始的8位值；后者的实例包含Unicode字符。
python2也有两种表示字符序列的类型：str和unicode。与python3不同的是，str实例包含原始的8位值；而unicode的实例，则包含Unicode字符。
把unicode字符表示成二进制数据（也就是原始的8位值）有许多种方法，最常见的还是utf-8编码。要想把unicode字符转换成二进制数据，就必须使用encode方法。要想想把二进制数据转化为unicode字符，则必须使用decode方法。
编写Python程序的时候，一定要把编码和解码操作放在最外围来做。程序的核心部分应该使用unicode字符类型，而且不要对字符编码做任何假设。这种方法即可以令程序接受多种类型的文本编码（如Latin-1,Shift JIS和Big5)，又可以保证输出的文本信息只采用一种编码形式。
```Python
在Python3中，接受str或bytes，并总是返回str的方法：
def to_str(bytes_or_str):
	if isinstance(bytes_or_str, bytes):
    	value = bytes_or_str.decode('utf-8')
    else: 
    	value = bytes_or_str
    return value
```
```Python
接受str或byte，并总是返回bytes的方法：
def to_bytes(bytes_or_str):
	if isinstance(bytes_to_str, str):
    	value = bytes_to_str.encode('utf-8')
    else:
    	value = bytes_to_str
    return value
```

注意：在Python3中，如果通过内置的open函数获取文件的句柄，该句柄默认会采用UTF-8编码格式来操作文件。而python2中，文件操作的默认编码格式则是二进制形式。