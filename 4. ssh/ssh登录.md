### 基本用法

远程登录服务器：

```
ssh user@hostname
```

- `user`: 用户名
- `hostname`: IP地址或域名

第一次登录时会提示：

```
The authenticity of host '123.57.47.211 (123.57.47.211)' can't be established.
ECDSA key fingerprint is SHA256:iy237yysfCe013/l+kpDGfEG9xxHxm0dnxnAbJTPpG8.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

输入`yes`，然后回车即可。
这样会将该服务器的信息记录在`~/.ssh/known_hosts`文件中。

然后输入密码即可登录到远程服务器中。

***

默认登录端口号为22。如果想登录某一特定端口：

```
ssh user@hostname -p 22
```

***

### 配置文件

创建文件 `~/.ssh/config`。

`~/.ssh/config`文件的格式配置说明：

1. ​​基本结构​​
每个主机的配置以 Host开头，后跟别名，配置项需缩进（推荐4个空格或Tab）。例如：
```
Host myserver
    HostName 192.168.1.100  # 服务器IP或域名
    User root               # 登录用户名
    Port 22                 # SSH端口（默认可省略）
    IdentityFile ~/.ssh/id_rsa  # 指定私钥路径（免密登录关键，可省略）
```
2. ​​关键字段说明​​

- ​`​Host`​​：定义别名，如 myserver，后续通过 ssh myserver连接。
- `​HostName`​​：服务器的真实IP或域名。
- ​`​User`​​：登录用户名。(服务器里存在的用户名)
- ​`​IdentityFile`​​：指定私钥文件路径（如 id_rsa），公钥需提前上传至服务器的 ~/.ssh/authorized_keys文件中。
    - 可省略，默认为 ~/.ssh/id_rsa。
- ​`​Port`​​：非默认端口时需指定（如 2222）。
    - 默认为22。（可省略）

**注意：** `config`文件放在`~/.ssh/`目录下。

***

### 密钥登录

创建密钥：

```
ssh-keygen
```

然后一直回车即可。

执行结束后，`~/.ssh/`目录下会多两个文件：

- `id_rsa`：私钥
- `id_rsa.pub`：公钥

***

之后想免密码登录哪个服务器，就将公钥传给哪个服务器即可。

例如，想免密登录`myserver`服务器。则将公钥中的内容，复制到`myserver`中的`~/.ssh/authorized_keys`文件里即可。

也可以使用如下命令一键添加公钥：

```
ssh-copy-id myserver
```

***

### 执行命令

命令格式：

```
ssh user@hostname command
```

例如：
```
ssh user@hostname ls -a
```

或者
```
# 单引号中的$i可以求值
ssh myserver 'for ((i = 0; i < 10; i ++ )) do echo $i; done'
```

或者
```
# 双引号中的$i不可以求值
ssh myserver "for ((i = 0; i < 10; i ++ )) do echo $i; done"
```