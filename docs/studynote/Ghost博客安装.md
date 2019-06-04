# Ghost博客安装

Ghost 是一个为博客和出版物而设计的内容平台，基于Markdown的编辑环境支持快速格式化及无缝的创作体验，将焦点完全放在正在创建的内容上。 并排实时预览可让您随时查看文章的显示方式。

## 新建非管理员账户

```properties
adduser luo    #luo替换成你的用户名不能是 root 和 ghost
usermod -aG sudo luo    #为该用户添加管理员权限
su - luo    #切换到该用户
```

## 更新软件包列表

```properties
sudo apt-get update
```

## 升级已安装包

```properties
sudo apt-get upgrade
```

## 安装nginx

```properties
sudo apt-get install nginx
```

## 设置防火墙允许nginx

```properties
sudo ufw allow 'Nginx Full'
```

## 安装MySQL

```properties
sudo apt-get install mysql-server
```

> 这里需要设置一个数据库root用户的密码

## 安装Nodejs

```properties
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash 
sudo apt-get install -y nodejs
```

## 安装Ghost-Cli

```properties
sudo npm i -g ghost-cli
```

## 通过Ghost-Cli 安装 Ghost
> 新建一个文件夹用来存放Ghost

```properties
sudo mkdir -p /var/www/ghost
```
> 让用户拥有该文件夹的权限

```properties
sudo chown luo:luo /var/www/ghost    #luo是前面新建的用户名
```
> 设置该文件夹的775权限（当前用户可读可写）

```properties
sudo chmod 775 /var/www/ghost
```
> 前往Ghost目录

```properties
cd /var/www/ghost
```

## 安装 Ghost

```properties
ghost install
```

## 安装成功后

> 安装成功后会有相应的提示，然后自在继续操作

* 会让你输入主机ip或者域名，注意：`需要http://前缀`，我的是`http://luokangyuan.com`
* 确定是localhost,这个不用改，直接回车
* 会让你输入Mysql的账户，输入root
* 会让你输入mysql的root账户的密码，这个密码我们再前面安装Mysql的时候设置过
* 这个时候会让你输入邮箱，输入就可以
* 后面所有的一路输入Y，确认操作即可