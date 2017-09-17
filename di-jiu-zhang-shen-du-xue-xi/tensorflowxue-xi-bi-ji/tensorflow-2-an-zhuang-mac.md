### Tensorflow安装

首先打开Tensorflow官方下载合适的版本：[https://www.tensorflow.org/install/](https://www.tensorflow.org/install/)

官方可以使用四种方式安装：

1. virtualenv 
2. "native" pip 
3. Docker 
4. 使用源码安装

本文使用pip安装，需要准备好pip版本。另外请注意使用的是CPU还是GPU版本。GPU版本会快很多。

Tensorflow模板需要Python的版本是：

* Python 2.7
* Python 3.3+

我的Mac使用的是python 2.7版本。

另外需要关闭Mac的的System Integrity Protection的问题，解决的办法是关闭保护SIP，以及允许安装第三方的软件。

> 关闭SIP
>
> 1. 重启电脑，电脑启动的时候按住command+R；
> 2. 等画面上显示苹果logo的时候之后，你会看到顶部菜单「实用工具」，选择子菜单"终端"；
> 3. 然后终端就打开了，你直接输入 csrutil disable ，输完之后重启

首先检查pip的版本。

```
bogon:~ geguo$ pip -V  #注意是大写的V
pip 6.0.3 from /Library/Python/2.7/site-packages (python 2.7)
```

官网推荐使用高版本的pip，首先升级pip

```
sudo easy_install --upgrade pip
sudo easy_install --upgrade six

再次检查版本
pip -V
pip 9.0.1 from /Library/Python/2.7/site-packages/pip-9.0.1-py2.7.egg (python 2.7)
```

**安装 TensorFlow**

安装Tensorflow的过程非常简单。

```
pip install tensorflow      # Python 2.7; CPU support


Collecting tensorflow
  Downloading tensorflow-1.3.0-cp27-cp27m-macosx_10_11_x86_64.whl (39.4MB)
    100% |████████████████████████████████| 39.4MB 32kB/s 
Requirement already satisfied: six>=1.10.0 in /Library/Python/2.7/site-packages/six-1.10.0-py2.7.egg (from tensorflow)
Requirement already satisfied: protobuf>=3.3.0 in /Library/Python/2.7/site-packages (from tensorflow)
Requirement already satisfied: wheel in /Library/Python/2.7/site-packages (from tensorflow)
Requirement already satisfied: backports.weakref>=1.0rc1 in /Library/Python/2.7/site-packages (from tensorflow)
Collecting numpy>=1.11.0 (from tensorflow)
  Downloading numpy-1.13.1-cp27-cp27m-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl (4.6MB)
    100% |████████████████████████████████| 4.6MB 191kB/s 
Collecting tensorflow-tensorboard<0.2.0,>=0.1.0 (from tensorflow)
  Downloading tensorflow_tensorboard-0.1.6-py2-none-any.whl (2.2MB)
    100% |████████████████████████████████| 2.2MB 384kB/s 
Collecting mock>=2.0.0 (from tensorflow)
  Downloading mock-2.0.0-py2.py3-none-any.whl (56kB)
    100% |████████████████████████████████| 61kB 706kB/s 
Requirement already satisfied: setuptools in /System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python (from protobuf>=3.3.0->tensorflow)
Collecting werkzeug>=0.11.10 (from tensorflow-tensorboard<0.2.0,>=0.1.0->tensorflow)
  Downloading Werkzeug-0.12.2-py2.py3-none-any.whl (312kB)
    100% |████████████████████████████████| 317kB 460kB/s 
Collecting html5lib==0.9999999 (from tensorflow-tensorboard<0.2.0,>=0.1.0->tensorflow)
Collecting markdown>=2.6.8 (from tensorflow-tensorboard<0.2.0,>=0.1.0->tensorflow)
Collecting bleach==1.5.0 (from tensorflow-tensorboard<0.2.0,>=0.1.0->tensorflow)
  Downloading bleach-1.5.0-py2.py3-none-any.whl
Collecting funcsigs>=1; python_version < "3.3" (from mock>=2.0.0->tensorflow)
  Downloading funcsigs-1.0.2-py2.py3-none-any.whl
Collecting pbr>=0.11 (from mock>=2.0.0->tensorflow)
  Downloading pbr-3.1.1-py2.py3-none-any.whl (99kB)
    100% |████████████████████████████████| 102kB 343kB/s 
Installing collected packages: numpy, werkzeug, html5lib, markdown, bleach, tensorflow-tensorboard, funcsigs, pbr, mock, tensorflow
Successfully installed bleach-1.5.0 funcsigs-1.0.2 html5lib-0.9999999 markdown-2.6.9 mock-2.0.0 numpy-1.13.1 pbr-3.1.1 tensorflow-1.3.0 tensorflow-tensorboard-0.1.6 werkzeug-0.12.2
```

#### **验证安装**

安装完毕后，我们需要来验证是否安装无误。

```
$python

import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```

如果python输入以下内容，就说明安装正确了。

```
Hello, TensorFlow!
```

具体的安装文档请查看：

[https://www.tensorflow.org/install/install\_mac\#validate\_your\_installation](https://www.tensorflow.org/install/install_mac#validate_your_installation)

