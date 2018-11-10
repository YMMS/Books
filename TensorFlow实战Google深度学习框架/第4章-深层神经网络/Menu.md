# 第4章 深层神经网络

## 4.1 深度学习与神经网络

### 4.1.1 线性模型的局限性

### 4.1.2 激活函数实现去线性化

### 4.1.3 多层网络解决异或运算

## 4.2 损失函数定义

### 4.2.1 经典损失函数

### 4.2.2 自定义损失函数

## 4.3 神经网络优化算法

## 4.4 神经网络进一步优化

### 4.4.1 学习率的设置

### 4.4.2 过拟合问题

### 4.4.3 滑动平均模型

提升模型在测试数据上鲁棒性. `TensorFlow`中提供了`tf.train.ExponentialMovingAverage`来实现滑动平均模型.

```
Class ExponentialMovingAverage
```

#### Function:

```
__init__(decay, num_updates=None, zero_debias=False, name='ExponentialMovingAverage')
```
+ decay: 衰减率,衰减因子越大，权重越接近最新的值($\lambda$)

+ num_updates: 如果设置了该参数，则$\lambda=\min{\{\lambda, \frac{1+num\_updates}{10+num\_updates}\}}$

+ zero_debias:

+ name: 操作名称

$\theta_{mean}=\lambda \theta_{mean} + (1-\lambda)\theta$

```
v1 = tf.Variable(0, dtype=tf.float32)
step = tf.Variable(0, trainable=False)
ema = tf.train.ExponentialMovingAverage(0.99, step)
maintain_averages_op = ema.apply([])
```

