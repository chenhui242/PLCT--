# ROS2 Humble 在 openEuler-24.03-Arm 上的测试
## 环境信息
### 硬件信息
1.处理器Intel Core i5-9400H CPU
2.内存：16GB
### 软件信息
- 宿主机操作系统：Ubuntu 22.04
- OS 版本：openEuler-24.03-arm
- 镜像地址：https://mirror.sjtu.edu.cn/openeuler/openEuler-24.03-LTS/virtual_machine_img/aarch64/openEuler-24.03-LTS-aarch64.qcow2.xz
- 软件源：http://121.36.84.172/dailybuild/EBS-openEuler-24.03-LTS/EBS-openEuler-24.03-LTS/EPOL/multi_version/ROS/humble/aarch64/
## 准备工作
1. 下载 OpenEuler 镜像
`` wget https://mirror.sjtu.edu.cn/openeuler/openEuler-24.03-LTS/virtual_machine_img/aarch64/openEuler-24.03-LTS-aarch64.qcow2.xz``
2. 解压镜像
`` xz -d openEuler-24.03-LTS-aarch64.qcow2.xz``
3. 安装qemu
``sudo apt-get install qemu qemu-system qemu-utils ovmf``
4. 安装UEFI 固件
``sudo apt-get install qemu-efi-aarch64``
5. 设置磁盘空间
``qemu-img create -f qcow2 img_base.qcow2 20G``


## 使用 QEMU 命令启动虚拟机
具体的命令还得参考自己的文件位置信息，比如我的自己的OVMF.fd文件是在usr/share/ovmf/OVMF.fd目录下，所以我的命令如下：
``qemu-system-aarch64 
  -M virt -m 8G -cpu cortex-a72 -smp 8 
  -bios /usr/share/ovmf/OVMF.fd 
  -drive id=hd0,media=disk,if=none,file=img_base.qcow2 
  -device virtio-scsi-pci 
  -device scsi-hd,drive=hd0 
  -nic user,model=virtio-net-pci,hostfwd=tcp::2222-:22,hostfwd=tcp::8000-:80,hostfwd=tcp::8080-:8080,hostfwd=tcp::8888-:8888,hostfwd=tcp::9090-:9090,hostfwd=tcp::9000-:9000 
  -nographic``
当出现如下结果则启动了qemu：
![输入账号、密码](https://github.com/chenhui242/PLCT--/blob/main/images/%E7%99%BB%E5%BD%95.png)
- 用户名：root
- 密码：openEuler12#$
## 测试安装
### 执行以下命令来修改软件源
`` bash -c 'cat << EOF > /etc/yum.repos.d/ROS.repo
[openEulerROS-humble]
name=openEulerROS-humble
baseurl=http://121.36.84.172/dailybuild/EBS-openEuler-24.03-LTS/EBS-openEuler-24.03-LTS/EPOL/multi_version/ROS/humble/aarch64/
enabled=1
gpgcheck=0
EOF'``
### 安装ros-humble
```dnf install "ros-humble-*" --skip-broken --exclude=ros-humble-generate-parameter-library-example```
安装完成后，需要再```~/.bashrc ```，追加一个启动ros-humble命令
```source /opt/ros/humble/setup.sh```
![追加命令的过程](https://github.com/chenhui242/PLCT--/blob/main/images/%E8%BF%BD%E5%8A%A0.png)
最后输入命令```source  ~/.bashrc```激活
![测试：当出现如下结果时，则安装成功](https://github.com/chenhui242/PLCT--/blob/main/images/%E6%B5%8B%E8%AF%95.png)
## 测试用例及结果
|        测试用例名        |   结果   |
|-------------------------|----------|
| 测试 turtlesim功能       |  失败    |
| 测试ros2 pkg create      |  失败    |
| 测试ros2 pkg executables |  失败    |
| 测试ros2 pkg list        |  失败    |
| 测试ros2 pkg prefix      |  失败    |
| 测试ros2 pkg xml         |  失败    |
| 测试ros2 run             |  失败    |
| 测试ros2 topic list      |  成功    |
| 测试ros2 topic info      |  成功    |
| 测试ros2 topic type      |  成功    |
| 测试ros2 topic find      |  成功    |
| 测试ros2 topic hz        |  失败    |
| 测试ros2 topic bw        |  失败    |
| 测试ros2 topic echo      |  失败    |
| 测试ros2 param 工具       | 失败    |
| 测试ros2 service 工具     | 失败    |
| 测试ros2 node list        | 失败    |
| 测试ros2 node info        | 失败    |
| 测试ros2 bag 工具         |  失败    |
| 测试ros2 launch 工具      |  失败    |
| 测试ros2 interface list   |  失败   |
| 测试ros2 interface package | 失败   |
| 测试ros2 interface packages| 失败   |
| 测试ros2 interface show    | 失败   |
| 测试ros2 interface proto   | 失败   |
| 测试 ros 通信组件相关功能   |  失败  |

## 测试ros 基础工具相关功能
1. 测试 turtlesim
执行ros run相关指令时出错，目前不支持
![1](https://github.com/chenhui242/PLCT--/blob/main/images/1.png)

2. ros2 pkg 工具
执行ros pkg相关指令时出错，目前不支持
![2](https://github.com/chenhui242/PLCT--/blob/main/images/2.png)

3. 测试ros2 pkg executables
执行ros2 pkg executables相关指令时出错，目前不支持
![3](https://github.com/chenhui242/PLCT--/blob/main/images/3.png)

4. 测试ros2 pkg list
执行ros2 pkg list相关指令时出错，目前不支持
![4](https://github.com/chenhui242/PLCT--/blob/main/images/4.png)

1. 测试 ros2 pkg prefix
执行ros2 pkg prefix相关指令时出错，目前不支持
![5](https://github.com/chenhui242/PLCT--/blob/main/images/5.png)

1. 测试 tros2 pkg xml
执行ros2 pkg xml相关指令时出错，目前不支持
![6](https://github.com/chenhui242/PLCT--/blob/main/images/6.png)

1. 测试 ros2 run
执行ros run相关指令时出错，目前不支持
![7](https://github.com/chenhui242/PLCT--/blob/main/images/7.png)

1. 测试 ros2 topic list
执行ros2 topic list相关指令时出错，支持
![8](https://github.com/chenhui242/PLCT--/blob/main/images/8.png)

1. 测试 ros2 topic info
执行ros2 topic info相关指令时出错，支持
![9](https://github.com/chenhui242/PLCT--/blob/main/images/9.png)

1. 测试 ros2 topic type
执行ros2 topic type相关指令时出错，支持
![10](https://github.com/chenhui242/PLCT--/blob/main/images/10.png)

1. 测试 ros2 topic find
执行ros2 topic find相关指令时出错，支持
![11](https://github.com/chenhui242/PLCT--/blob/main/images/11.png)

1. 测试 ros2 topic hz
执行ros2 topic hz相关指令时出错，目前不支持
![12](https://github.com/chenhui242/PLCT--/blob/main/images/12.png)

1. 测试 ros2 topic bw
执行ros2 topic bw相关指令时出错，目前不支持
![13](https://github.com/chenhui242/PLCT--/blob/main/images/13.png)

1. 测试 ros2 topic echo
执行ros2 topic echo相关指令时出错，目前不支持
![14](https://github.com/chenhui242/PLCT--/blob/main/images/14.png)

1. 测试 ros2 param 工具
执行ros2 param 工具相关指令时出错，目前不支持
![15](https://github.com/chenhui242/PLCT--/blob/main/images/15.png)

1. 测试 ros2 service 工具
执行ros2 service 工具相关指令时出错，目前不支持
![16](https://github.com/chenhui242/PLCT--/blob/main/images/16.png)

1. 测试 ros2 node list
执行ros2 node list相关指令时出错，目前不支持
![17](https://github.com/chenhui242/PLCT--/blob/main/images/17.png)

1. 测试 ros2 node info
执行ros2 node info相关指令时出错，目前不支持
![18](https://github.com/chenhui242/PLCT--/blob/main/images/18.png)

1. 测试 ros2 bag 工具
执行ros2 bag 工具相关指令时出错，目前不支持
![19](https://github.com/chenhui242/PLCT--/blob/main/images/19.png)

1. 测试 ros2 launch 工具
执行ros2 launch 工具相关指令时出错，目前不支持
![20](https://github.com/chenhui242/PLCT--/blob/main/images/20.png)

1. 测试 ros2 interface list
执行ros2 interface list相关指令时出错，目前不支持
![21](https://github.com/chenhui242/PLCT--/blob/main/images/21.png)

1. 测试 ros2 interface package
执行ros2 interface package相关指令时出错，目前不支持
![22](https://github.com/chenhui242/PLCT--/blob/main/images/22.png)

1. 测试 ros2 interface packages
执行ros2 interface packages相关指令时出错，目前不支持
![23](https://github.com/chenhui242/PLCT--/blob/main/images/23.png)

1. 测试 ros2 interface show
执行ros2 interface show相关指令时出错，目前不支持
![24](https://github.com/chenhui242/PLCT--/blob/main/images/24.png)

1. 测试 ros2 interface proto
执行ros2 interface proto相关指令时出错，目前不支持
![25](https://github.com/chenhui242/PLCT--/blob/main/images/25.png)

1. 测试 ros 通信组件
执行ros 通信组件相关指令时出错，目前不支持
![26](https://github.com/chenhui242/PLCT--/blob/main/images/26.png)











