![æ¨¡åå¾](https://images.gitbook.cn/f3c480e0-3be2-11e9-82f8-0f2e500ca934)

# Java基础

## JDK和JRE

- JDK：Java Development Kit 的简称，Java 开发工具包，提供了 Java 的开发环境和运行环境。
- JRE：Java Runtime Environment 的简称，Java 运行环境，为 Java 的运行提供了所需环境。

JDK包括JRE，当然还包括java源码编译器javac等其他java调试工具<br/>如果只是运行java程序，只需要安装JRE就行了，要开发java程序就需要安装JDK

## ==和equals()

### == 

- 对于基本类型是值比较
- 对于引用类型是引用比较

### equals()

equals()本质上就是==，但是String和Integer重写（重写事也必须重写hashCode方法）了equals()方法，使之变成了值比较

**所以**：equals()本质上就是==，只是对于少数引用类型重写了equals()方法使之变成了值比较，比如*String,Integer*

## hashCode

hashCode相等，equals()不一定相等，因为就算hashCode相等，不代表在散列表中的键值对相等

## final

- final 修饰的类叫最终类，该类不能被继承。
- final 修饰的方法不能被子类重写（被声明为final类的方法自动声明为final）
- final 修饰的变量（包括基本类型和实例变量）叫常量，常量必须初始化，不能被修改。

##Math.round()

Math.round(-1.5) = -1<br/>Math.round(1.5) = 2

Math.round()向坐标轴右取整

## String、StringBuffer和StringBuilder

