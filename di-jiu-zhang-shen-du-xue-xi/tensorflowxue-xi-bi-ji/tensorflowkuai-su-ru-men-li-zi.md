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

在如上代码中，我们随机生成了x\_data,并且x\_data类型为float32。接着定义了 y\_data,

y\_data=f\(x\)=0.1\*x+0.3

本节我们需要使用数据训练出weight=0.1，biases=0.3。

首先定义 weight为-0.1到0.1的随机数。biases默认为0。

f\(x\)=weight\*x+biases

紧接着，定义损失函数loss=tf.reduce\_mean\(tf.square\(y-y\_data\)\)。

然后定义优化器，我们希望训练的结果是loss最够的小。

接着初始化tensorflow的变量，创建session。执行session。为了方便看到过程，我们定期打印出weights和biases的值。

运行的结果如下:

```
(0, array([-0.39889359], dtype=float32), array([ 0.78034401], dtype=float32))
(20, array([-0.07990838], dtype=float32), array([ 0.39556831], dtype=float32))
(40, array([ 0.0449638], dtype=float32), array([ 0.32923555], dtype=float32))
(60, array([ 0.08316375], dtype=float32), array([ 0.30894351], dtype=float32))
(80, array([ 0.09484958], dtype=float32), array([ 0.30273595], dtype=float32))
(100, array([ 0.09842442], dtype=float32), array([ 0.30083698], dtype=float32))
(120, array([ 0.099518], dtype=float32), array([ 0.30025604], dtype=float32))
(140, array([ 0.09985254], dtype=float32), array([ 0.30007833], dtype=float32))
(160, array([ 0.0999549], dtype=float32), array([ 0.30002397], dtype=float32))
(180, array([ 0.0999862], dtype=float32), array([ 0.30000734], dtype=float32))
(200, array([ 0.09999579], dtype=float32), array([ 0.30000225], dtype=float32))
```

会发现经过201步的执行，weight的值趋近于0.1，biases趋近于0.3。和我们之前的定义是一致的。



