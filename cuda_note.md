# CUDA配置笔记

### 查看Nvidia驱动信息

1. `nvidia-smi`

   > 查看到驱动版本和该驱动能兼容的CUDA版本
   >
   > ![image-20220310162436231](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220310162436231.png)

2. `nvcc -V`

   > 查看安装的CUDA的版本信息（命令不存在，要添加环境变量）
   >
   > ![image-20220310162517860](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220310162517860.png)

3. CUDA安装目录在`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA`
4. CUDNN下载解压后，将`bin`，`include`，`lib/x64`中的文件复制到CUDA对应的目录中

### 卸载CUDA

1. 教程：https://blog.csdn.net/m0_37605642/article/details/99100924
2. 在控制面板->卸载程序中将对应版本的CUDA全卸载
3. 在卸载CUDA11.4的过程中，提示有一个叫SDK？的东西存在，直接删掉就行
4. 还把CUDA11.4的文件夹删除了，不知道对安装旧版本的有没有影响

### Pip安装GPU版的paddle

1. 官方教程：https://www.paddlepaddle.org.cn/documentation/docs/zh/install/pip/windows-pip.html

2. 前提：python，CUDA等安装好

3. GPU版本安装

   CUDA11.2的PaddlePaddle

   ```shell
   python -m pip install paddlepaddle-gpu==2.2.2.post112 -f https://www.paddlepaddle.org.cn/whl/windows/mkl/avx/stable.html
   ```

4. 进入python查看

   ```python
   import paddle
   
   paddle.utils.run_check()
   paddle.device.get_device()
   ```

​		![image-20220310164208930](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220310164208930.png)

![image-20220310164140747](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220310164140747.png)

​		

 5. 卸载paddle

    ```shell
    # CPU版本的PaddlePaddle:
    python -m pip uninstall paddlepaddle
    
    # GPU版本的PaddlePaddle:
    python -m pip uninstall paddlepaddle-gpu
    ```

    

