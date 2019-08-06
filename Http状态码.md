# 500

服务器端响应错误

造成原因：

1. 自己太菜，后端逻辑错误

#404

网页或文件未找到

原因：

1. 根据链接找不到你的页面或者服务

# 400

`Bad Request`，错误的请求

原因：

1. 链接不合法，如缺少必要的请求参数
2. 链接语义有错

# 403

拒绝访问，服务器能理解你的请求，但是不响应

原因：

1. 权限不够



# 415

`Unsupported Media Type`服务器无法处理请求附带的媒体格式

原因：和HTTP请求中的`Content-Type`有关，当没有写`Content-Type`或者`Content-Type`和服务器端期待的类型不同时，服务器端会返回415错误

![1565076764120](C:\Users\13997\AppData\Roaming\Typora\typora-user-images\1565076764120.png)

