# 正则表达式

```python
import re

my_str = 'aa123bbaa456bb'
my_re = r'aa(.+?)bb'
result = re.findall(my_re, mystr)[0]
print(result)
```

结果：输出`['123','456']`

# 进制转换

## 转十进制

`int()`，第一个参数数字，第二个参数进制

```python
int('0xf',16)
int('101010',2)
int('16',8)
```

## 转十六进制

`hex()`，接收十进制的参数，所以先把其他进制转十进制再转十六进制

```python
hex(521)
hex(int('521',8))
hex(int('101010',2))
```

## 转二进制

`bin()`，先转十进制再转二进制

```python
bin(521)
bin(int('521',8))
bin(int('101010',2))
```

## 转八进制

`oct()`，可以将任意进制的数转8进制的

```python
oct(0b1010)
oct(521)
oct(0x521)
```

