## 在Docker容器中使用Jupyter
由于Docker容器没有图形界面，难以查看其中的图片，同时不习惯使用vim的同学对编辑器中的文档和代码也有困难。Jupyter可以用自己的电脑远程连接Docker容器，为其提供图形界面。

### 1. 按照操作指南中的步骤登录天津院服务器，并连接计算节点

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
* <docker_port>和1中的<docker_port>一致

![image](https://github.com/RuijieJ/doc/blob/master/imgs/2.png)
请记录下最后一行的token号，之后会用到。

### 5. 新开一个ssh窗口，连接容器
```shell
ssh -N -f -L localhost:<local_port>:localhost:<remote_port> -p <ssh_port> tsinghuaee<XX>@<node_ip>
```
参数说明：
* <local_port>：自己的电脑上任意未被占用的端口，自定义
* <remote_port>: 与1中的<remote_port>相同
* <ssh_port>: 固定为11100
* tsinghuaee<XX>：自己的服务器账号
* <node_ip>：计算节点c4130-015的<node_ip>为10.20.101.35；计算节点c4130-016的<node_ip>为10.20.101.36

![image](https://github.com/RuijieJ/doc/blob/master/imgs/3.png)

### 6. 在自己的电脑上打开Jupyter
打开浏览器，进入地址localhost:<local_port>，即可在该页面查看和修改Docker容器中的文件，其中<local_port>与5中的<local_port>相同。
第一次登录需要在"Password or token"栏中输入4中记录的token，点击"Log in"。

![image](https://github.com/RuijieJ/doc/blob/master/imgs/5.png)

### 7. 注意事项
再次提醒大家不要修改任何从tsinghuaee76账号下挂载到容器中的文件，即不要修改/workspace/Dataset/中的任何文件。

