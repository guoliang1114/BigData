#### Tensorflow快速入门例子

本节我们使用一个小例子来展示一下tensorflow的工作方式。

```
#coding=utf-8
import tensorflow as tf
import numpy as np

#生成数据
x_data = np.random.rand(100).astype(np.float32)
y_data = x_data*0.1+0.3

#开始创建tf结构
weights = tf.Variable(tf.random_uniform([1],-1.0,1.0));
biases = tf.Variable(tf.zeros([1]))

y=weights*x_data+biases

loss = tf.reduce_mean(tf.square(y-y_data))

optimizer = tf.train.GradientDescentOptimizer(0.5)
train = optimizer.minimize(loss)

init = tf.initialize_all_variables()
#结束创建tf结构
sess = tf.Session()
sess.run(init);

for step in range(201):
    sess.run(train)
    if step%20 == 0:
        print(step,sess.run(weights),sess.run(biases))

```

解释:

一般python编码不支持中文，如果python文件中需要使用中文，修改先定义文件编码。因此在第一行我们使用coding来指定python文件的编码。

