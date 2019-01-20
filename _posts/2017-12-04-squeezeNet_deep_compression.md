---
layout:     post
title:      "car classification using squeezeNet following deep compression" 
subtitle:   ""
author:     "aichitudou"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - 深度学习
    - model compression
---



## 介绍
论文：https://arxiv.org/pdf/1602.07360.pdf <br>
用caffe实现squeezeNet的car的二分类，并且实现deep compression，在论文中有提及。<br>

主要参考的是下面的工作：<br>
[SqueezeNet](https://github.com/DeepScale/SqueezeNet)<br>
[EasyDeepCompression](https://github.com/nishisuke/EasyDeepCompression)<br>
[Deep-Compression-AlexNet](https://github.com/songhan/Deep-Compression-AlexNet/blob/master/decode.py)<br>

## 正文
### 数据集
参考的是[CarND-Vehicle-Detection](https://github.com/udacity/CarND-Vehicle-Detection)<br>
用的是它提供的[vehicle](https://s3.amazonaws.com/udacity-sdc/Vehicle_Tracking/vehicles.zip)和[non-vehicle](https://s3.amazonaws.com/udacity-sdc/Vehicle_Tracking/vehicles.zip)的图片做二分类.<br>

### 制作数据集
1. 进入到每一个有图片的文件夹，在命令行中中运行以下命令，目的是为了把当前文件的路径存到a.txt中
```
ls | grep ".png" | sed "{s:^:`pwd`/:;s/$/& 1/}" > a.txt
```
|    ：表示通道，前面的输入当作后的输入<br>
sed s：替换<br>
^    : 表示文件开头，也就是将每一行开头替换成'pwd'命令的输出<br>
;    : 表示是两个的命令的分隔<br>
$    : 表示文件的结尾，将结尾替换成" 1"，也就是在最后加上该图片的标签，正样本就是1，负样本就是0<br>
&    : 指代每一行的内容

2. 用[random_split_train_val.py](https://github.com/aichitudou/squeezenet_deep_compression/blob/master/random_split_train_val.py)把数据集按5:1分成了训练集和数据集
```
python random_split_train_val.py a.txt b.txt c.txt ...
```

3. 上面的命令之后会生成几个 \*\_train.txt和\*\_val.txt，将它们各自连成一个文件
```
cat *_train.txt > train.txt
``` 

4. 打乱train.txt中的每一行，生成到train_random.txt文件中去，（原来记得好像需要将0标签的放到第一行，不知道对不对，我随机生成的是1标签在第一行，我就打开val_random.txt随便找了一行0标签的，替换到第一行了）
```
awk 'BEGIN{srand()}{b[rand()NR]=$0}END{for(x in b)print b[x]}' train.txt | tee train_random.txt
```

OK，数据集最后就得到了，train_random.txt

### 训练car二分类模型

需要train_val.prototxt,solver.prototxt,squeezenet_v1.1.caffemodel这三个文件<br>

修改`train_val.prototxt`<br>
由于我是用的自己的数据集的.txt格式的集合，没转成LMDB的caffe形式，所以在train_val.prototxt的data层做了修改，具体可以参考我的[github](https://github.com/aichitudou/squeezenet_deep_compression/blob/master/train_val.prototxt)。并且图像大小要记得设成64\*64。<br>

还有最后的`conv10`层的名字改成`conv10_car`了，对应的下面的`relu`层也做了相应的修改。`num_output`改成了2，因为要做2分类。修改名字是为了不在用pre-trained weights。我们要在自己的训练集上fine-tune。同时删除了最后的top-5那一层。因为我们只有2类，就别top-5了。


修改`solver.prototxt`<br>
test_iter：200   #表示测试集需要算多少次才能算完，我前面在train_val.prototxt的TEST阶段设的batch_size是20，而我的测试集大约有3900多张，所以200次可以算完。
test_interval: 1000 #每迭代1000次就在测试集上测试一下

训练,caffe的路径改成自己的就行了。<br>
```
../caffe/build/tools/caffe train -solver solver.prototxt -weights squeezenet_v1.0.caffemodel -gpu 0
```

最后得到`train_iter_70000.caffemodel`，准确率能到99.8%，模型大小为2.8M

测试模型的效果
```
python eval_classifier.py
```

### deep compression
需要用到[deep_compression](https://github.com/aichitudou/squeezenet_deep_compression/blob/master/deep_compression.py)和[deploy.prototxt](https://github.com/aichitudou/squeezenet_deep_compression/blob/master/deploy.prototxt)<br>
调用deep compression
```
python deep_compression.py deploy.prototxt train_iter_70000.caffemodel 0.3 64
```
0.3：表示每一层的weights排序后，后30%就直接剪掉变成0了
256：表示每一层的weights聚类中心个数为64

压缩过后，准确率基本不变，压缩过后的文件存到.npy文件里了，有758k大。转成caffemodel之后大小还是本来的那么大（2.8M）。

这个deep compression的主要思想说一下：<br>
先按你给的percent，将30%的比较小的weights变成0，然后将剩下的权重信息用kmeans聚类，用聚类中心来代表每一类的权重值。然后是将每个权重的索引表示成差分的形式，将值控制在4个位能表示的大小，不能表示就将它分解，分解出来的数对应的clusters.labels_插入一个0值，代表分解的这个值的聚类中心是0，也就是没有值。这样整个weights就可以用索引代表了，而索引值压缩到4位，那么两个值可以整合到一个bytes里面了。然后按着一定的顺序存起来，再按照这个规则decode就可以了。

参考了[超轻量级网络SqueezeNet算法详解](http://blog.csdn.net/shenxiaolu1984/article/details/51444525)

## 后记
上面的代码在我的github上可以找到<br>
[squeezenet_deep_compression](https://github.com/aichitudou/squeezenet_deep_compression)


