# Pytorch基础篇一
## Tensor用法与转换
### Tensor的创建
```
x=torch.Tensor(5,3)
y=torch.rand(5,3)
z=torch.randn(5,3)
```
### Tensor的运算
```=
print(x+y)
torch.add(x,y)
```
>Tensor的inplace运算`x.add_(val)`,后缀`_`代表inplace

### Tensor和numpy的ndarray转换
```
n1=np.ones([5,3])
n2=torch.from_numpy(n1)
n3=n2.numpy()
```
>ndarray和tensor是共享内存，修改一个另外一个也会改变

### Tensor转换为CUDA Tensor的方式
```
t=[torch.randn(5,5).cuda() if torch.cuda.is_available() else torch.randn(5,5)]
```
# Variable and Gradients
## Variable
Variable是Pytorch中最核心的类，Variable中包含了data和grad等
## Gradient
若想计算Gradient，只需对最终的Variable执行`backward()`函数
