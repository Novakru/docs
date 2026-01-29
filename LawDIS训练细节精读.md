1. `diffusors_lawDIS`代码
- 其实`__init__.py`的作用就是按照`huggface`代码库的结构，逐个文件夹的导入自定义的`autoencoder_kl_lawdis`实现
- `.\diffusers_lawdis\models\autoencoders\autoencoder_kl_lawdis.py`该文件引用了`.\diffusers_lawdis\models\autoencoders\vae_lawdis.py`中的`Encoder`、`Decoder`实现，编码器和解码器的实现，变动和论文中提到的是相同的，在
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg1NDc4NjE2LC0yMDg4NzQ2NjEyLDE0MD
YwOTg4MjQsNTgyNjU5ODgwXX0=
-->