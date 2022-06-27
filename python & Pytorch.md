# python 安装 和依赖
>* 在vscode 下需要安装的插件
>* AREPL for python
>* requests 模块（使用命令 pip install requests       进行安装）
      如果出现“requests" is not accessedPylance Import "requests" could not be resolved ....
      ”报错的话， 在.vscode文件夹中新建 settings.json 文件，在文件添加
      {
            "python.pythonPath": "C:\\python38\\python.exe",
            ...,
            ...,
      }

>* Kite AutoComplete插件
>* Python Docstring Generator
>* 安装pytest
      在设置中，输入“test” 查找python test
      在"python>testing: Pytest Enablle"选中“Enable testing using pytest”,此时下发会提示需要下载"pytest"。点击安装。

# 修改默认版本
>* 先删除默认的Python软链接：
>>* sudo rm -rf /usr/bin/python
>* 建立新的软链接，指向另一版本
>>* sudo ln -sf /usr/bin/python2.7 /usr/bin/python








# pytorch 安装和依赖
1、Anaconda镜像源网址
	https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/

2、手动添加变量环境
	在path中添加：
		安装目录\Anaconda
		安装目录\Anaconda\Scripts 
		安装目录\Anaconda\Library\bin

3、在cmd(win+r)中搭建conda环境
	3.1、由于默认源的下载速度通常缓慢，因此更换为清华源（或科大源等）：在cmd中执行：
		》conda config --add channels http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/win-64/
		》conda config --add channels http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/win-64/
		》conda config --set show_channel_urls yes
		》conda config --add channels http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/win-64/
	（如果想换回默认安装源，执行：》conda config --remove-key channels）

	3.2、创建环境
		》conda create -n pytorch_cpu python=3.8
	3.3、打开Anaconda Prompt （在开始里有，和cmd终端一样）
	
	3.4、激活环境
		在Anaconda Prompt 输入：》activate pytorch_cpu
		（如果想退出激活环境状态，输入：》deactivate 	）	

4、安装pytorch
	官网让使用的指令：conda install pytorch torchvision torchaudio cpuonly -c pytorch (-c pytorch)  
	但由于使用清华镜像源，所以最后去掉 -c pytorch  。因此最后安装时需要的命令时是：
	》conda install pytorch torchvision torchaudio cpuonly 

5、命令行输入python进入python，并输入下面代码：
>*	> import torch
>*	> import torchvision
>*	> print(torch.__version__)
>*	成功打印，测试成功，使用exit()退出python。

# 运行pip3 (python3.9)报错 ***cannot import name 'sysconfig' from 'distutils'***
>* 安装依赖
>>* sudo apt-get install python3.9-distutils
>>* sudo python3.9 -m pip install --upgrade --force-reinstall pip

# 解决 解决 ***UserWarning: Matplotlib is currently using agg, which is a non-GUI backend....*** 和 ***ModuleNotFoundError: No module named 'tkinter'***

>* 安装QT5
>>* sudo pip install  PyQt5
>* 引入以下文件
```
import matplotlib
import matplotlib.pyplot as plt
matplotlib.use('Qt5Agg')
```

# 安装/卸载pip
>* 方法1:
>>* sudo apt-get install python-pip		或    sudo apt-get install python3-pip
>* 方法2: 可以制定python版本
>>* curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
>>* sudo pythonx.x.x get-pip.py
>*  可执行程序在 /usr/local/bin