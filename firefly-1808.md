# 网络设置
## 无线设置
### 常用命令
>* 激活网络
>>* ifconfig wlan0 up
>* 扫面网络
>>* iwlist wlan0 scan
>* 关闭链接
>>* wpa_cli terminate
>* 连接状态
>>* wpa_cli status

### 无线网口配置
>* 新建文件 /etc/my_wpa_supplicant.conf，添加如下内容
>>* 
```
ctrl_interface=/var/run/wpa_supplicant
network={
	ssid="TP-LINK_22E1D2"
	psk="密码"
}
```
>* 连接wlan0到网络，并以daemon方式运行
>>* wpa_supplicant -B -i wlan0 -c /etc/my_wpa_supplicant.conf
`
-B Background 在后台以daemon 运行
-i interface
-c 配置文件
`
>* 设置IP地址
>>*  ifconfig wlan0 192.168.1.131
>*  加入网关到路由
>>* route add default gw 192.168.1.1 dev wlan0
>
>* 每次开机都需要重新配置无线`wlan0`
```
wpa_supplicant -B -i wlan0 -c /etc/my_wpa_supplicant.conf
ifconfig wlan0 192.168.1.131
route add default gw 192.168.1.1 dev wlan0
```
### 开机启动无线设置
>* 1、创建.sh 文件；
>* 2、给予可执行权限；
>* 3、将 `wpa_supplicant -B -i wlan0 -c /etc/my_wpa_supplicant.conf` 添加到文件中；
>* 4、.sh文件复制到 `/etc/profile.d` 目录下。
>* 5、重启 `reboot`   完成！


# RK1808 debug调试 
>* ***举例 `REPO_SDK/rk1808_linux_release_20210306/external/rknpu/rknn/rknn_api/examples/rknn_mobilenet_demo`***
>* REPO_SDK文件夹中有交叉编译工具，位于 ` ...../REPO_SDK/rk1808_linux_release_20210306/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin`
>* 1、将 ` ...../REPO_SDK/rk1808_linux_release_20210306/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/gdbserver` 复制到 开发板 `/usr/bin` 目录下。
>* 2、在 `CMakeLists.txt` 文件中添加如下两行，用于设置编译模式和优化等级。 
`
set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb") 
set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")
`
>* 3、 创建调试文件 ***launch.json***
```
{
      "version": "0.2.0",
      "configurations": [
            {
                  "name": "rknn_mobilenet_demo",
                  "type": "cppdbg",
                  "request": "launch",
                  "program": "${workspaceFolder}/install/rknn_mobilenet_demo/rknn_mobilenet_demo",
                  "args": [
                        "${workspaceFolder}/install/rknn_mobilenet_demo/mobilenet_v1.rknn",
                        "${workspaceFolder}/install/rknn_mobilenet_demo/dog_224x224.jpg",
                  ],
                  "stopAtEntry": false,
                  "cwd": "${fileDirname}",
                  "environment": [],
                  "externalConsole": false,
                  "MIMode": "gdb",
                  "miDebuggerPath": "${workspaceFolder}/../../../../../../prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gdb",
                  // "miDebuggerPath": "/home/hdb/linuxBoard/firefly-Core-1808-JD4-3399/REPO_SDK/rk1808_linux_release_20210306/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gdb",
                  "miDebuggerServerAddress": "192.168.1.107:2001",	//RK1808实际地址和端口号
                  "setupCommands": [
                        {
                              "description": "为 gdb 启用整齐打印",
                              "text": "-enable-pretty-printing",
                              "ignoreFailures": true
                        }
                  ]
            }
      ]
}
```
>* 4、新建脚本 ***make_build_debug.sh***
```
#!/bin/bash
mkdir -p build_debug
cd build_debug
cmake .. -DCMAKE_BUILD_TYPE=Debug
make
make install
```
>* 5、新建脚本 ***make_clean.sh***
```
#!/bin/bash
rm -rf build_debug
rm -rf build_release
```
>* 6、 执行.make_build_debug.sh 文件,生成 `install/rknn_mobilenet_demo/rknn_mobilenet_demo`
>* 7、将`install/rknn_mobilenet_demo` 复制到 RK1808 `/userdata/` 目录下
>>* scp -r -d install/rknn_mobilenet_demo root@192.168.1.107:/userdata/
>* 8、在RK1808上， 进入 `/userdata/rknn_mobilenet_demo` ,并开启 gdbserver
>>* cd /userdata/rknn_mobilenet_demo
>>* gdbserver 192.168.1.107:2001 ./rknn_mobilenet_demo mobilenet_v1.rknn dog_224x224.jpg
>>>* 终端显示：
```
Process ./rknn_mobilenet_demo created; pid = 702                                
Listening on port 2001 
```
>* 9、在vscode中点 ***F5*** 调试。

# 运行 **RK-Toolkit**
## 创建 **virtualenv** 环境
>* 安装 依赖
>>* sudo apt install virtualenv
>>* sudo apt-get install libpython3.6-dev
>>* sudo apt install python3-tk

>* 创建 环境搭建目录
>>* virtualenv -p /usr/bin/python3 /home/hdb/zmkg-pro/venv

>* 启用虚拟环境
>>* cd /home/hdb/zmkg-pro
>>* source venv/bin/activate
>>* 或 (source /home/hdb/zmkg-pro/venv/bin/activate)

>* 停止虚拟服务
>>* deactivate