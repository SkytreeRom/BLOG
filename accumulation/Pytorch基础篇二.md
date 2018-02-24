# Pytorch基础篇二
## 定义网络
网络的定义的网络均需要通过`torch.nn.Module`实现，`Net`类继承`torch.nn.Module`类。用户定义的类包含`__init__()`和`forward()`  
`__init__()`函数中定义卷积层、全连接层等，`forward()`函数中定义relu、池化等操作
>`torch.nn`仅支持mini-batch,维度的含义是`nSamples x nChannels x Height x Width`  
对于仅有single sample的情况，则`input.unsqueeze(x)`可以在指定位置处添加一个假的batch维度

### 回顾
>
`torch.Tensor` - A multi-dimensional array.  
`autograd.Variable` - Wraps a Tensor and records the history of operations applied to it. Has the same API as a Tensor, with some additions like backward(). Also holds the gradient w.r.t. the tensor.  
`nn.Module` - Neural network module. Convenient way of encapsulating parameters, with helpers for moving them to GPU, exporting, loading, etc.  
`nn.Parameter` - A kind of Variable, that is automatically registered as a parameter when assigned as an attribute to a Module.  
`autograd.Function` - Implements forward and backward definitions of an autograd operation. Every Variable operation, creates at least a single Function node, that connects to functions that created a Variable and encodes its history.

### 快速定义网络
`torch.nn.Sequential()`实现，此时使用的均是`nn.Module`中的类，而不是像在`nn.functional`中的函数
## 损失函数和反向传播
欲得到损失先计算，先获得损失函数`criterion=nn.MSELoss()`,然后计算具体的损失`loss=criterion(output,target)`。  
反向传播过程中需要先将网络中参数的buffer置空`net.zero_grad()`,然后执行`output.backward()`进行反向传播。
## 更新权重
### 直接更新
```
learning_rate = 0.01
for f in net.parameters():
    f.data.sub_(f.grad.data * learning_rate)
```
### 使用`torch.optim`更新
```
import torch.optim as optim
optimizer = optim.SGD(net.parameters(), lr=0.01)
optimizer.zero_grad()
output = net(input)
loss = criterion(output, target)
loss.backward()
optimizer.step()
```

>注意此时是对optimizer进行`zero_grad()`

---  
`Feb 23th,2018`
