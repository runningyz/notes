### 信息：

举例：

A：巴西队进入世界杯

B：中国队进入世界杯

事件B的信息量更大,**概率越小的事件发生了，包含的信息量越大，意味着不确定性越大，熵就越大 **

### 熵

事件x有n种可能性，它的熵为：

![1542529134183](https://github.com/runningyz/notes/blob/master/pic/entropy/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20181118161832.png)

即p（x）越小，整体的绝对值越小，H（X）越大，即概率小-->熵大

### KL散度（相对熵）

![1542529280839](https://github.com/runningyz/notes/blob/master/pic/entropy/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20181118162106.png)

p是真实概率，q是预测概率，q是我们模拟真实分布做的预测，当q无限接近p的时候，KL散度无限趋近于0

### 交叉熵（cross-entropy）

把上面的KL散度拆开：

![1542529394553](https://github.com/runningyz/notes/blob/master/pic/entropy/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20181118162304.png)

前面正好是p的熵 ，由于p是真实分布，是固定的，所以在实际训练过程中只关注后面的部分，式子的后半部分，叫做交叉熵：

![1542529481527](https://github.com/runningyz/notes/blob/master/pic/entropy/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20181118162429.png)

即：交叉熵越小-->KL散度越小-->p和q的情况越接近-->预测模型越接近完美

所以经常用交叉熵作为loss