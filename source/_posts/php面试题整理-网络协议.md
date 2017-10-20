---
title: php面试题整理-网络协议
date: 2017-10-08 16:47:33
tags:
    - php面试
---
### HTTP协议状态码 ###

#### 五类响应 ####

1XX 指示信息——请求已接收，继续处理；

2XX success——请求已被成功接收；

3XX redirection——要完成请求必须进行更进一步操作；<!-- more -->

4XX 客户端错误——服务器无法处理请求或者语法错误；

5XX 服务器错误——服务器未能实现合法的请求；

#### 常见状态代码 ####

200 OK：客户端请求成功

400 Bad Request：客户端请求有语法错误，不能被服务端所理解

401 Unauthorize：请求未经授权，这个拽太代码必须和 www-authenticate 报头域一起使用

403 Forbidden：服务器收到请求，但是拒绝提供服务

404 NotFound：请求资源不存在，eg. 输入了错误的URL

500 Internal Server Error：服务器发生不可预期的错误

503 Server Unavailable：服务器当前不能处理客户端的请求，一段时间后可能恢复正常

### OSI七层模型 ###

物理层：建立、维护、断开物理连接

数据链路层：建立逻辑连接，进行硬件地址寻址、差错校验等功能

网络层：进行逻辑地址寻址，实现不同网络之间的路径选择

传输层：定义传输数据的协议端口号，以及流控和差错校验。eg. TCP、UDP，数据包一旦离开网卡即进入网络传输层

会话层：建立、管理、终止会话

表示层：数据的表示、安全、压缩

应用层：网络服务与最终用户的一个接口。eg. HTTP、FTP、SMTP

### HTTP工作原理 ###

客户端发送请求给服务器，创建一个TCP连接，指定端口号，默认80，连接到服务器，服务器监听浏览器请求，一旦监听到客户端请求，分析请求类型，服务器会向客户端返回状态信息和数据内容。

#### get和post区别 ####

- 后退按钮get没有区别，post则会重新提交

- get可以收藏为书签，可以被浏览器缓存，参数会保留在历史记录当中，安全性较差

- get的content-type为application/x-www-form-urlencoded，而post文件传输时，form的enctype属性为multipart/form-data

- get有数据长度限制2048

- get只允许ASCii，而post也允许二进制数据

### P.S. content-type ###

在Form元素的语法中，`content-type`表明提交数据的格式，指定将数据回发到服务器时浏览器使用的编码类型。 

- `application/x-www-form-urlencoded`： 窗体数据被编码为名称/值对。这是标准的编码格式。 

- `multipart/form-data`： 窗体数据被编码为一条消息，页上的每个控件对应消息中的一个部分。 

- `text/plain`： 窗体数据以纯文本形式进行编码，其中不含任何控件或格式字符。

常用有两种：`application/x-www-form-urlencoded`和`multipart/form-data`，默认为`application/x-www-form-urlencoded`。 

 - 当 action 为 get 时候，浏览器用`x-www-form-urlencoded`的编码方式把 form 数据转换成一个字串（`name1=value1&name2=value2..`.），然后把这个字串append到url后面，用?分割，加载这个新的 url。 

 - 当 action 为 post 时候，浏览器把 form 数据封装到 http body 中，然后发送到 server。 如果没有`type=file`的控件，用默认的`application/x-www-form-urlencoded`就可以了。但是如果有`type=file`的话，就要用到`multipart/form-data`了。浏览器会把整个表单以控件为单位分割，并为每个部分加上 Content-Disposition （form-data或者file）, Content-Type（默认为text/plain）,name（控件name）等信息，并加上分割符（boundary）。