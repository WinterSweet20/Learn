# 正则表达式

```python
import re

my_str = 'aa123bbaa456bb'
my_re = r'aa(.+?)bb'
result = re.findall(my_re, mystr)[0]
print(result)
```

结果：输出`['123','456']`

