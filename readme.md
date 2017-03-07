[TOC]

*此容器组支持创建laravel项目*

如果您喜欢该项目，请给一个 **star**
# 谢谢 `:)`
# 欢迎各位的 `PR`

**如果您遇到了问题，请提交 `issue` 以及遇到问题之前您执行过什么步骤，帮助我重现 `it` 并 `Fix it`. 我在此对您表示感谢**`:)`

**如果您有什么新的想法，欢迎在 `issue` 中告知我，也欢迎您贡献您的代码`:)`**

## 目录结构

```
.
|-- app 网站目录
|	|-- index.php 首页
|-- data 数据存储目录
|	|-- mysql mysql 数据存储目录
|	|-- redis redis 数据存储目录
|-- docker-compose.yml 容器组管理文件
|-- images 构建文件
|	|-- console 网站管理容器，多数工具在此容器中
|	|	|-- composer composer 文件
|	|	|-- Dockerfile console 容器构建文件
|	|-- nginx 网站服务提供容器
|	|	|-- sites 网站站点文件
|	|-- php-fpm php 文件处理容器
|	|	|-- conf php 的一些自定义文件
|	|	|Dockerfile php-fpm 容器构建文件
|-- logs  日志目录
|	|-- nginx nginx 的日志
|    	|-- access.log nginx的访问日志
|    	|-- error.log nginx的错误日志
|__ readme.md readme 文件
```

## 使用


### 1. 克隆该项目
`git clone https://github.com/mouyong/docker.git`

### 2. 进入项目目录
`cd docker/web`


### 3. 启动各个容器
`docker-compose up -d`

当容器组启动完成，在浏览器中输入 `http://localhost` 可以看到 php 的相关信息。

### 4. 进入容器
`docker-compose exec 容器服务名 bash` 即可进入相应容器。

例如：
进入 console 容器 `docker-compose exec console bash`

### 创建 laravel 项目
#### 1. 进入 console 容器 
`docker-compose exec console bash`

#### 2. `composer create-project --prefer-dist laravel/laravel 项目名` or `laravel new blog`

例如:
创建一个博客项目
`composer create-project --prefer-dist laravel/laravel blog`
or
`laravel new blog`

#### 3. 进入项目
`cd 项目名`

例如:
`cd blog`

#### 4. 授予相应目录权限
`chmod 777 -R bootstrap/cache storage`

#### 5. 如果您看到了 laravel ，欢呼吧`：）`恭喜您，您的项目创建完成。

注意：
**php 的错误日志查看方式如下：**

`docker-compose logs php-fpm`

**nginx 日志内容记录在** `logs/nginx/`目录中

## web 容器组

### 1. console

`容器中已包含 git、composer、redis 等工具`
该容器普通用户默认密码为 `root` ，您可以在 `docker-compose.yml` 文件的 `console` 容器中进行修改。
  
  管理项目的容器：
   1. 可在此容器中直接修改网站配置。网站配置在 `/etc/nginx/conf.d/` 目录下。
   2. 可在此容器中直接操作 redis 容器中储存的数据。直接在命令行中输入 `redis-cli` 即可。`alias`中已配置 `redis-cli -h xxx`。
   3. 可在此容器中找到 mysql 容器的数据卷，数据在 `/var/lib/mysql/` 目录下。
   4. 可在此容器中找到 redis 容器的数据卷，数据在 `/data/` 目录下。
   5. `3` 和 `4` 好像没什么用处。唔，，，你可以直接在此容器中进行数据备份，至于如何备份，** depends on you. **

### 2. php-fpm

  提供 php 文件解析

### 3. nginx
  
  提供 web 访问。
  
  网站配置文件在 `images/nginx/sites/` 目录下。如需添加虚拟站点，请拷贝一份，然后修改对应的 `server_name`、`root`，并将你的域名添加至宿主机的host文件。

### 4. mysql
  
  提供数据储存，数据储存在 `data/mysql/` 目录下。
  
### 5. redis

  提供数据缓存，数据储存在 `data/redis` 目录下。
