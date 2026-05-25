---
layout: post
title: 用Keras进行手写字体识别（MNIST数据集）
date: 2020-12-16 10:22:14
toc: true
categories: [学习记录]
tags: [深度学习, 神经网络, python]

comments: true
---

# 加载Keras中的MNIST数据集

```python
from keras.datasets import mnist

(train_images, train_labels), (test_images, test_labels) = mnist.load_data()
```

#### 查看数据

```python
>>> train_images.shape
(60000, 28, 28)
>>> len(train_labels)
60000
>>> train_labels
array([5, 0, 4, ..., 5, 6, 8], dtype=unit8)
```

<!--more-->

#### 测试数据

```python
>>> test_images.shape
(10000, 28, 28)
>>> len(test_labels)
10000
>>> test_labels
array([7, 2, 1 ... , 4, 5, 6], dtype=unit8)
```

# 网络架构（model）

```python
from keras import models
from keras import layers

network = models.Sequential()
network.add(layers.Dense(512, activation='relu', input_shape=(28 * 28, )))
network.add(layers.Dense(10, activation='softmax'))
```

Keras可以帮助我们实现一层一层的连接起来，在本例中的网络包含2个Dense层，他们是密集连接（也叫全连接）的神经层。第二层是一个10路softmax层，他将返回一个由10个概率值（总和为1）组成的数组。每个概率值表示当前数字图像属于10个数字类别中的某一个的概率。

# 编译

要想训练网络，我们还需要选择编译（compile)步骤的三个参数。

- **损失函数**：网络如何衡量在训练数据上的性能，即网络如何朝着正确的方向前进。
- **优化器（optimizer）**：基于训练数据和损失函数来更新网络的机制。
- **在训练和测试过程中需要监控的指标（metric）**：本例只关心精度，即正确分类的图像所占的比例。

```python
network.compile(optimizer='rmsprop', 
                loss='categorical_crossentropy', 
                metrics=['accuracy'])
```

# 准备图像数据

```python
train_images = train_images.reshape((60000, 28 * 28))		# 把一个图像变成一列数据用于学习
train_images = train_images.astype('float32') / 255		# astype用于进行数据类型转换

test_images = test_images.reshape((10000, 28 * 28))
test_images = test_images.astype('float32') / 255
```

# 准备标签

```python
from keras.utils import to_categorical

train_labels = to_categorical(train_labels)
test_labels = to_categorical(test_labels)
```

# 拟合（fit）模型
```pytohn
>>> network.fit(train_images, train_labels, epochs=5, batch_size=128)
Epoch 1/5
469/469 [==============================] - 2s 5ms/step - loss: 0.2535 - accuracy: 0.9263
Epoch 2/5
469/469 [==============================] - 2s 5ms/step - loss: 0.1032 - accuracy: 0.9698
Epoch 3/5
469/469 [==============================] - 2s 5ms/step - loss: 0.0682 - accuracy: 0.9799
Epoch 4/5
469/469 [==============================] - 2s 5ms/step - loss: 0.0492 - accuracy: 0.9849
Epoch 5/5
469/469 [==============================] - 2s 5ms/step - loss: 0.0367 - accuracy: 0.9894
```

#### 检查一下模型在测试集上的性能

```python
>>> test_loss, test_acc = network.evaluate(test_images, test_labels)
>>> print('test_loss:', test_loss)
test_loss: 0.07141019403934479				# 损失
>>> print('test_acc:', test_acc)
test_acc: 0.977400004863739				# 精度
```

# 查看某个具体图像的显示情况

```python
digit = train_images[1].reshape(28, 28)
import matplotlib.pyplot as plt

plt.imshow(digit, cmap = plt.cm.binary)
plt.show()
```

![](/assets/blogImg/MNIST.jpg) 