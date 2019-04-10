nvidia驱动与cuda安装
<!--more-->
### 安装过程
1. 在官网下载runfile文件，注意不要下载deb文件，无论是network还是local版本，都需要apt从nvidia源获取资源，然而某种神秘力量会导致安装失败，甚至连VPN也获取不到（可能是因为我太菜了吧）
2. 屏蔽开源驱动 nouveau 在`/etc/modprobe.d/blacklist.conf`中添加`blacklist nouveau`保存
3. 删除旧NVIDIA驱动
    ```
    sudo apt-get --purge remove nvidia-*
    sudo apt-get --purge remove xserver-xorg-video-nouveau
    ```
4. 重启电脑
5. `sudo service lightdm stop`若没有安装则先安装`sudo apt install lightdm`
6. 按Ctrl + Alt +F1(F1~F6均可)到x-server, Ctrl+Alt+F7是返回，并进入runfile文件目录
7.  ```
    Run `sudo sh cuda_10.1.105_418.39_linux.run`
    Follow the command-line prompts
    ```
8. 重启X-window 服务
    ```
    sudo service lightdm start
    ```
### 验证
1. 检查 CUDA Toolkit是否安装成功 
    ```
    nvcc -V
    ```
    报错的话可以根据提示解决
2. 进入到Samples安装目录，然后在该目录下终端输入make。可使用find命令
3. 可以在Samples里面找到`bin/x86_64/linux/release/`目录 并切换到该目录，运行deviceQuery程序

    `sudo ./deviceQuery`查看输出结果，重点关注最后一行，Pass表示通过测试  