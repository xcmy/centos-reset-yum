# centos-reset-yum
### CentOS下重装yum

###### 1 - 卸载yum
```
rpm-qa | grep yum | xargs rpm -e--nodeps 
```

###### 2 - 查询rpm包并卸载

```
<!--查询相关包-->
rpm -qa |  grep yum
rpm -qa |  grep python-iniparse
rpm -qa |  grep python-urlgrabber

<!--删除命令-->
rpm -e + 包名

```

###### 3 - 查询rpm包下载
```
<!--查询系统版本-->
[root]# cat /etc/redhat-release
CentOS Linux release 7.6.1810 (Core)

<!--获取相关系统下的包-->
wget http://mirrors.163.com/centos/7.6.1810/os/x86_64/Packages/python-iniparse-0.4-9.el7.noarch.rpm
wget http://mirrors.163.com/centos/7.6.1810/os/x86_64/Packages/python-urlgrabber-3.10-9.el7.noarch.rpm
wget http://mirrors.163.com/centos/7.6.1810/os/x86_64/Packages/yum-3.4.3-161.el7.centos.noarch.rpm
wget http://mirrors.163.com/centos/7.6.1810/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
wget http://mirrors.163.com/centos/7.6.1810/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-50.el7.noarch.rpm
```
###### 4 - 安装rpm包

```
<!--和上一步在同层文件夹下-->
<!--因为有依赖关系，按如下顺序依次安装，最后两个必须一起安装-->

<!--执行-->
<!--安装失败可重新输入此命令并加参数--nodeps –force-->
rpm -ivh python-iniparse-0.4-9.el7.noarch.rpm
rpm -ivh python-urlgrabber-3.10-9.el7.noarch.rpm
rpm -ivh yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
rpm -ivh yum-3.4.3-161.el7.centos.noarch.rpm yum-plugin-fastestmirror-1.1.31-50.el7.noarch.rpm


<!--检测-->
<!--查询相关包-->
rpm -qa |  grep yum
rpm -qa |  grep python-iniparse
rpm -qa |  grep python-urlgrabber
```


###### 5 - yum配置

```
cd /etc/yum.repos.d/

<!--若有CentOS-Base.repo文件，则编辑-->
<!--若无CentOS-Base.repo文件，则获取并编辑-->

<!--获取-->
wget http://mirrors.163.com/.help/CentOS7-Base-163.repo

<!--1、把文件里面的$releasever全部替换为版本号，即7 最后保存-->
<!--2、把文件里面的$basearch全部替换为x86_64 最后保存。-->

mv  CentOS7-Base-163.repo CentOS-Base-163.repo

```


示例源文件
```
# CentOS-Base.repo
#
# The mirror system uses the connecting IP address of the client and the
# update status of each mirror to pick mirrors that are updated to and
# geographically close to the client.  You should use this for CentOS updates
# unless you are manually picking other mirrors.
#
# If the mirrorlist= does not work for you, as a fall back you can try the
# remarked out baseurl= line instead.
#
#

[base]
name=CentOS-$releasever - Base
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#released updates
[updates]
name=CentOS-$releasever - Updates
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```
改变后文件
```
# CentOS-Base.repo
#
# The mirror system uses the connecting IP address of the client and the
# update status of each mirror to pick mirrors that are updated to and
# geographically close to the client.  You should use this for CentOS updates
# unless you are manually picking other mirrors.
#
# If the mirrorlist= does not work for you, as a fall back you can try the
# remarked out baseurl= line instead.
#
#
[base]
name=CentOS-7 - Base - 163.com
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
baseurl=http://mirrors.163.com/centos/7/os/x86_64/
gpgcheck=1
gpgkey=http://mirrors.163.com/centos/RPM-GPG-KEY-CentOS-7

#released updates
[updates]
name=CentOS-7 - Updates - 163.com
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
baseurl=http://mirrors.163.com/centos/7/updates/x86_64/
gpgcheck=1
gpgkey=http://mirrors.163.com/centos/RPM-GPG-KEY-CentOS-7

#additional packages that may be useful
[extras]
name=CentOS-7 - Extras - 163.com
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras
baseurl=http://mirrors.163.com/centos/7/extras/x86_64/
gpgcheck=1
gpgkey=http://mirrors.163.com/centos/RPM-GPG-KEY-CentOS-7

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-7 - Plus - 163.com
baseurl=http://mirrors.163.com/centos/7/centosplus/x86_64/
gpgcheck=1
enabled=0
gpgkey=http://mirrors.163.com/centos/RPM-GPG-KEY-CentOS-7
```

###### 6 - 修改python路径（若报错）


如果遇到错误
```
There was a problem importing one of the Python modules
required to run yum. The error leading to this problem was:

   No module named yum

Please install a package which provides this module, or
verify that the module is installed correctly.

It's possible that the above module doesn't match the
current version of Python, which is:
2.6.6 (r266:84292, Feb 26 2019, 23:33:13)
[GCC 4.8.5 20150623 (Red Hat 4.8.5-36)]

If you cannot solve this problem yourself, please go to
the yum faq at:
  http://yum.baseurl.org/wiki/Faq

```
则检查python版本

```
<!--若以上安装完成后执行yum命令报错，检查python路径，保持统一-->

<!--python路径及版本-->
python
whereis python
which python

vim /usr/bin/yum

若版本不一致，则修改头部 usr/bin/python >> usr/bin/python+版本号 
```


