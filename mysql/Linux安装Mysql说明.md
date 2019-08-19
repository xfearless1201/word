# Linux安装Mysql说明

## 下载Mysql
[官网]下载linux系统使用的mysql,版本号5.6,下载地址[https://dev.mysql.com/downloads/mysql/5.6.html#downloads] [[点击下载]](https://dev.mysql.com/downloads/mysql/5.6.html#downloads "[点击下载]")

## 上传Mysql安装安装包到linux服务器
*使用rz命令添加mysql安装包，如果使用rz命令提示-bash: rz: command not found时说明没有安装rz命令，通过[yum -y install lrzsz]命令进行安装*

解压安装到 usr/local

    命令:tar -zxvf mysql-5.6.45-linux-glibc2.12-x86_64.tar.gz -C /usr/local/
    
    解压后地址:/usr/local/mysql-5.6.45-linux-glibc2.12-x86_64
    
    修改/usr/local/mysql-5.6.45-linux-glibc2.12-x86_64 为 usr/local/mysql.
    
    命令：mv mysql-5.6.45-linux-glibc2.12-x86_64/ mysql

#### 创建mysql用户组及用户
    groupadd mysql
    useradd -r -g mysql mysql

#### 进入到mysql目录，执行添加MySQL配置的操作

    cp support-files/my-medium.cnf /etc/my.cnf
    或：
    cp support-files/my-default.cnf /etc/my.cnf
	是否覆盖？按y 回车

#### 编辑/etc/my.cnf文件
    
    vi /etc/my.cnf

#### my.cnf文件
    [mysqld]
    
    # Remove leading # and set to the amount of RAM for the most important data
    # cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
    # innodb_buffer_pool_size = 128M
    
    # Remove leading # to turn on a very important data integrity option: logging
    # changes to the binary log between backups.
    # log_bin
    
    # These are commonly set, remove the # and set as required.
    basedir = /usr/local/mysql
    datadir = /usr/local/mysql/data
    port = 3306
    # server_id = .....
    socket = /tmp/mysql.sock
    character-set-server=utf8
    skip-name-resolve
    log-err=/usr/local/mysql/data/error.log
    pid-file=/usr/local/mysql/data/mysql.pid
####修改mysql的访问权限（注意后面的小点，表示当前目录）
*mysql所在路径：/usr/local/mysql*
命令输入目录如：*/usr/local*

    命令：
    chown -R mysql .
    chgrp -R mysql .
    scripts/mysql_install_db --user=mysql 
    ##提示-bash: scripts/mysql_install_db: /usr/bin/perl: bad interpreter: No such file or directory
    输入命令:yum -y install perl perl-devel 进行安装
    chown -R root .

	进入目录:usr/local/mysql
    chown -R mysql:msyql data
#### 初始化数据（在mysql/bin或者mysql/scripts下有个 mysql_install_db 可执行文件初始化数据库），进入mysql/bin或者mysql/scripts目录下，执行下面命令

    ./mysql_install_db --verbose --user=root --defaults-file=/etc/my.cnf --datadir=/usr/local/mysql/data --basedir=/usr/local/mysql --pid-file=/usr/local/mysql/data/mysql.pid --tmpdir=/tmp

如果执行上面的命令提示:
    FATAL ERROR: please install the following Perl modules before executing ./scripts/mysql_install_db:
    Data::Dumper 错误时

通过命令：yum -y install autoconf 来解决此问题