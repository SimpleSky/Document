# 移动端GitLab使用说明

[GitLab管理后台地址](http://192.168.1.20:8081)，请自行登录注册账号。

可以使用SSH的验证方式，和用户名密码的验证方式。以下讲解都以OSX系统进行展开的，Windows用户请参考[Windows生成sshKey配置到GitLab](https://www.cnblogs.com/zqifa/p/gitlab-5.html)，或者自行百度。

> #### 使用SSH方式验证

以下仅以OSX用户为例：
1. 使用OSX系统的同学不用安装，OSX已经默认安装了该服务。
2. 安装完后，OSX用户打开终端，输入以下命令：
```
ssh-keygen -t rsa -C "你的邮箱(建议使用公司邮箱)"
```
之后会在`/Users/yourUserName/.ssh`路径下生成一个私钥文件`id_rsa`和一个公钥文件`id_rsa.pub`，然后将公钥复制添加到GitLab管理后台。

> #### iOS项目地址

1. 福利贷：ssh://git@192.168.1.20:2200/ios-team/fulidai.git
2. 福利金融：ssh://git@192.168.1.20:2200/ios-team/fulijinrong.git
3. 百贷宝：ssh://git@192.168.1.20:2200/ios-team/baidaibao.git

> #### Android项目地址

1. 福利贷：ssh://git@192.168.1.20:2200/android-team/fulidai.git
2. 福利金融：ssh://git@192.168.1.20:2200/android-team/fulijinrong.git
3. 百贷宝：ssh://git@192.168.1.20:2200/android-team/baidaibao.git
