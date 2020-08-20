# HTTP

## HTTP协议格式

## HTTP请求报文和响应报文
### HTTP请求报文
HTTP 请求分为三个部分：状态行、请求头、消息主体。HTTP 定义了与服务器交互的不同方法，常用的HTTP方法有：
- GET： 用于请求访问已经被URI（统一资源标识符）识别的资源，可以通过URL传参给服务器
- POST：用于传输信息给服务器，主要功能与GET方法类似，但一般推荐使用POST方式。
- PUT： 传输文件，报文主体中包含文件内容，保存到对应URI位置。
- HEAD： 获得报文首部，与GET方法类似，只是不返回报文主体，一般用于验证URI是否有效。
- DELETE：删除文件，与PUT方法相反，删除对应URI位置的文件。
### HTTP 响应报文
HTTP响应也由3个部分构成，分别是：状态行、响应头(Response Header)、响应正文，状态行由协议版本、数字形式的状态代码、及相应的状态描述，常见的状态码有如下几种：
- 200 OK 客户端请求成功
- 400 Bad Request 由于客户端请求有语法错误，不能被服务器所理解。
- 401 Unauthorized 请求未经授权。这个状态代码必须和WWW-Authenticate报头域一起使用
- 403 Forbidden 服务器收到请求，但是拒绝提供服务。服务器通常会在响应正文中给出不提供服务的原因
- 404 Not Found 请求的资源不存在，例如，输入了错误的URL

## HTTP基本特性

## HTTP持久连接（Keep-Alive模式）

## HTTP中Session与Cookie

## HTTP1.0、HTTP1.1和HTTP2.0

## HTTPS
