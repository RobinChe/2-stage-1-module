## 1. 总体设计
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200407022938336.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L215c3dlZXQxMTE=,size_16,color_FFFFFF,t_70)
### Tomcat的两个心脏
1. Connector用于处理连接相关的事情，并提供Socket与Request和Response相关的转化;
2. Container用于封装和管理Servlet，以及具体处理Request请求；

### Service 是面向外界的连接体
1. 主要结合Connector和Container，实现封装，向外提供功能。
2. 一个service只能有一个container，但可以有多个connector

### Server
管理整个tomcat生命周期

## 2. 请求流程
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200407023415811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L215c3dlZXQxMTE=,size_16,color_FFFFFF,t_70)
请求进入tomcat，经由Service交付到Connector，Connector将接收的请求封装为Request和Response，然后讲封装后的数据传递到Container处理，处理完成后返回Connector，通过Socket将结果返还给客户端。
### Connector
Connector最底层使用的是（Tcp/Ip）Socket来进行连接的，Request和Response是按照HTTP协议来封装的。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020040702354286.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L215c3dlZXQxMTE=,size_16,color_FFFFFF,t_70)
1. 通过不同类型的ProtocolHandler，Connector可以处理类型的连接请求，比如socket、nio等连接
2. 每个ProtocolHandler都包含了三个组件：Endpoint、Processor、Adapter
3. Endpoint实现TCP/IP协议，Processor实现HTTP协议的并将Endpoint接收到的Socket封装成Request，Adapter将Request适配大宋Container进行具体的处理
4. 在Endpoint中，其抽象实现AbstractEndpoint里包含Acceptor（监听请求）和AsyncTimeout（检查异步Request的超时）两个内部类和Handler（处理接收到的Socket，并调用Processor进行处理）接口。
### Container
管理Servlet，处理Request请求
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200407023751702.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L215c3dlZXQxMTE=,size_16,color_FFFFFF,t_70)
1. Engine可以管理多个主机，一个Service最多只能有一个Engine；
2. Host是虚拟主机，通过配置Host就可以添加多台主机；
3. Context就意味着一个WEB应用程序；
4. Wrapper是Tomcat对Servlet的一层封装；

