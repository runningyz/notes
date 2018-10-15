### AOP

#### 概括

+ 每个bean都被cglib或者JDK代理
+ 每个bean可以有多个拦截器,拦截器分为:
  + 内层:用户设置的 即AOP,其设计原型就是职责链
  + 外层:由spring内核控制流程
+ 代理方法被调用时,先经由外层拦截器,决定调用哪个内层拦截器

### AOP方法调用流程

![AOP流程图](https://user-gold-cdn.xitu.io/2018/10/12/16665e02ae84637a?imageslim)

