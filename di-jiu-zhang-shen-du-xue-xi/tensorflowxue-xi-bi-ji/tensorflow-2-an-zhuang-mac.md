### Tensorflow安装

首先打开Tensorflow官方下载合适的版本：[https://www.tensorflow.org/install/](https://www.tensorflow.org/install/)

Tensorflow默认使用pip安装，需要准备好pip版本。另外请注意使用的是CPU还是GPU版本。GPU版本会快很多。

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

