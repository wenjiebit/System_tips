# System_tips
CNDNN只是CUDA下的一个H文件和动态链接库，只需要复制或者替换就可以

cuDNN is a cuda library which provides basic operations like forward and backward passes for neural networks on GPU. This is used by caffe and other toolkits.

You can download cuDNN from : https://developer.nvidia.com/cudnn. Note that you need to login to be able to download.

This post is just for my own reminder on cuDNN installation. Basically untar, and then copy the header and shared-libs to /usr/local/cuda-7.5

Assume cudnn-7.5-linux-x64-v5.0-ga.tgz is downloaded in a directory ./cudnn

$ tar -xvzf cudnn-7.5-linux-x64-v5.0-ga.tgz

$ sudo cp -i include/cudnn.h /usr/local/cuda-7.5/include/
sudo cp -i lib64/libcudnn* /usr/local/cuda-7.5/lib64/

After this, just cmake caffe and you should see cuDNN found (see my config).
