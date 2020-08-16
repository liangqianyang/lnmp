# 基于docker搭建的lnmp环境,含有nginx,php,mysql,redis,加入了redis,swoole,mongodb扩展

## 项目目录
    |-- lnmp   
        |-- .env
        |-- docker-composer.yml docker容器编排
        |-- mysql               mysql          
        |   |-- conf            配置文件目录
        |   |   |-- my.cnf
        |   |-- data            数据持久化目录
        |   |-- log             错误日志目录
        |-- nginx               nginx
        |   |-- conf            nginx 配置目录
        |   |   |-- nginx.conf
        |   |   |-- conf.d
        |   |       |-- default.conf
        |   |-- log             nginx 日志目录
        |       |-- error.log
        |       |-- nginx.access.log
        |       |-- nginx.error.log
        |-- php                 php
        |   |-- 7.4.8
        |       |-- Dockerfile  php编译文件
        |       |-- php.ini
        |       |-- sources.list 加速源
        |       |-- www.conf
        |-- redis
          |   |-- conf            配置文件目录
                |   |   |-- redis.conf
                |   |-- data            数据持久化目录
         |-- README.md            
                    
### 使用docker搭建的环境，可根据自己的环境修改相关的配置
### php使用的版本为7.4，安装了redis，swoole，composer等扩展
### .env文件中包含了mysql，nginx，php，redis的版本配置和conf文件，以及数据持久化的目录
 
    