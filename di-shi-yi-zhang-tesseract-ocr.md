# 第十一章 Tesseract OCR

**OCR**\(Optical Character Recognition\):  光学字符识别,是指对图片文件中的文字进行分析识别，获取的过程。

Tesseract是开源的OCR识别引擎，初期Tesseract引擎由HP实验室研发，后来贡献给了开源软件业，后经由Google进行改进，消除bug，优化，重新发布。Tesseract\(/'tesərækt/\) 这个词的意思是"超立方体"，指的是几何学里的四维标准方体，又称"正八胞体"。

所谓 OCR 是图像识别领域中的一个子领域，该领域专注于对图片中的文字信息进行识别并转换成能被常规文本编辑器编辑的文本。

在 1995 年 Tesseract 曾是世界前三的 OCR 引擎，而且在现在的免费 OCR 引擎中，其识别精度也仍然是出类拔萃的。因为其免费与较好的效果，许多的个人开发者以及一些较小的团队在使用着 Tesseract ，诸如验证码识别、车牌号识别等应用中，不难见到 Tesseract 的身影。

**Tesseract 4.0介绍**

Tesseract 4.0中包含了一个新的基于神经元网络的识别引擎，使得识别的精度比以前的版本大大提高了，相应的，对机器的计算能力要求也有了一个显著的提高。当然对于复杂的语言，它实际上比基本Tesseract要运行得更快。

和基本的Tesseract相比，神经元网络要求大量的训练数据，训练速度也慢了很多。对于拉丁语系的语言，版本中提供的训练好的模型是在400000个文本行，4500种字体上训练得到的。对于 其他语言，可能没有这么多 的字体，但它们训练的文本行数是差不多的。Tesseract的训练将需要几天到2周的时间，而不是几分钟到几个小时。即使使用了这么多的训练数据，你可能还是发现，它并不适合你特定的问题，因此你还需要重新训练模型。


