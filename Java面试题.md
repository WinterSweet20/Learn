![æ¨¡åå¾](https://images.gitbook.cn/f3c480e0-3be2-11e9-82f8-0f2e500ca934)

# Java基础

## JDK和JRE

- `JDK`：`Java Development Kit` 的简称，Java 开发工具包，提供了 Java 的开发环境和运行环境。
- `JRE`：`Java Runtime Environment `的简称，Java 运行环境，为 Java 的运行提供了所需环境。

JDK包括JRE，当然还包括java源码编译器javac等其他java调试工具<br/>如果只是运行java程序，只需要安装JRE就行了，要开发java程序就需要安装JDK

## ==和equals()

### == 

- 对于基本类型是值比较
- 对于引用类型是引用比较

### equals()

`equals()`本质上就是`==`，但是`String`和`Integer`重写（重写事也必须重写`hashCode`方法）了`equals()`方法，使之变成了值比较

**所以**：`equals()`本质上就是`==`，只是对于少数引用类型重写了`equals()`方法使之变成了值比较，比如*String,Integer*

## hashCode

`hashCode`相等，`equals()`不一定相等，因为就算`hashCode`相等，不代表在散列表中的键值对相等

## final

- final 修饰的类叫最终类，该类不能被继承。
- final 修饰的方法不能被子类重写（被声明为final类的方法自动声明为final）
- final 修饰的变量（包括基本类型和实例变量）叫常量，常量必须初始化，不能被修改。

##Math.round()

`Math.round(-1.5) = -1`<br/>`Math.round(1.5) = 2`

`Math.round()`向坐标轴右取整

## String、StringBuffer和StringBuilder

`String`和其他的区别，`String`是不可修改的，修改都是产生新的字符串对象，然后修改指针指向它

`StringBuffer`和`StringBuilder`最大的区别是`StringBuffer`是线程安全的，`StringBuilder`的性能比`StringBuffer`高

### new String

`String str = “Hello”`和`String str = new String("Hello")`不一样<br/>前一个是把它分配到常量池，后一个则是堆内存中

### 字符串反转

```java
StringBuilder stringBuilder = new StringBuilder;
stringBuilder.append("hello");
System.out.println(stringBuilder.reverse());//olleh

//StringBuffer同
```

### String常用方法

| 方法 | 效果 |
| ---- | ---- |
|  indexOf()|返回指定字符的索引|
|charAt()|返回指定索引处的字符|
|replace()|字符串替换|
|trim()|去除字符串两端空白|
|split()|分割字符串，返回一个分割后的字符串数组|
|getBytes()|返回字符串的 byte 类型数组|
|length()|返回字符串长度|
|toLowerCase()|将字符串转成小写字母|
| toUpperCase()|将字符串转成大写字符|
|substring()|截取字符串|
|equals()|字符串比较。|

## 抽象类

- 抽象类不一定要有抽象方法
- 普通类和抽象类的区别：
  - 普通类不可以包含抽象方法
  - 普通类可以直接实例化
- 抽象类不能用`final`修饰
  - 抽象类就是让其他类继承，`final`不能被继承
- 接口和抽象类的区别：
  - 实现不同：抽象类用继承`extends`，接口用实现`impements`
  - 抽象类可以有构造函数，接口不能有
  - 类可以实现多个接口，只能继承一个抽象类
  - 接口中的方法默认public，抽象类的可以是任意修饰符

## IO





