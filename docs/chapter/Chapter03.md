<!-- # 第 3 章 Linux平台下安装与配置MySQL -->

## 3.1、RPM文件安装

```shell
rpm -ivh mysql.rpm
```

## 3.2、二进制文件安装

```shell
groupadd mysql
useadd –g mysql mysql
tar –xzvf mysql.bin.tar.gz
scripts/mysql_install_db –user mysql
```

## 3.3、源码文件安装

```shell
groupadd mysql
useadd –g mysql mysql
tar –xzvf mysql.src.tar.gz
./configure –prfix=/usr/local/mysql
make
make install
```
