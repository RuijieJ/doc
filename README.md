## 在天津院服务器计算节点中使用Jupyter
天津院服务器默认是没有图形界面的，使用Jupyter可以方便地用自己本地的浏览器管理服务器中的文件和运行程序。

### 1. 登录服务器，并连接计算节点

### 2. 建立一个tmux会话
这因为之后启动Jupyter时会占用现有窗口，使用tmux把Jupyter的窗口放在后台运行
```shell
tmux new -s jupyter
```

### 3. 安装Jupyter
第一次使用时需要先安装Jupyter，视网速情况大约需要5到30分钟不等，之后再启动Jupyter可以跳过此步骤
```shell
pip install jupyter
```

### 4. 运行Jupyter
```shell
jupyter notebook --no-browser --port=<remote_port> --ip=0.0.0.0 --allow-root
```
参数说明：
* <remote_port>:计算结点中的某个未被占用端口，可随意输入。如果端口已被占用会提示"port is already allocated"，换一个即可。

例如：

![image](https://github.com/RuijieJ/doc/blob/master/imgs/new1.png)
请记录下最后一行的token号（上例中为d4db1e2dd86e165d40b49a7ffca28ac39f184723af640354），之后会用到。

### 5. 新开一个ssh窗口，连接计算节点

#### 使用mobaXterm连接时
使用mobaXterm时，点击窗口栏的"+"号，新打开一个本地窗口，然后输入
```shell
ssh -N -f -L localhost:<local_port>:localhost:<remote_port> tsinghuaee<XX>@<node_ip>
```
参数说明：
* <local_port>：自己的电脑上任意未被占用的端口，自定义
* <remote_port>: 与2中的<remote_port>相同
* tsinghuaee<XX>：自己的服务器账号
* <node_ip>：自己当前连接的计算节点的IP地址
  
  c4130-017的<node_ip>为10.20.101.37
  
  c4130-018的<node_ip>为10.20.101.38
  
  c4130-019的<node_ip>为10.20.101.39
  
  c4130-020的<node_ip>为10.20.101.40

例如：
![image](https://github.com/RuijieJ/doc/blob/master/imgs/new2.png)

#### 使用Xshell连接时
使用Xshell时，首先新建一个会话
第一步，设置 连接 参数：
* 协议： 设置为ssh
* 主机：计算节点ip地址

  c4130-017的<node_ip>为10.20.101.37
  
  c4130-018的<node_ip>为10.20.101.38
  
  c4130-019的<node_ip>为10.20.101.39
  
  c4130-020的<node_ip>为10.20.101.40
* 端口：默认为22

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
* 目标主机：计算节点ip地址<node_ip>
* 目标端口：与2中的<remote_port>相同

![image](https://github.com/RuijieJ/doc/blob/master/imgs/xshell3.png)

第四步，使用该新建会话进行连接

### 6. 在自己的电脑上打开Jupyter
打开浏览器，进入地址localhost:<local_port>，即可在该页面查看和修改服务器中的文件，其中<local_port>与5中的<local_port>相同。
例如，按照上述例子，在本里浏览器中输入localhost:2333，即可进入如下页面：
![image](https://github.com/RuijieJ/doc/blob/master/imgs/new3.png)

在"Password or token"栏中输入4中记录的token，点击"Log in"。也可以在下方设置新的密码。

可进入如下页面开始管理你的服务器文件和运行程序：
![image](https://github.com/RuijieJ/doc/blob/master/imgs/new4.png)


