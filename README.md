# Ansible-V2Ray-VMess-WebSocket-TLS-Web

使用Ansible快速架设VMess-WebSocket-TLS协议的V2Ray代理服务器, 使用Caddy实现.

本项目中的Playbook会在服务器上安装V2Ray、Caddy以及其依赖关系包，并且部署一个VMess协议，使用WebSocket和TLS的代理服务器，也可以套CDN使用。

# 快速上手

## 推荐使用的操作系统

* CentOS Stream 8/9
 
* Alma Linux 8/9

* Rocky Linux 8/9

1. 安装需要用到的软件包

```
dnf install git epel-release -y
dnf install ansible -y
```

2. 获取Ansible Playbook

```
git clone https://github.com/ZhaoKunqi/ansible-v2ray-vmess-ws-tls-caddy.git
```

3. 进入项目的目录

```
cd ansible-v2ray-vmess-ws-tls-caddy
```

4. 修改配置文件
```
vi config.yml
```

5. 编辑以下内容

```
# 已经解析到本机IP(VPS的IP地址)的域名
custom_domain_name: 这里写你的域名,例如 genshin.hoyoverse.com

# 自定义转发路径，随便写一个即可
custom_path: 'wanyuanshenwande'

# VMess的UUID(或密码)，可以使用uuidgen命令生成，也可以使用https://www.uuidgenerator.net/生成
uuid: '640b714c-9f02-4234-9d16-c1a1464f1385'
```

保存退出以后，运行Ansible Playbook

强制打开Firewalld和SELinux的playbook

```
ansible-playbook site.yml
```

如果不需要额外安全性或者VPS服务商提供的VPS是魔改过的镜像，安全功能完整的Ansible Playbook可能无法运行。

若无法使用的情况下，可以选择plain.yml这个playbook来执行，但会有安全风险请注意。

```
ansible-playbook plain.yml
```

如果Ansible正常运行完，那说明Caddy服务器和V2Ray服务器都部署好了，您可以尝试连接。

# 待办事项

## 通过Ansible新版本特性将site.yml和plain.yml合为同一个Playbook方便使用。

# 更新日志

## 2024年11月7日：

更新了文档与使用说明

## 2024年10月24日: 

改善了安全性，现在无论VPS厂商是否删除/关闭firewalld，都会重新加载firewalld并配置好相关防火墙规则。

改善了安全性，现在无论VPS厂商是否禁用/启用selinux，都会尽量将selinux设置为enforcing安全模式。

降低了复杂度，现在会使用Caddy来管理ACME/反向代理/伪装页面。

降低了复杂度，现在可以直接使用CloudFlare小黄云(CloudFlare Proxy)的解析记录，避免了潜在的IP地址泄露。

## 2023年10月11日: 

有一些VPS厂商会默认删除VPS镜像中的防火墙并且设置SELinux为不激活的状态来减少用户可能遇到的麻烦，

在这种被二次定制过的系统中运行时会找不到firewalld服务，也无法改成permissive模式，于是对site.yml进行更新，使其支持在这种环境下部署。

1. 修复了在已经将firewalld.service删除掉的VPS上运行时报错的问题
 
2. 修复了在已经将SELinux设置为disabled模式的VPS上运行时报错的问题
