[合集 \- Jetson(1\)](https://github.com)1\.Jetson Orin NX烧录\+设备树更改？看这一篇就够了！11\-27收起
# Jetson Orin NX烧录\+设备树更改？看这一篇就够了！



> 笔者的设备为Jetson Orin NX 16GB \+ 达妙科技的Orin NX载板
> 本博客同步发表在CSDN：[https://blog.csdn.net/xiongqi123123/article/details/144079706](https://github.com)


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ea91404c3de3423db39061956a3a6d79.jpeg#pic_center)](https://i-blog.csdnimg.cn/direct/ea91404c3de3423db39061956a3a6d79.jpeg#pic_center)
[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b716a879a39e4433bfe8875f69219eb5.jpeg#pic_center)](https://i-blog.csdnimg.cn/direct/b716a879a39e4433bfe8875f69219eb5.jpeg#pic_center)


## 一、SDKmanager安装


        由于Nvidia近几年的JetPack更新都没有提供Jetson系列对应的镜像，因此不管是TF卡、EMMC还是NVME都无法像树莓派、RDK等开发板一样直接将系统烧录至对应的存储设备，转而需要使用Nvidia Jetson官方的SDKmamager进行烧录


* SDKmanager下载链接：[SDK Manager \| NVIDIA Developer](https://github.com)
* VMware17虚拟机下载链接：[VMware 17安装教程](https://github.com)


        要注意，SDKmanager只有在Linux环境中才能使用，因此我们还需要安装对应Linux环境且虚拟机需要80GB以上！！！我使用的VMware17\+Ubuntu22\.04，具体的JetPack支持如下图：


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2d5568b641fa4881b3c3adcec64a0683.png#pic_center)](https://i-blog.csdnimg.cn/direct/2d5568b641fa4881b3c3adcec64a0683.png#pic_center)


总的来说，Ubuntu22\.04只有JetPack6\.x才支持


我们首先将SDKmanager的Deb包下载到虚拟机之后输入命令即可开始安装SDKmanager：



```
sudo dpkg -i SDKmanager-xxxxxxxxxx.deb   #将-i后面改为对应下载的文件名

```

！！！不出意外安装过程中可能会失败，这是因为系统缺少对应的包，输入以下命令即可解决问题并且同时完成安装



```
sudo apt --fix-broken install

```

安装完成之后我们输入以下命令即可打开sdkmanager（当然也可以在应用中心直接双击打开）



```
sdkmanager    #打开sdkmanager

```

[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/67ffb17cf8d94cf9bfc572e4455aee31.png#pic_center)](https://i-blog.csdnimg.cn/direct/67ffb17cf8d94cf9bfc572e4455aee31.png#pic_center)


## 二、Jetson Orin NX的烧录


        首先我们先把改接的线路接好，在这个图片里我从左到右从上到下依次接的线为HDMI、扩展坞以及Recovery数据口的数据线（链接电脑），我们看板子的右下角有三个小按钮，中间的那个按钮为Rec即Recovery按钮。
[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/501128a323bb444cbb1ae715effcba5a.jpeg#pic_center)](https://i-blog.csdnimg.cn/direct/501128a323bb444cbb1ae715effcba5a.jpeg#pic_center)


        相对Jetson设备进行烧录，首先需要进入到设备的Recovery模式，每个设备进入Recovery模式的方法不一样，比如如果使用的如果是官方的Nano或者是NX开发者套件，那么可以通过将Rec引脚与GND引脚短接即可进入Recovery模式，但是我们使用的达妙的板子与官方的开发者套件进入Rec的方法不太一样，我们需要按住中间那个Rec按钮之后上电，并保持3s之后再松开，这样即可进入板子的Recovery模式（不进入Rec模式的话SDKmanager也能识别到，但是后续烧录会提示板子不在Rec模式无法烧录），我们将板子上电并进入烧录模式后虚拟机会提示让我们选择新USB需要连接到主机还是我们的虚拟机，由于我们是在虚拟机中进行烧录，所以需要选择连接到对应的虚拟机（建议勾选左下角“记住我的选择，以后不再询问”，因为烧录过程中设备会反复弹出然后重连，不勾选的话会需要一直选择比较麻烦）


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a15a2b4a822f45e5af255b386d4ada8f.png#pic_center)](https://i-blog.csdnimg.cn/direct/a15a2b4a822f45e5af255b386d4ada8f.png#pic_center)


        接着勾选连接到虚拟机后我们便能在SDKmanager主页的“Target Hardware”一栏中看到我们的设备（如果确认设别已经连接到虚拟机可以点击“Target Hardware”一栏下方的刷新按钮重新探测），在这个界面里第一栏我们选择Jetson即可，第二栏我们如果不需要在这个虚拟机里面安装对应Jetson设备的开发环境的话便可以将“Host Machine”取消勾选，这样可以节省虚拟机的空间以及节约烧录的时间，第三栏是用来选择需要安装的JetPack版本，我们根据自己需要选择即可，以下则为不同的JetPack对应的Ubuntu版本




| **JetPack 版本** | **L4T 版本** | **Ubuntu 版本** |
| --- | --- | --- |
| JetPack 6\.1 | L4T 36\.4 | Ubuntu 22\.04 (64\-bit) |
| JetPack 5\.x | L4T 35\.x | Ubuntu 20\.04 (64\-bit) |
| JetPack 4\.6 | L4T 32\.6\.x | Ubuntu 18\.04 (64\-bit) |
| JetPack 4\.5 | L4T 32\.5\.x | Ubuntu 18\.04 (64\-bit) |
| JetPack 4\.4 | L4T 32\.4\.x | Ubuntu 18\.04 (64\-bit) |
| JetPack 4\.3 | L4T 32\.3\.x | Ubuntu 18\.04 (64\-bit) |
| JetPack 4\.2 | L4T 32\.2\.x | Ubuntu 18\.04 (64\-bit) |
| JetPack 4\.1 | L4T 31\.1\.x | Ubuntu 18\.04 (64\-bit) |


        第四栏则为选择是否需要DeepStream(NVIDIA DeepStream 是一个端到端的视频分析 SDK，主要用于高性能的 AI 推理和视频分析任务)以及GXF Runtime（是NVIDIA Isaac SDK 的核心部分，专为图形化的任务执行设计，其主要用于机器人和边缘 AI 应用）如果我们不知道未来需不需要这两个SDK的话可以先不勾选，我们烧录完系统后还可以安装对应部分


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/cd4a42d119934402bb5276d697dab1ad.png#pic_center)](https://i-blog.csdnimg.cn/direct/cd4a42d119934402bb5276d697dab1ad.png#pic_center)


        我们点击“Continue”后便会进入到下一个页面，这个页面主要是来选择我们需要在烧录系统的同时具体安装哪一些官方的SDK组件，以及选择我们对应组件的下载路径，所需的SDK越多，对应虚拟机需要的空间越大，这也就是我们一开头说建议虚拟机分配80GB的原因，但是如果看到这你发现你虚拟机的空间少了也不用担心，我们还可以增加，只需要在Vmware的虚拟机管理页面增加空间再在Ubuntu里面启用磁盘工具扩容即可


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/50ea53646a624e51b2dfc10bd67bb96a.png#pic_center)](https://i-blog.csdnimg.cn/direct/50ea53646a624e51b2dfc10bd67bb96a.png#pic_center)


接着我们一部分一部分来介绍对应组件都有什么作用


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a036674c574c4e0ca2245f0bf7bac713.png#pic_center)](https://i-blog.csdnimg.cn/direct/a036674c574c4e0ca2245f0bf7bac713.png#pic_center)


        首先如上图文字所示，这部分的选项即用来选择是否下载以及烧录Jetson的Linux镜像，如果只是用SDKmanager来安装对应组件的话，这部分便可以不用勾选
[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c7aa6412e82e4008be32a1ef5b1c344b.png#pic_center)](https://i-blog.csdnimg.cn/direct/c7aa6412e82e4008be32a1ef5b1c344b.png#pic_center)
        "Jetson Runtime Components"这部分是Jetson的运行组件，具体用途以及功能如下，看不懂或者不清楚没有关系，这部分建议直接无脑勾选上！


* **Additional Setups**：用于安装 Jetson 设备运行时环境的附加设置，例如配置系统参数或依赖包
* **CUDA Runtime**：提供 CUDA（Compute Unified Device Architecture）运行时库，用于 GPU 的并行计算和深度学习推理，支持 CUDA 核心功能，例如矩阵运算、卷积操作和其他计算密集型任务
* **CUDA X\-AI Runtime**：扩展 CUDA 的 AI 加速功能，提供优化的深度学习运行时支持（可能包括 TensorRT 和 DNN 加速模块），针对 AI 应用（如图像分类、物体检测）提供额外的性能优化
* **Computer Vision Runtime**：专为计算机视觉任务设计的运行时环境，可能包含 OpenCV 和 VisionWorks 等库的支持，处理图像预处理、对象检测、特征提取等任务
* **NVIDIA Container Runtime**：支持容器化应用运行的运行时环境，例如运行基于 Docker 的 AI 应用，允许用户通过容器技术快速部署和测试 AI 应用，对需要使用 NVIDIA 容器技术（如 DeepStream 容器）的用户来说至关重要
* **Multimedia**：提供多媒体处理支持，例如视频编码解码、音频处理等功能，针对视频流处理（如 H.264/H.265 编解码）、摄像头输入等场景，对于需要处理视频或音频流的应用（如实时视频分析）非常重要
[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5214612ee1804b7ebca21bdd41e0717b.png#pic_center)](https://i-blog.csdnimg.cn/direct/5214612ee1804b7ebca21bdd41e0717b.png#pic_center)


        “Jetson SDK Components”和“Jetson Platform Services”是用来开发和优化 Jetson 平台上应用的关键工具和库，具体子组件用处和功能介绍如下，但是由于我们一般都是在主机对代码进行开发，然后放在Jetson平台上运行很少用Jetson平台来直接开发一些应用，因此这部分建议可以先不用勾选


* **CUDA**（此CUDA为开发包非运行包）：CUDA 开发包，包括开发者工具和库，用于编写和调试 GPU 加速代码，主要用于 GPU 计算开发，比如并行计算、深度学习推理加速等
* **CUDA\-X AI**：CUDA 的 AI 扩展包，包含专门针对深度学习和机器学习任务优化的库和工具。
* **Computer Vision**：提供用于计算机视觉的优化库和工具，可能包含 OpenCV 和 VisionWorks 等
* **Developer Tools**：包含调试、分析和优化工具，例如 Nsight Systems 和 Nsight Graphics


        因此，我们最简安装因该勾选的组件如下图所示，选择好后我们便可以点击左下角的"I accept..."之后再进行烧录啦,过程中会需要输入虚拟机的密码，输入之后便可开始烧录！！！


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4179e8d301e9444fb23d67397f8a705c.png#pic_center)](https://i-blog.csdnimg.cn/direct/4179e8d301e9444fb23d67397f8a705c.png#pic_center)
[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0c4b286ad2f648cbbb1ba7088dc84a48.png#pic_center)](https://i-blog.csdnimg.cn/direct/0c4b286ad2f648cbbb1ba7088dc84a48.png#pic_center)


        接着我们便进入了烧录，大概在烧录到12%的时候会弹出一个提示，这个提示是告诉你系统正在准备烧录Jetson Linux的镜像，在提示框中输入准备在Jetson Orin NX的系统中使用的账号和密码即可开始简易安装！
[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e31c319e17c6468f843e1bbbbe582f1b.png#pic_center)](https://i-blog.csdnimg.cn/direct/e31c319e17c6468f843e1bbbbe582f1b.png#pic_center)


        过程中系统会不断提示设备弹出以及设备重连，不用担心这是正常的，我们耐心选择连接到虚拟机即可


        ...大概进度到25%的时候，Jetson Orin NX的镜像便已经烧录完成，可以正常开机，只不过这还只是单纯的镜像，还需要继续烧录所需要的组件，如下图所示，即代表镜像已经烧录完成准备安装SDK组件，这时候需要输入我们的连接方式以及板子的IP地址和板子的账号密码，“Connection”可以选择我们与板子的连接方式，一般默认“USB”，IP Address为USB连接方式下虚拟网卡的静态IP地址，一般都是192\.168\.55\.1可以不用更改，接着我们输入我们最开始设置的用户名和密码再点击“Install”即可继续继续烧录


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2f720621cf6344e5ac4ce69048902953.png#pic_center)](https://i-blog.csdnimg.cn/direct/2f720621cf6344e5ac4ce69048902953.png#pic_center)


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d8520e0e8c7a4035b7a8666306d3c931.png#pic_center)](https://i-blog.csdnimg.cn/direct/d8520e0e8c7a4035b7a8666306d3c931.png#pic_center)


        等待一段时间之后（大概十分钟的样子），我们的Jetson就烧录完成了，但是！但是！！但是！！！不出意外应该会有某一个组件提示烧录失败，而且大概率是“Nvidia Container Runtime”这一个组件安装失败，这个时候我们不用担心，这是因为Docker源的问题，国内网速不好或者网络状态不好的时候都无法安装成功，但是我们不需要重新继续一次全部安装！！！


因为我们可以再进入系统之后手动进行安装！！！超快！！！！！！


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6ba42d278eae451c95134d9a9b72cb56.png#pic_center)](https://i-blog.csdnimg.cn/direct/6ba42d278eae451c95134d9a9b72cb56.png#pic_center)


        这时候我们可以通过ssh连接上我们的Jetson Orin NX，这时候先别急着把Rec数据线拔掉，我们可以先通过USB静态IP访问我们的NX



```
ssh NX的Username@192.168.55.1   #直接用USB静态IP

```

[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/fe44ff56e7fa4743a8a6b3ea0cc25450.png#pic_center)](https://i-blog.csdnimg.cn/direct/fe44ff56e7fa4743a8a6b3ea0cc25450.png#pic_center)


        但是这个时候我们会发现我们的NX没有WIFI，这是因为JetPack 6\.1的Bug导致网卡的内核驱动丢失导致的，解决这个问题需要从apt源下载对应的组件，但是我们的NX这时候又没有网络该怎么办呢？很简单，插网线即可，如果路由器在身边的话可以直接将网线接上我们的NX如果身边没有路由器或者路由器比较远的话也没有关系，我们可以打开Win11的设置，进入“网络和Internet”\-\>"高级网络设置"\-\>“WLAN”\-\>"更多适配器选项"，按照下图依次选择即可将我们电脑的WIFI网络通过网线共享出去，这时候只需要将我们的电脑和NX再用一根网线连接起来即可


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/57057342d5cd4338a2b9d9e9213ec540.png#pic_center)](https://i-blog.csdnimg.cn/direct/57057342d5cd4338a2b9d9e9213ec540.png#pic_center)


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a2d3939ba0a34ac19b1aad7fff07ffa8.png#pic_center)](https://i-blog.csdnimg.cn/direct/a2d3939ba0a34ac19b1aad7fff07ffa8.png#pic_center):[wgetcloud全球加速器服务](https://wgetcloud6.org)


        有了网络之后我们首先使用小鱼的工具来进行换源，以提高我们APT的下载速度



```
wget http://fishros.com/install -O fishros&&. fishros

```

[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/82a9e763bd3045c88d5759954a1bfb98.png#pic_center)](https://i-blog.csdnimg.cn/direct/82a9e763bd3045c88d5759954a1bfb98.png#pic_center)


我们输入5来更换系统源


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/89c6029f9da847028583c48bafbdd1a7.png#pic_center)](https://i-blog.csdnimg.cn/direct/89c6029f9da847028583c48bafbdd1a7.png#pic_center)


！！！Warning！！！


！！！注意！！！这里一定不能选择2！不能清理第三方源，我们选择“仅更换系统源”


！！！注意！！！这里一定不能选择2！不能清理第三方源，我们选择“仅更换系统源”


！！！注意！！！这里一定不能选择2！不能清理第三方源，我们选择“仅更换系统源”


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/edffde958b4c44ba9037e6cef1744099.png#pic_center)](https://i-blog.csdnimg.cn/direct/edffde958b4c44ba9037e6cef1744099.png#pic_center)


        至于这里的是否添加ROS源则大家根据自己的需求选择即可


        接着我们在ssh命令框中输入以下指令即可解决JetPack 6\.1版本NX没有WIFI的问题(随着时间的推移，每个人的NX的“5\.15\.136\-tegra”以及“linux\-headers\-5\.15\.136\-tegra\-ubuntu22\.04\_aarch64”可能都不一样，建议大家在输入命令的时候多使用Tab进行补全来匹配适合自己板子的对应文件)



```
sudo rm /lib/modules/5.15.136-tegra/build

```


```
sudo ln -s /usr/src/linux-headers-5.15.136-tegra-ubuntu22.04_aarch64/3rdparty/canonical/linux-jammy/kernel-source/ /lib/modules/5.15. 136-tegra/build

```


```
sudo apt install -y iwlwifi-modules

```


```
reboot

```

[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f8c998ea46cf4352afd48d6f3c5f497f.png#pic_center)](https://i-blog.csdnimg.cn/direct/f8c998ea46cf4352afd48d6f3c5f497f.png#pic_center)


        重启后我们便发现我们的NX有了WIFI，接着我们来解决“Nvidia Container Runtime”安装失败的问题，我们依次在命令行输入以下命令即可


* **手动添加 Docker 的 APT 源和 GPG 密钥：**



```
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=arm64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io

```

* **安装 NVIDIA Container Runtime：**



```
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) && \
curl -s -L https://nvidia.github.io/nvidia-container-runtime/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-runtime-keyring.gpg && \
curl -s -L https://nvidia.github.io/nvidia-container-runtime/$distribution/nvidia-container-runtime.list | \
sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-runtime-keyring.gpg] https://#g' | \
sudo tee /etc/apt/sources.list.d/nvidia-container-runtime.list
sudo apt-get update
sudo apt-get install -y nvidia-container-runtime

```

* **检查 Docker 是否工作正常**：



```
sudo docker run hello-world

```

* **检查 NVIDIA Container Runtime**：



```
sudo docker run --rm --gpus all nvidia/cuda:11.0-base nvidia-smi

```

如果运行正常，则说明NVIDIA Container Runtime已经手动安装完成啦！！！


如果遇到这个提示“Unable to find image 'hello\-world:latest' locally”，则是由于网络问题无法拉取Docker镜像，这个可以通过配置VPN及代理的方式解决


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/73432044fd4048f5b7c79bcd6dc7919d.png#pic_center)](https://i-blog.csdnimg.cn/direct/73432044fd4048f5b7c79bcd6dc7919d.png#pic_center)


## 三、Jetson其他配置


### 1、Jtop安装


        Jtop是Jetson系列设备最佳设备状态监控软件，可以实时查看CPU，GPU，内存等硬件设备使用情况，开发环境配置情况，同时方可以直接在图形化界面是设置运行功率和风扇转速。


安装的方式很简单，依次输入以下命令即可：



```
sudo apt install python3-pip

```


```
sudo -H pip3 install -U jetson-stats

```


```
reboot

```

重启之后我们在命令行输入jtop，即可查看到当前板子的各种状态


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/bf0971bb39d044cdb4b4eed1a1b68ec6.png#pic_center)](https://i-blog.csdnimg.cn/direct/bf0971bb39d044cdb4b4eed1a1b68ec6.png#pic_center)


### 2、CUDA与CUDNN的配置


        我们拿到这个新系统后我们输入nvidia\-smi发现会有对应的输出，但是输入nvcc \-V却说没有这个命令，注意！！！这不是因为我们的板子没有CUDA！只是因为我们没有将CUDA添加进系统环境变量，我们在“/usr/local”路径下发现其实我们是有安装CUDA的


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5dce2c3ccec248a3926b10970709898c.png#pic_center)](https://i-blog.csdnimg.cn/direct/5dce2c3ccec248a3926b10970709898c.png#pic_center)


解决办法很简单，只需要把如下的命令添加进\~/.bashrc即可



```
sudo gedit  ~/.bashrc    #进入bashrc并在最后添加即可

```


```
export CUBA_HOME=/usr/local/cuda
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export PATH=/usr/local/cuda/bin:$PATH

```


```
source ~/.bashrc

```

source之后我们再输入nvcc \-V会发现CUDA会有输出了,而CUDNN也是有安装的，我们无需自行安装


### 3、添加SWAP交换内存


虽然NX的内存有16GB，但是防止不够用我们还是可以添加交换内存来增加的我们的内存容量，使用如下命令即可



```
sudo fallocate -l 8G /var/swapfile
sudo chmod 600 /var/swapfile
sudo mkswap /var/swapfile
sudo swapon /var/swapfile
sudo bash -c 'echo "/var/swapfile swap swap defaults 0 0" >> /etc/fstab'

```

## 四、修改设备树，适配达妙载板（官方套件无需此步）


        由于达妙的载板是第三方的板子，载板上有一些设计的接口NX默认是不支持的，需要我们修改设备树进行使能，涉及到的口有一路串口以及一路USB3\.0的口，由于达妙商家并未明说具体是哪一路口未使能再加上我们RM需要用到这几个口于是我就没去测试到底是哪几个口未使能了


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1cef3c0be1c749919b171829deddf586.png#pic_center)](https://i-blog.csdnimg.cn/direct/1cef3c0be1c749919b171829deddf586.png#pic_center)


        达妙官方的说明书:[控制板/ORIN 载板 · kit/达妙科技 \- 码云 \- 开源中国](https://github.com)


        达妙载板的使用说明书：[达妙科技DM\-ORIN NX使用说明书V1\.1\.pdf](https://github.com)


        现在我们便开始修改NX的设备树，首先我们需要下载好以下两个包，一个是编译的工具链，一个是NX的内核源码


* 具体的下载链接：[Jetson Linux \| NVIDIA Developer](https://github.com)


在给出的页面中下载如下两个包至虚拟机即可


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4510f344559448d9bc01bfc8efaa0495.png#pic_center)](https://i-blog.csdnimg.cn/direct/4510f344559448d9bc01bfc8efaa0495.png#pic_center)


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e09322ddf1614b9899ddbd972aa5af84.png#pic_center)](https://i-blog.csdnimg.cn/direct/e09322ddf1614b9899ddbd972aa5af84.png#pic_center)


        接着我们在虚拟机中将下载好的“aarch64\-\-glibc\-\-stable\-2022\.08\-1\.tar.bz2”以及“public\_sources.tbz2”解压至$HOME/nv\_src中



```
mkdir -p $HOME/nv_src
tar -xjf aarch64--glibc--stable-2022.08-1.tar.bz2 -C $HOME/nv_src
tar -xjf public_sources.tbz2 -C $HOME/nv_src

```

[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c6262dff908543b2849f866abfc62bbe.png#pic_center)](https://i-blog.csdnimg.cn/direct/c6262dff908543b2849f866abfc62bbe.png#pic_center)


并保证“aarch64\-\-glibc\-\-stable\-2022\.08\-1”里的目录结构如下图所示


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8878e2852a624829b8be94133183be7c.png#pic_center)](https://i-blog.csdnimg.cn/direct/8878e2852a624829b8be94133183be7c.png#pic_center)


接着我们往\~/.bashrc中添加我们编译工具链的环境



```
export CROSS_COMPILE_AARCH64_PATH=$HOME/nv_src/aarch64--glibc--stable-2022.08-1
export CROSS_COMPILE=$HOME/nv_src/aarch64--glibc--stable-2022.08-1/bin/aarch64-buildroot-linux-gnu-

```

        接着我们根据Nvidia官方给出的步骤修改设备树，首先我们进入到public\_src里面的source目录下输入以下指令解压对应的包



```
cd /Linux_for_Tegra/source
tar xf kernel_src.tbz2
tar xf kernel_oot_modules_src.tbz2
tar xf nvidia_kernel_display_driver_source.tbz2

```


```
./generic_rt_build.sh "enable"      #启用实时配置

```

根据你当前使用编译的平台选择下载对应的工具链



```
sudo apt install tegra-21x-dt          #Jetson设备
sudo apt-get install device-tree-compiler #自己的主机  

```


```
make -C kernel    #编译内核

```

[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8e853e5983a24d12865fa183ea7d6eb3.png#pic_center)](https://i-blog.csdnimg.cn/direct/8e853e5983a24d12865fa183ea7d6eb3.png#pic_center)


        接着我们进入/home/xq/nv/Linux\_for\_Tegra/Linux\_for\_Tegra/source/hardware/nvidia/t23x/nv\-public可以找到一个文件叫“tegra234\-p3768\-0000\.dtsi”我们点进去按照如下修改



```
	serial@31d0000 {
			current-speed = <115200>;
			status = "okay";
		};
	/* 增加下面这部分开启串口1 */
	serial@3110000 {/* Enable UART1 */
		status = "okay";
	};
	/* ......  */
	/* 找到这部分，并按照下面的修改，增加usb2-2和usb3-2，用于开启额外的USB3.0 */
	padctl@3520000 {
			status = "okay";

			pads {
				usb2 {
					lanes {
						usb2-0 {
							nvidia,function = "xusb";
							status = "okay";
						};

						usb2-1 {
							nvidia,function = "xusb";
							status = "okay";
						};

						usb2-2 {
							nvidia,function = "xusb";
							status = "okay";
						};
					};
				};

				usb3 {
					lanes {
						usb3-0 {
							nvidia,function = "xusb";
							status = "okay";
						};

						usb3-1 {
							nvidia,function = "xusb";
							status = "okay";
						};
						
						usb3-2 {
							nvidia,function = "xusb";
							status = "okay";
						};
					};
				};
			};

			ports {
				/* recovery port */
				usb2-0 {
					mode = "otg";
					vbus-supply = <&vdd_5v0_sys>;
					status = "okay";
					usb-role-switch;
				};

				/* hub */
				usb2-1 {
					mode = "host";
					vbus-supply = <&vdd_1v1_hub>;
					status = "okay";
				};

				/* M.2 Key-E */
				usb2-2 {
					mode = "host";
					vbus-supply = <&vdd_5v0_sys>;
					status = "okay";
				};

				/* hub */
				usb3-0 {
					nvidia,usb2-companion = <1>;
					status = "okay";
				};

				/* J5 */
				usb3-1 {
					nvidia,usb2-companion = <0>;
					status = "okay";
				};
				usb3-2 {
					nvidia,usb2-companion = <2>;
					status = "okay";
				};
			};
		};

		usb@3550000 {
			status = "okay";

			phys = <&{/bus@0/padctl@3520000/pads/usb2/lanes/usb2-0}>,
			       <&{/bus@0/padctl@3520000/pads/usb3/lanes/usb3-1}>;
			phy-names = "usb2-0", "usb3-0";
		};

		usb@3610000 {
			status = "okay";

			phys = <&{/bus@0/padctl@3520000/pads/usb2/lanes/usb2-0}>,
			       <&{/bus@0/padctl@3520000/pads/usb2/lanes/usb2-1}>,
			       <&{/bus@0/padctl@3520000/pads/usb2/lanes/usb2-2}>,
			       <&{/bus@0/padctl@3520000/pads/usb3/lanes/usb3-0}>,
			       <&{/bus@0/padctl@3520000/pads/usb3/lanes/usb3-1}>，
			       <&{/bus@0/padctl@3520000/pads/usb3/lanes/usb3-2}>;
			phy-names = "usb2-0", "usb2-1", "usb2-2", "usb3-0",
				    "usb3-1", "usb3-2";
		};

```

接着我们对依次编译NVIDIA 的树外内核模块以及设备树文件



```
#编译NVIDIA 的树外内核模块
cd /Linux_for_Tegra/source
export IGNORE_PREEMPT_RT_PRESENCE=1
export CROSS_COMPILE=/bin/aarch64-buildroot-linux-gnu-
export KERNEL_HEADERS=$PWD/kernel/kernel-jammy-src
make modules

```


```
#编译设备树文件
cd /Linux_for_Tegra/source
export CROSS_COMPILE=/bin/aarch64-buildroot-linux-gnu-
export KERNEL_HEADERS=$PWD/kernel/kernel-jammy-src
make dtbs

```

        完成编译之后我们便可以进入到以下路径“/Linux\_for\_Tegra/source/kernel\-devicetree/generic\-dts/dtbs”这里面就有我们要的dtb文件


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/cf73b5ea49644042a1fde0d21bfa8b1e.png#pic_center)](https://i-blog.csdnimg.cn/direct/cf73b5ea49644042a1fde0d21bfa8b1e.png#pic_center)


        我们可以发现里面有很多的DTB，如果不确定自己的Orin NX需要哪个，最简单的办法是打开你的NX，看一下/boot/dtb下面的文件，就比如我的Orin NX对应路径下面的文件是这个


[![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d4fb73e5bae34529bbb0d43f6b8f8348.png#pic_center)](https://i-blog.csdnimg.cn/direct/d4fb73e5bae34529bbb0d43f6b8f8348.png#pic_center)


        因此我们便可以在我们编译完成的那一堆DTB文件中找到我们所需的这个文件，并将其拿来替换掉Jetson里面原先就有的这个dtb文件，接着reboot重启一下便完成了我们的达妙载板的设备树更改使能


 \_\_EOF\_\_

   ![](https://github.com/SkyXZ)SkyXZ  - **本文链接：** [https://github.com/SkyXZ/p/18572123](https://github.com)
 - **关于博主：** 评论和私信会在第一时间回复。或者[直接私信](https://github.com)我。
 - **版权声明：** 本博客所有文章除特别声明外，均采用 [BY\-NC\-SA](https://github.com "BY-NC-SA") 许可协议。转载请注明出处！
 - **声援博主：** 如果您觉得文章对您有帮助，可以点击文章右下角**【[推荐](javascript:void(0);)】**一下。
     
