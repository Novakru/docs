1. `diffusors_lawDIS`代码
- 其实`__init__.py`的作用就是按照`huggface`代码库的结构，逐个文件夹的导入自定义的`autoencoder_kl_lawdis`实现。
- `.\diffusers_lawdis\models\autoencoders\autoencoder_kl_lawdis.py`该文件引用了`.\diffusers_lawdis\models\autoencoders\vae_lawdis.py`中的`Encoder`、`Decoder`实现，编码器和解码器的实现，变动和论文中提到的是相同的，在编码器和解码器之间添加跳跃连接，传递多尺度特征，保留高频细节。
- 核心：自定义`AE`的实现，没有提到训练细节。

2. 训练的核心`UnetModel`
- 文中推理使用的[unet_2d_conditionModel](https://github.com/huggingface/diffusers/blob/main/src/diffusers/models/unets/unet_2d_condition.py)代码实现。
- 其本身可以直接适配`8`的`in_channels`，但是目前常见的`ckpt`都是默认`Unet.in_channels = 4`所以其实还是需要先读取`pre_training`的权重，然后额外添加`4`个端口的新权重
- 维度分析


| `input_image` | 输入 | `(H_orig, W_orig, 3)` | 原始 PIL Image |
| `rgb` | 预处理 | `[1, 3, 768, 768]` | Resize 并归一化后的 Tensor |
| `h` | VAE Encode | `[1, 8, 96, 96]` | Encoder 原始输出 (含均值+方差) |
| `rgb_latent` | VAE Latent | `[1, 4, 96, 96]` | 采样/取均值后的图像特征 |
| `dis_latent` | 扩散初始 | `[1, 4, 96, 96]` | 初始化的随机噪声 |
| `unet_input` | U-Net 输入 | **`[1, 8, 96, 96]`** | *Latent (4) + Noise (4) 拼接* |
| `noise_pred` | U-Net 输出 | `[1, 4, 96, 96]` | 去噪步预测结果 |
| `pred` | VAE Decode | `[1, 1, 768, 768]` | 解码后的单通道 Mask |
| `dis_pred` | 最终输出 | `(H_orig, W_orig)` | Resize 回原图大小的 Numpy 数组 |

3. 回归基本功
- [(79 封私信 / 80 条消息) Diffusion 和Stable Diffusion的数学和工作原理详细解释 - 知乎](https://zhuanlan.zhihu.com/p/597924053)
- [(79 封私信 / 80 条消息) DiT：从理论到实践，万字长文深入浅出带你学习Diffusion Transformer - 知乎](https://zhuanlan.zhihu.com/p/711055614)

4. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxNDY5NzgzOTksMTQxMDMxOTc1NSwtNT
UzODc5NTQyLC0xMDQxMjMzMDYwLC0zMzAyODI3MzQsLTIwODg3
NDY2MTIsMTQwNjA5ODgyNCw1ODI2NTk4ODBdfQ==
-->