# debian

## 初始化设置

为了提升系统的安全性和可靠性，进行基础的系统初始化设置。它们包括：

1. 创建一个普通管理员账户
2. 使用 SSH 密钥认证身份
3. 禁用 SSH 密码登录

### 一、创建普通管理员账户

#### 1. 创建普通用户

> 实际的用户名请按需修改

```sh
adduser zouzonghua
```

#### 2. 将用户添加到 sudo 用户组

使用 `usermod` 命令，附加 `-aG` 参数，将用户 `zouzonghua` 添加到 `sudo` 用户组。

```sh
usermod -aG sudo zouzonghua
```

> `-aG` 参数可以在保持用户原有用户组设置的前提下，将用户追加到指定的用户组。不使用 `-a` 参数会使用户离开原有用户组，仅加入到命令中指定的用户组。

### 二、使用 SSH 密钥登录云服务器

#### 1. 创建密钥对

##### Windows 系统

> 在本地计算机的 PowerShell 中执行 ssh-keygen.exe 命令，会在 C:\Users\Herald/.ssh/ 目录生成一个名为 id_rsa 的私钥和一个名为 id_rsa.pub 的公钥。

```sh
PS C:\Users\zouzonghua> ssh-keygen.exe
```

##### MacOS & Linux 系统

> 打开终端，执行 ssh-keygen 命令，会在 ~/.ssh/ 目录生成一个名为 id_rsa 的私钥和一个名为 id_rsa.pub 的公钥。

```sh
ssh-keygen
```

#### 2. 将公钥复制到云服务器

##### 通用方法

> Windows 系统将命令中的 ~/.ssh/id_rsa.pub 替换成 C:\Users\zouzonghua/.ssh/id_rsa.pub。

username 替换为登录云服务器的用户名，remote_host 替换为云服务器的 IP 地址。

```sh
cat ~/.ssh/id_rsa.pub | ssh username@remote_host "mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && chmod -R go= ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

### 三、禁用 SSH 密码登录 & ROOT 账户登录

编辑 ssh 服务的配置文件 `sshd_config` 将 PasswordAuthentication 项修改为 no ，PermitRootLogin 项修改为 no 。

```sh
sudo vi /etc/ssh/sshd_config
```

重启 ssh 服务

```sh
sudo systemctl restart ssh
```

## 安装常用软件

### zsh

查看系统当前的 shell

```sh
echo $SHELL
```

查看系统所有 shell

```sh
cat /etc/shells
```

安装&重启

```sh
apt install zsh
chsh -s /bin/zsh
sudo reboot
```

### 更新软件源、软件、删除没用到的包

```sh
sudo apt update
sudo apt upgrade
sudo apt autoremove
```

## 常见错误解决

### Errors were encountered while processing 的解决办法

```sh
cd /var/lib/dpkg
sudo mv info info.bak
sudo mkdir info
sudo apt-get upgrade
```
