# python基础
## python代码的运行
命令行模式下
```bash
# 第一种，调用python解释器
$ python file_path/python_code_file.py

# 第二种，直接输入文件
$ file_path/python_code_file.py
```
第二种方法需在python代码文件中指定解释器，`#!/usr/bin/env python`，且执行前需给予.py文件执行权限`chmod 755 python_code_file.py`

## 编译 解释 动态 静态 强类型 弱类型
- 编译型语言 vs 解释型语言  
解释型通常也称为脚本语言  

- 动态类型语言 vs 静态类型语言
    - 动态  
    语言执行期间做数据类型检查，变量定义时无需指定数据类型，比如`python`，`ruby`
    - 静态  
    数据类型在编译时检查，变量定义时需指定数据类型，比如`C`，`C++`，`Java`

- 强类型定义语言 vs 弱类型定义语言
    - 强类型  
    强制数据类型定义，需通过强制转换才能改变数据类型，比如`python`，`Java`
    - 弱类型  
    一个变量可以赋不同类型的值，比如`vb`，`php`

## 格式化输出
[PyFormat](https://pyformat.info)


## 文件操作
- 打开文件
```python
f1 = open(file_path, encoding='utf8', mode='r')
```
open方法在底层调用了操作系统的接口，使得能够操作文件。  
`f1`是`文件句柄`，对文件的操作都是通过文件句柄完成的。
`encoding`默认跟随操作系统，windows是`gbk`，linux和mac是`utf8`  
- 文件句柄的方法
    - read
    - write
    - tell，返回光标的位置，以字节为单位
    - seek，查找光标的位置，可用于文件的断电续传
    - flush，强制刷新
- 文件内容修改的实现逻辑
    1.以读的模式打开原文件
    2.以写的模式创建一个新文件
    3.将原文件的内容读取出来，修改成新的内容，写入新文件
    4.在内存中将原文件删除
    5.将新文件重命名为原文件



## 数据在内存中的存储
### 数据结构在内存中的id
- id 能获取数据结构在内存中的地址
- is 判断变量的内存地址是否相同
- =  判断两个变量的值是否相等
```python
name = 'ryan'
print(id(name))

f_name = 'ryan'
l_name = 'oligen'
print(f_name is l_name)

a = [1, 2, 3]
b = [1, 2, 3]
print(a is b)
print(a == b)
```

### 同一代码块 vs 非同一代码块
- 同一个`.py`文件中的代码属于同一代码块
- 命令行模式下，每一个`>>>`提示符后的每一行代码都是不同的代码块

- 同一代码块下变量在内存中的存储机制
    1.在内存中创建字典，存储`k-v`键值对，比如`i1 = 1`，会以`dic = {i1: 1}`的形式存储，i1指向一个内存地址x038374028，该地址存储着值1
    2.新建一个变量`i2 = 1`，在字典dic中寻找是否存在值1
    3.若存在，则i2也指向同一个内存地址，若不存在，则指向不同的地址
```python
i1 = 1
i2 = 1
print(f'id(i1): {id(i1)},  id(i2): {id(i2)}')
```







