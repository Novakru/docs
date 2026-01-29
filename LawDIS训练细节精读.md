1. `diffusors_lawDIS`代码
- 其实`__init__.py`的作用就是按照`huggface`代码库的结构，逐个文件夹的导入自定义的`autoencoder_kl_lawdis`实现。
- `.\diffusers_lawdis\models\autoencoders\autoencoder_kl_lawdis.py`该文件引用了`.\diffusers_lawdis\models\autoencoders\vae_lawdis.py`中的`Encoder`、`Decoder`实现，编码器和解码器的实现，变动和论文中提到的是相同的，在编码器和解码器之间添加跳跃连接，传递多尺度特征，保留高频细节。
- 核心：自定义`AE`的实现，没有提到训练细节。

2. 训练的核心`UnetModel`
- 文中推理使用的[unet_2d_conditionModel](https://github.com/huggingface/diffusers/blob/main/src/diffusers/models/unets/unet_2d_condition.py)代码实现。
- 其本身可以直接适配`8`的`in_channels`，但是目前常见的`ckpt`都是默认`Unet.in_channels = 4`所以其实还是需要先读取`pre_training`的权重，然后额外添加`4`个端口的新权重
- 维度分析

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NTA5MzY3MDUsLTEwNDEyMzMwNjAsLT
MzMDI4MjczNCwtMjA4ODc0NjYxMiwxNDA2MDk4ODI0LDU4MjY1
OTg4MF19
-->