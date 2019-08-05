# 注解

1. `@ResponseBody`表示此接口为纯数据接口，不带任何界面展示（由于所有的 Restful 接口都只是返回数据，所以我们可以直接在类级别上添加 `@ResponseBody` 注解。）
2. `@Controller`和`@Responsebody`一起使用的时候使用`@RestController`

# Json报文

一个login的例子：
```java
@RestController
@RequestMapping("/sys/user")
public class UserController {

    @RequestMapping("login")
    public Map<String, String> login() {
        Map<String, String> hashMap = new HashMap<>();
        hashMap.put("msg", "登录成功");
        return hashMap;
    }
}
```

# 接口规范

## 报文规范

一般来说，接口响应主要告诉使用方三项信息：状态码、描述、数据（其中数据不是必须的）

需要定义一个Result类封装响应报文：

```java
public class Result{
    private int code;
    private String msg;
    private Object data;
    
    public Result(ResultCode resultCode, Object data){
        this(resultCode);
        this.data = data
    }
    public Result(ResultCode resultCode){
        this.code = resultCode.getCode();
        this.msg = resultCode.getMsg()
    }
    
    ...
}
```
同时定义一个枚举类来维护我们的状态码：

```java
public enum ResultCode{
    SUCCESS(0, "请求成功"),
    WARN(-1, "网络异常，请稍后重试");
    
    private int code;
    private String msg;
    
    ReslutCode(int code, String msg){this.code = code;this.msg = msg;}
    public int getCode(){return code;}
    public String getMSg(){return msg;}
}
```

## 请求数据规范

loarding......

# 参数校验

loarding......

## 简单参数校验

loarding......

## 复杂参数校验

loarding......

# 其他

##  `@RequestParam`

1. `test(@RequestParam("userName") String name)`

2. `test(@RequestParam String name)`

3. `test(String name)`

第三种没有`name`参数不会报错

第二种会报错，可以添加`required=false`

第一种会报错，可以添加`required=false`，并且`name`和`userName`可以不一样

# SQL ERROR

## SQL Syntax

`You have an error in your SQL syntax`

SQL语句写错了，自己菜

==会提示在哪一行==，自己检查就是了

## myBatis绑定错误

`org.apache.ibatis.binding.BindingException: Invalid bound statement (not found)`

一般是自己的`Mapper`文件和`.xml`文件对不上，检查就好了

主要是==包名==，==namespace==，==函数名称==

# 常用函数