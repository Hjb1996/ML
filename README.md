1:数据下载
 coco数据集较大，可以使用Google gsutil工具搭配命令行下载

 sudo apt-get install aria2
 aria2c -c <url> 

train2017：http://images.cocodataset.org/zips/train2017.zip
val2017：http://images.cocodataset.org/zips/val2017.zip
test2017：http://images.cocodataset.org/zips/test2017.zip
trainval2017：http://images.cocodataset.org/annotations/annotations_trainval2017.zip


2：配置要求
配置已经在requirements.txt中进行写出，执行：

pip install -r requirements.txt

亲测最新的torch 1.7也可以使用

3：整体结构 darknet53
主干特征提取网络->提取特征；输入是一个416*416*3->进行下采样，宽和高被不断地压缩，通道数不断地扩张。->我们获取一堆特征层，可以表示输入图片的特征。

4:残差网络
    权值初始化--->卷积 下采样 + darknet53; 先使用1*1的卷积（32通道的），再使用3*3的卷积（64通道的）,可以帮助我们减少参数量； 先用1*1的卷积调整通道数，3*3的网络进行特征提取。5次循环，2次卷积进行分类预测和回归预测。

5：评估指标
    阈值为0.5时，F1值,Recall值,Precision值

6：损失函数
    采用sigmoid+交叉熵损失函数，反向传播进行loss的传递