# CentOS Server 7.3

启用网卡(默认未开启)
```
# ip addr
# cat /etc/sysconfig/network-scripts/ifcfg-ens33
# sed -i "s/ONBOOT=no/ONBOOT=yes/g" /etc/sysconfig/network-scripts/ifcfg-ens33
# sed -i "s/SELINUX=enforcing/SELINUX=permissive/g" /etc/selinux/config
# reboot
```

关闭 selinux
```
# sed -i "s/SELINUX=enforcing/SELINUX=permissive/g" /etc/selinux/config
# reboot
# getenforce
```

安装基本工具
```
# yum install -y net-tools
# yum install -y epel-release
# yum install -y python-pip
# pip install -U pip
# pip -V
```

修改 pip 源配置
```
# mkdir ~/.pip
# tee ~/.pip/pip.conf <<-'EOF'
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
EOF
```


补充常用的工具（wget, vim）
```
# yum install -y wget vim-enhanced
```


更新源(官方源速度貌似还行，看着办吧)
```
# mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
# wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

安装 Linux Standard Base（可选）
```
# yum install -y redhat-lsb
# lsb_release -a
LSB Version:	:core-4.1-amd64:core-4.1-noarch:cxx-4.1-amd64:cxx-4.1-noarch:desktop-4.1-amd64:desktop-4.1-noarch:languages-4.1-amd64:languages-4.1-noarch:printing-4.1-amd64:printing-4.1-noarch
Distributor ID:	CentOS
Description:	CentOS Linux release 7.3.1611 (Core)
Release:	7.3.1611
Codename:	Core
```


安装docker

https://docs.docker.com/install/linux/docker-ce/centos/

```
# yum install -y yum-utils device-mapper-persistent-data lvm2
# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# ls /etc/yum.repos.d/docker-ce.repo
# yum install -y docker-ce
```

启动docker
```
# systemctl start docker
```

默认安装版本太低（废弃）

redis requires Ruby version >= 2.2.2.
```
# yum install -y redis
# redis-server -v
Redis server v=3.2.10 sha=00000000:0 malloc=jemalloc-3.6.0 bits=64 build=c8b45a0ec7dc67c6
# yum remove -y redis

# yum install -y ruby
# ruby -v
ruby 2.0.0p648 (2015-12-16) [x86_64-linux]
# yum remove -y ruby
```

安装依赖
```
# yum install -y git-core
# yum install -y gcc
# yum install -y bzip2
# yum install -y openssl-devel readline-devel zlib-devel
```

安装 rbenv
```
# git clone https://github.com/rbenv/rbenv.git ~/.rbenv
# echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
# ~/.rbenv/bin/rbenv init
# echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
# source ~/.bash_profile
```

安装插件 ruby-build
```
# mkdir -p "$(rbenv root)"/plugins
# git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
```

检查环境
```
# curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
```

安装 ruby
```
# rbenv install -l
# rbenv install 2.4.4
# rbenv global 2.4.4
# ruby -v
ruby 2.4.4p296 (2018-03-28 revision 63013) [x86_64-linux]
# rbenv version
2.4.4 (set by /root/.rbenv/version)
```