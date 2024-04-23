<!--
 * @Author: chenleilong chenleilong@51yund.com
 * @Date: 2023-03-16 10:18:31
 * @LastEditors: chenleilong chenleilong@51yund.com
 * @LastEditTime: 2023-03-16 13:41:29
 * @FilePath: /GitHub/vuepress_blog/docs/note/Linux学习.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
# Linux

## 一些操作记录

### 常用操作
```shell
# 查看ip
hostname -1
ifconfig
ip addr
ip address
ip addr show
ip address show


```

### 更新系统
```shell
sudo dnf clean all
sudo dnf update
```

### 更新yum源
```shell
# 1、将源文件备份
cd /etc/yum.repos.d/ && mkdir backup && mv *repo backup/

# 2、下载阿里源文件
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-8.repo

#3、更新源里面的地址
sed -i -e "s|mirrors.cloud.aliyuncs.com|mirrors.aliyun.com|g " /etc/yum.repos.d/CentOS-*
sed -i -e "s|releasever|releasever-stream|g" /etc/yum.repos.d/CentOS-*

#4、生成缓存
yum clean all && yum makecache
```

### 安装vim
```shell
sudo dnf install vim
```
 - 将 VIM 编辑器设置为默认的系统范围编辑器
```shell
cat ～/etc/profile.d/vim.sh
export VISUAL="vim"
export EDITOR="vim"
```
- [vi操作](https://www.runoob.com/linux/linux-vim.html)

### 修改本地系统默认语言环境
- 查看本机语言包
```shell
locale -a
```
- 查看当前语言环境
```shell
localectl
```
- 修改语言环境
```shell
loclocalectl set-locale LANG=en_US.UTF-8alectl
```
- 下载语言包
```shell
# 下载单个语言包
dnf install glibc-langpack-en
# 下载全部语言包
dnf install glibc-all-langpacks -y
```
### 开启/关闭防火墙
```shell
# 1，查看防火墙状态
systemctl status firewalld.service


# 2，开启防火墙
systemctl start firewalld.service

# 3，关闭防火墙
systemctl stop firewalld.service

# 4，禁用防火墙
systemctl disable firewalld.service
```
### [添加和删除用户](https://www.myfreax.com/how-to-add-and-delete-users-on-centos-8/)
```shell
# 添加用户
sudo useradd daloong

# 给用户设置密码
sudo passwd daloong

# 在CentOS中默认情况下，wheel组成员具有sudo访问权限。如果要赋予新创建的用户具有sudo权限，需要将用户添加到wheel组中
sudo usermod -aG wheel daloong

# 删除用户
sudo userdel daloong
sudo userdel -r daloong #包括用户邮件和home目录

# 切换用户，使用root切换到其它账号无需密码，其它账号切换到root需要输入root密码
su - daloong

# 注销当前用户的登录状态
exit

# 查看一个用户的ID信息
id daloong

# 查看某一时刻用户的行为
w

# 查看当前已经登录的账号
who

# 查看当前用户的登录历史
last

# 查看系统中所有用户的最后一次登录时间、登录端口和来源IP
lastlog

# 使用如下命令打开 CentOS8用户组的配置文件
vi /etc/group

# 建立一个CentOS8用户组
groupadd users

# 删除一个用户组，初始组（主组）是不能删除的，只能删除附加组
groupdel users

# 为用户组users添加用户daloong
gpasswd -a users daloong

# 为CentOS8用户组users删除用户daloong
gpasswd -d users daloong
```

### 新建、重命名、删除文件夹、文件
- 新建文件夹
  - 格式：mkdir [选项] DirName
  - 命令中的［选项］一般有以下两种：
    -m 用于对新建目录设置存取权限，也可以用 chmod 命令进行设置。
    -p 需要时创建上层文件夹(或目录)，如果文件夹(或目录)已经存在，则不视为错误。
```shell

```

### 查找
- 查找微信小程序代码总行数：`find . "(" -name "*.json" -or -name "*.js" -or -name "*.wxss"  -or -name "*.wxml" ")" -print | xargs wc -l`