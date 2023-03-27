# Typora+GitHub代替云笔记（Git上传文件至GitHub）

## Part01 前提环境

拥有GitHub/Gitee账户，电脑上装有Git。建议在Typora中设置图片存储路径为相对路径（图片也上传到GitHub上），这样远程打开笔记时也能显示图片。

## Part02 操作方法

### 2.1 创建仓库

2.1.1 登录GitHub，进入主页面，选择新建仓库

![](./image/01.png)

2.1.2 根据自己实际情况，编写创建的仓库信息

![](./image/02.png)

2.1.3 复制仓库的https地址备用

![](./image/03.png)

### 2.2 安装Git

2.2.1 进入Git官网（https://git-scm.com/downloads）下载安装程序。此处比较考验网络。

2.2.2 选择安装包，根据自己的需要选择功能即可。我这里保持默认。

### 2.3 配置上传

2.3.1 进入我们需要上传到GitHub上的目录中，右键”打开Git Bash Here“。

2.3.2 输入命令”Git clone 仓库地址路径（就是之前复制的GitHub仓库地址）“。

2.3.3 如果项目仓库有文件的话，则本地项目文件夹下面就会多出个文件夹，该文件夹名即为GitHub上面的仓库名。将本地文件夹下的所有文件（除了新多出的那个文件夹不用），其余都复制到那个新多出的文件夹下.

2.3.4 在Git Bash中cd进入typora目录

![](./image/05.png)

2.3.5 接下来依次输入以下代码即可完成其他剩余操作

```
git add * （注：别忘记后面的*，此操作是把Test文件夹下面的文件都添加进来）
```

![](./image/06.png)

```
git commit -m "提交信息" （注：“提交信息”里面换成你需要，如“first commit”）
```

如果是第一次使用的话，需要输入一些作者信息，按照提示信息输入即可。

![](./image/08.png)

```
Git push -u origin master（注：此操作目的是把本地仓库push到GitHub上面，此步骤需要你输入帐号和密码）
```

## Part03 常见报错及解决办法

### 错误1：LF与CRLF报错

#### 错因分析

此处执行报错，是因为LF和CRLF其实都是换行符，但是不同的是，LF是Linux和Unix系统的换行符，CRLF是window 系统的换行符。这就给跨平台的协作的项目带来了问题，保存文件到底是使用哪个标准呢？ Git为了解决这个问题，提供了一个”换行符自动转换“的功能，并且这个功能是默认处于”自动模式“即开启状态的。这个换行符自动转换会把自动把你代码里与你当前操作系统不相同的换行的方式转换成当前系统的换行方式（即LF和CRLF之间的转换），这样一来，当你提交代码的时候，即使你没有修改过某个文件，也被Git认为你修改过了，从而提示"LF will be replaced by CRLF in *****"

#### 解决方法

最简单的一种办法就是把自动转换功能关掉。输入命令 ：Git config core.autocrlf false (仅对当前Git仓库有效）；Git config --global core.autocrlf false (全局有效，不设置推荐全局）。然后重新提交代码即可。

### 错误2：无法提交更改

#### 错因分析

若报错”failed to push some refs to https://GitHub.com/xxxxxx/xxxx.git“说明在创建GitHub仓库的时候选择了创建README.md文件

#### 解决方法

先执行” git pull --rebase origin master“，再push就可以了

### 错误3：OpenSSL SSL_read

#### 错因分析

若产生“fatal: unable to access ‘https://GitHub.com/…’: OpenSSL SSL_read: Connection was reset, errno 10054”，一般是这是因为服务器的SSL证书没有经过第三方机构的签署，所以才报错

#### 解决方法

参考网上解决办法：解除ssl验证后，再次Git即可。可以使用命令“ git config --global https.sslVerify "false“ ”。