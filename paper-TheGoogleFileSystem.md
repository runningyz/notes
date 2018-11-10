## The Google File System

### 1.Abstract and introduction

+ 当前和历史系统遇到的问题
+ 大量的文件 数量和size 管理笨重
+ 分布的服务和高频的访问场景
+ 宽松的一致性模型 减少负担
+ 引入原子添加操作 多客户端同时写入

### 2.Design Overview

#### 2.1 assumptions

+ 分布式系统组件廉价 所以必须包含监控 容错 自动恢复等功能
+ ?????小文件不优化?????
+ 负载来自:1.大量流式读取 2.小量随机读取
+ 针对负载来说,写的类型同样类似于读 小文件写只需提供支持 而不需要efficient
+ 原子操作保证:多个生产者对同一文件操作
+ 高吞吐量>低延迟

#### 2.2 interface

+ 不提供接口标准
+ GFS提供操作快照和record

#### 2.3 architecture

+ files切分成fixed size的chunks 每个chunk 64M
+ master维护元数据信息 namespace file-chunk-mapping
+ 每个chunk保存三份 容易保证可靠性
+ client通过master获取连接信息 通过与chunkserver直接通信获取数据
+ client和chunkserver都不缓存数据

#### 2.4 single master

+ client把文件名和offset翻译成一个chunk index发送给master
+ master响应一个 chunk handle和一个 replica的location给客户端
+ client把文件名和chunk index缓存为一个key 方便后面读取replica

#### 2.5 chunk size

+ size是64M
+ size太小会增加master的负担 需要client频繁与master通信
+ size大 可以通过保持TCP长连接 减轻网络负担
+ size大的缺点就是:增大了当前这个chunk变成hot spot的概率

#### 2.6 metadata

+ master存储了三种元信息:
  + file和chunk的namespace
  + file-chunk-mapping
  + 每个chunk-repllica的location (not persistently 每次master启动或者新的chunkserver加入 master都会询问一下这类信息)
+ 2.6.1 in-memory data structures

