Ansible for Gost Server
=========================

[ansible](https://www.ansible.com/) 是 RedHat 开发的批量自动化管理服务器的工具，无需在被管理的服务器上安装客户端，它通过 SSH 登录服务器进行操作。

## 系统约定

* 所有被管理的服务器都是 CentOS 7.x 系统
* 默认的管理员账户为 sysops，具有 sudo 权限
* root 账户禁止 SSH 登录(暂未设置)
* 禁止使用密码 SSH 登录，强制使用密钥登录

## 配置约定规范

| 规范 | 内容 |
| --- | --- |
| 应用及配置目录 | /data/projects |
| 日志目录 | /data/var/log |
| 数据目录 | /data/var/lib |
| 基础服务的配置目录 | /data/etc |
| 主机名称 | {地区}-{机房}-{用途}-{内网IP结尾} |

## 规范字典

地区字典

| 键 | 值 |
| --- | --- |
| la | 洛杉矶 |
| hk | 香港 |

机房字典

| 键 | 值 |
| --- | --- |
| bwg | 搬瓦工 |
| hd | hostdare |
| gcp | 谷歌云 |

用途字典

| 键 | 值 |
| --- | --- |
| ops | 开发运维 |
| srv | 应用服务 |

## 安装 ansible 执行环境

1. 准备好 Python3 环境，为 ansible 建立一个 virtualenv
2. 安装 ansible， `pip install ansible`
3. 安装 sshpass，这样在 ansible 连接服务器时会允许输入 SSH 密码，一般在第一次系统初始化时，需要输入 root 的密码，在初始化完成后，则可以使用 ssh-key 登录服务器

## 使用说明

### 初始化

```sh
ansible-playbook provisioning.yml -i environments/prod -u root --ask-pass
```

### 安装 docker 服务

```sh
ansible-playbook basic-service.yml -i environments/prod -u sysops --become --key-file=files/ssh-key/ssh-key -t remove_docker,install_docker
```

### 安装 Mysql

安装

```sh
ansible-playbook basic-service.yml -i environments/prod -u sysops --become --key-file=files/ssh-key/ssh-key -t install_mysql
```

### 安装 Redis

安装

```sh
ansible-playbook basic-service.yml -i environments/prod -u sysops --become --key-file=files/ssh-key/ssh-key -t rc-local,install_redis
```

### 安装 RabbitMQ

安装

```sh
ansible-playbook basic-service.yml -i environments/prod -u sysops --become --key-file=files/ssh-key/ssh-key -t install_rabbitmq
```


### 一次安装多个服务

安装

```
ansible-playbook basic-service.yml -i environments/prod -u sysops --become --key-file=files/ssh-key/ssh-key -t rc-local,install_mysql,install_redis,install_rabbitmq

```
