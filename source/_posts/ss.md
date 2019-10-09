title: psql 导出数据库操作时认证失败
author: 黄东杰
date: 2019-03-08 02:05:26
tags:
---
使用 `pg_dump -b dbname -U username -f test.sql` 导出数据库数据时遇到问题：

```
pg_dump: [archiver (db)] connection to database "*" failed: FATAL:  Peer authentication failed for user "*"
```

是因为 psql 默认的密码认证方式所导致的，解决方式有多种，我个人用的是直接修改密码认证方式的办法，步骤如下：

 <!--more-->
 
编辑 psql 配置文件

`sudo vim /etc/postgresql/[psql 版本号]/main/pg_hba.conf` 找到下方内容所在的行
    
    
```bash
# Database administrative login by Unix domain socket
local   all             postgres                                md5
```
    
这一行的意思是只有 postgres 用户在本地登录数据库时才会使用 md5 的方式验证，所谓的 md5 方式就是登录时必须填写密码，然后用 md5 摘要算法验证，简单理解就是我们常用的账户名 + 密码的认证方式

此外还有另外两种验证方式：
    
peer：即信任当前的系统用户，当使用当前的系统用户登录进数据库时不需要认证，如果我们要用别的账户登录进数据库时就会认证错误。
    
trust：永远相信任何用户的连接，即所有用户登录都不需要认证
    
我们这里将 postgres 改成 all，即所有用户都需要进行密码认证的方式
    
```bash
# Database administrative login by Unix domain socket
local   all             all                                    md5
```
    
然后重启 psql： `service postgresql restart`
    
ref：https://gist.github.com/AtulKsol/4470d377b448e56468baef85af7fd614