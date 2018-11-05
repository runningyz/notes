## RPC note

##### headline:

+ rpc是什么 解决什么问题
+ rpc出现的场景 和一些常见软件
+ rpc交互过程和协议
+ 经典和常用的服务器模型
+ RPC的分布式

### RPC是什么

1.rpc远程过程调用 出自1984年nelson和birrell的一篇论文 Implementing Remote Procedure Calls 论文链接:

http://delivery.acm.org/10.1145/360000/357392/p39-birrell.pdf?ip=219.143.205.144&id=357392&acc=ACTIVE%20SERVICE&key=BF85BBA5741FDC6E%2EC98212438850C910%2E4D4702B0C3E38B35%2E4D4702B0C3E38B35&__acm__=1541400797_ed109bcbc5aa0d451135e8edf1a13aa5

2.提出的背景:

物理分离的子系统发生交互时,常见的方案有:

+ RPC
+ 分布式消息队列
+ HTTP请求调用
+ 数据库
+ 分布式缓存

3.一些常见的rpc中间件:

+ dubbo(alibaba)
+ dubboX(当当)
+ Motan(新浪)
+ Thrift(Facebook)
+ gRpc(Google)

### RPC出现的场景 和常见的软件

+ 在web领域常见的代理Nginx

  ![img](https://user-gold-cdn.xitu.io/2018/5/31/163b41f9801d8800?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

+ Hadoop平台节点之间的通信 使用hadoop rpc

![img](https://user-gold-cdn.xitu.io/2018/5/31/163b45dc4e72d505?imageslim)

+ Tensorflow Cluster的RPC通讯框架,google的gRPC

![img](https://user-gold-cdn.xitu.io/2018/6/7/163d86e168946589?imageslim)



### RPC交互过程和协议

1.一个简单的CS结构的 信息交互系统

![img](https://user-gold-cdn.xitu.io/2018/6/5/163cf789da84cb53?imageslim)

2.更详细的交互图

![img](https://user-gold-cdn.xitu.io/2018/5/31/163b4dcf06e0a780?imageslim)

![img](https://user-gold-cdn.xitu.io/2018/10/17/166802bd272e86a2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

3.RPC协议涉及的四个部分

+ 消息边界(长度前缀法 特殊符号分割法)

  + 特殊符号分割法 一般用/r/n 但是在传输二进制消息时 特别容易出现误分割的情况 需要先转码再传输
  + 长度前缀法 可读性比较差 但适合传输二进制消息 HTTP名字的由来,就在于它结合了这两种方式,在HTTP消息的头部采用了/r/n分割的方法,消息体的部分,使用了长度前缀法,可以传输音视频图片等,所以称为超文本传输协议

+ 消息表示(二进制 文本) 同上

+ 消息结构(有模式 无模式)

  + 有模式:如json消息 可以通过key-value的形式看出内容和意义

  ```json
     [
           {
             "type": "home",
             "number": "212 555-1234"
           },
           {
             "type": "fax",
             "number": "646 555-4567"
           }
       ]
  
  ```

  ​	b.无模式:json消息可读性强,但是冗余很多,比如同一个key,对应很多value,这个key会变得冗余,一般解决方案是在代码中,在交互前,规定消息体具体内容的顺序

  ```java
  @Override
  	public void writeImpl() {
  		writeByte((byte) this.platformId);
  		writeLong(deviceId);
  		writeStr(productId);
  		writeStr(channelId);
  		writeStr(versionId);
  		writeStr(phoneModel);
  	}...
  
  @Override
  	public void readImpl() {
  		this.platformId = readByte();
  		this.deviceId = readLong();
  		this.productId = readStr();
  		this.channelId = readStr();
  		this.versionId = readStr();
  		this.phoneModel = readStr();
  	}...
  ```

+ 压缩算法(CPU压力和网络吞吐量的平衡)

+ 消息协议中对流量优化的一个小方法:

  以传输一个整数为例,有两种措施:

  1.变长编码:每个字节的最高位,用来表示后续还有没有字节属于当前这个数

  2.zigzag编码 解决负数占用字节多的问题 :

  负数直接存为对应的正数,正数直接乘2

### 经典的和常用的单机服务器模型

+ 单线程同步模型
+ 多线程同步模型
+ preforking模型(线程池) 保护服务器
+ 单线程异步
+ 多线程异步

以上的模型都比较好理解,重点解释一下异步的实现

同步IO类似于你去银行柜台办理业务,当排到你的时候,你就在柜台前等,提交完材料,验证完身份,就一直坐在那里等待结果,加入是一个耗时业务的话,很浪费你的时间.而异步IO是去吃鲜芋仙,你去点餐相当于向服务器发起请求,一般的异步IO方法会有一个对象来负责通知发起方处理结果,而此时商家发给你的那个会闪灯的牌子和电脑里的订单共同构成了这个对象,你再等待的时候,可以继续出去逛逛或者干别的事,这就是一个简单的异步IO的例子.下图是一个多进程preforking的异步模型:

![img](https://user-gold-cdn.xitu.io/2018/5/11/1634e13697d3b055?imageslim)



这其中的绿色节点select就有点像鲜芋仙的闪灯的牌子.这个地方是操作系统提供的函数,传统的又select和poll方法,改进的有epoll和kqueue,

