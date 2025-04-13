# 在 VMware 上安装 openEuler 的完整流

------

## 下载 openEuler

![image-20250313102344729](C:\Users\W\AppData\Roaming\Typora\typora-user-images\image-20250313102344729.png)



------

## 在 VMware 中安装 openEuler

### 1. 创建虚拟机

1. 下载完成后，打开 VMware（如果尚未安装，可参考 [如何安装 VMware](https://blog.csdn.net/weixin_74195551/article/details/127288338)）。

2. 点击 **新建虚拟机**，进入创建向导。![image-20250313102406576](C:\Users\W\AppData\Roaming\Typora\typora-user-images\image-20250313102406576.png)

   

3. 选择 **典型（推荐）** 模式。

   ![image-20250313102428259](C:\Users\W\AppData\Roaming\Typora\typora-user-images\image-20250313102428259.png)

4. 选择 **稍后安装操作系统**，然后点击 **下一步**。

   

5. 选择 **Linux** 作为操作系统类型，并指定 **Linux 5.x 内核 64 位**。

   

6. 设置虚拟机名称，并选择合适的存储位置。

   

### 2. 设置虚拟机

1. 为虚拟机分配 **32GB** 磁盘空间，并选择 **将磁盘存储为单个文件** 以提高性能。

   !

2. 进入 **自定义硬件** 选项。

   !

3. 在 **新 CD/DVD（IDE）** 选项中，选择刚刚下载的 openEuler ISO 镜像（文件名应为 `openEuler-22.03-LTS-SP1-x86_64-dvd`）。

   

4. 分配 **2GB 内存** 以确保流畅运行。

   

5. 设置 **2 个 CPU 内核**，然后点击 **完成**。

   

------

## 启动 openEuler 并完成安装

### 1. 配置openEuler选择刚刚创建的虚拟机，并点击 **开启**。

1. ![image-20250313102724915](C:\Users\W\AppData\Roaming\Typora\typora-user-images\image-20250313102724915.png)

2. 在启动菜单中选择 **Install openEuler 22.03-LTS-SP1** 进行安装。

   ![image-20250313102739516](C:\Users\W\AppData\Roaming\Typora\typora-user-images\image-20250313102739516.png)

3. 选择 **中文** 作为安装语言，便于操作。

   

4. 确认安装目标磁盘，直接点击 **完成**。

   ![image-20250313102802891](C:\Users\W\AppData\Roaming\Typora\typora-user-images\image-20250313102802891.png)

5. **启用以太网**，确保网络连接正常。

   !

### 2. 设置虚拟机用户

### **设置 root 用户密码**（请记住您的密码，安装完成后需要用到）。

1. 

2. **创建一个新用户**，作为日常操作账号。

   !

3. 安装完成后，**重启系统**，进入 openEuler。

   测试 openEuler

