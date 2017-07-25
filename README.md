DeepLab
=====
机器学习、深度学习、量化投资，好用的工具、实用的工具，尽在DeepLab ! 

验证码识别
---------
### 问题
通常验证码识别需要如下几个步骤：

（1）将整张图片切割为多个小图片，每个小图片包含一个字符

（2）训练模型，识别每个小图片中的字符

（3）将每个小图片的识别出的字符拼接为字符串作为整体识别结果

复杂一点的验证码往往各个字符不是均匀分布，难以准确切割；切割质量直接影响到识别的准确度。是否可以不切割图片直接识别其中的字符串？

### 思路
这其实是一个多标签分类问题，每个验证码图片有4个字符（标签），并且顺序固定；只要将卷积神经网络的最后一层softmax稍加修改就能实现多标签分类。

具体来说，假设我们的验证码一共有4个字符,每个字符取26个大写字母中的一个；将卷积神经网络的输出层激活函数修改为sigmoid，输出层的[0-25]输出值对应第一个字符的onehot编码，[26-51]输出值对应第二个字符的onehot编码，[52-77]输出值对应第三个字符，[78-103]输出值对于第四个字符，并使用binary_crossentropy作为损失函数。

### 实验环境
keras theano cv2

训练集4000张图片，测试集2000张，每张图片大小20*80

### 网络结构

### 代码结构
generate.py 在GenPics目录下随机生成6000张验证码图片及其对应验证码字符串。

cnn_end2end_ocr.py 端到端验证码识别模型的训练、测试。

### 模型结果
Epoch = 60，Test Whole Accurate :  0.994

对于本文使用的较为简单的验证码，测试集整体正确率(4个字符都正确识别)99%以上。

### 参考：
https://zhuanlan.zhihu.com/p/21344595
