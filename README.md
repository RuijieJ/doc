## 在Docker容器中使用Jupyter
由于Docker容器没有图形界面，难以查看其中的图片，同时不习惯使用vim的同学对编辑器中的文档和代码也有困难。Jupyter可以用自己的电脑远程连接Docker容器，为其提供图形界面。**本文档初稿由林东生同学完成。**

### 1. 按照操作指南中的步骤登录服务器，并连接计算节点

### 2. 打开容器，并绑定计算结点的某端口和容器的某端口
TensorFlow组：
```shell
nvidia-docker run -it -p <remote_port>:<docker_port> -v /home/tsinghuaee76/OCR_experiments/Datasets/Scene_Text:/Dataset tensorflow/east
```
PyTorch组：
```shell
nvidia-docker run -it -p <remote_port>:<docker_port> -v /home/tsinghuaee76/OCR_experiments/Datasets/Scene_Text:/workspace/Dataset pytorch/e2e-mlt
```
参数说明:
* <remote_port>: 计算结点中的某个未被占用端口，如果端口已被占用会提示"port is already allocated"
* <docker_port>：容器中某个端口，自定义

![image](https://github.com/RuijieJ/doc/blob/master/imgs/1.png)

### 3. 用tmux打开一个会话
```shell
tmux
```
如果提示没有安装tmux (bash: tmux: command not found)，则先安装
```shell
apt-get install tmux
tmux
```

### 4. 安装和运行Jupyter
第一次使用时需要先安装Jupyter，视网速情况需要3到10分钟不等
```shell
pip install jupyter
```
启动Jupyter
```shell
jupyter notebook --no-browser --port=<docker_port> --ip=0.0.0.0 --allow-root
```
参数说明：
* <docker_port>和2中的<docker_port>一致

![image](https://github.com/RuijieJ/doc/blob/master/imgs/2.png)
请记录下最后一行的token号，之后会用到。

### 5. 新开一个ssh窗口，连接容器

#### 使用mobaXterm连接时
使用mobaXterm时，点击窗口栏的"+"号，新打开一个本地窗口，然后输入
```shell
ssh -N -f -L localhost:<local_port>:localhost:<remote_port> -p <ssh_port> tsinghuaee<XX>@<node_ip>
```
使用XShell时，=====TO EDIT=====

参数说明：
* <local_port>：自己的电脑上任意未被占用的端口，自定义
* <remote_port>: 与2中的<remote_port>相同
* <ssh_port>: 固定为11100
* tsinghuaee<XX>：自己的服务器账号
* <node_ip>：计算节点c4130-015的<node_ip>为10.20.101.35；计算节点c4130-016的<node_ip>为10.20.101.36

![image](https://github.com/RuijieJ/doc/blob/master/imgs/3.png)

#### 使用Xshell连接时
使用Xshell时，首先新建一个会话
第一步，设置 连接 参数：
* 协议： 设置为ssh
* 主机：计算节点ip地址，计算节点c4130-015的ip为10.20.101.35；计算节点c4130-016的ip为10.20.101.36
* 端口：固定为11100

![image](https://github.com/RuijieJ/doc/blob/master/imgs/xshell1.png)

第二步，设置 连接-用户身份验证 参数：
* 用户名：自己的服务器账号
* 密码：自己的服务器密码

![image](https://github.com/RuijieJ/doc/blob/master/imgs/xshell2.png)

第三步，通过设置 SSH-隧道 添加端口侦听：
（1）在 TCP/IP转移 中，点击 添加 按钮

（2）设置 转移规则：
* 源主机：设置为localhost，即本机
* 侦听端口：自己的电脑上任意未被占用的端口，自定义
* 目标主机：计算节点ip地址，计算节点c4130-015的ip为10.20.101.35；计算节点c4130-016的ip为10.20.101.36
* 目标端口：与2中的<remote_port>相同

![image](https://github.com/RuijieJ/doc/blob/master/imgs/xshell3.png)

第四步，使用该新建会话进行连接

### 6. 在自己的电脑上打开Jupyter
打开浏览器，进入地址localhost:<local_port>，即可在该页面查看和修改Docker容器中的文件，其中<local_port>与5中的<local_port>相同。
第一次登录需要在"Password or token"栏中输入4中记录的token，点击"Log in"。

![image](https://github.com/RuijieJ/doc/blob/master/imgs/5.png)

### 7. 其他事项
(1) 当完成第2步之后，访问服务器的<remote_port>端口就相当于访问Docker容器的<docker_port>端口。例如，当我们在容器中保存了TensorFlow记录文件并想用TensorBoard进行可视化，可以在容器中输入
```shell
tensorboad –logdir=<log_path> –p <docker_port>
```
然后，在自己的电脑上打开浏览器，网址输入
```shell
<node_ip>:<remote_port>
```
即可打开TensorBoard页面。
参数说明：
* <log_path>: TensorFlow的summary文件所在文件夹
* <remote_port>: 与2中的<remote_port>相同
* <docker_port>: 与2中的<docker_port>相同
* <node_ip>: 与5中<node_ip>相同

(2) 再次提醒大家不要修改任何从tsinghuaee76账号下挂载到容器中的文件，即不要修改Dataset/文件夹中的任何文件。

