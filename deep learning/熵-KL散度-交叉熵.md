### 信息：

举例：

A：巴西队进入世界杯

B：中国队进入世界杯

事件B的信息量更大,**概率越小的事件发生了，包含的信息量越大，意味着不确定性越大，熵就越大 **

### 熵

事件x有n种可能性，它的熵为：

![1542529134183](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1542529134183.png)

即p（x）越小，整体的绝对值越小，H（X）越大，即概率小-->熵大

### KL散度（相对熵）

![1542529280839](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1542529280839.png)

p是真实概率，q是预测概率，q是我们模拟真实分布做的预测，当q无限接近p的时候，KL散度无限趋近于0

### 交叉熵（cross-entropy）

把上面的KL散度拆开：

![1542529394553](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1542529394553.png)

前面正好是p的熵 ，由于p是真实分布，是固定的，所以在实际训练过程中只关注后面的部分，式子的后半部分，叫做交叉熵：

![1542529481527](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1542529481527.png)

即：交叉熵越小-->KL散度越小-->p和q的情况越接近-->预测模型越接近完美

所以经常用交叉熵作为loss