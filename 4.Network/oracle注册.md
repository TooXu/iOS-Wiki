## SOP
1. 注册 [oracle](https://www.oracle.com/cn/cloud/free/?source=:ow:o:p:nav:081520OCIHeroCallout_cn&intcmp=:ow:o:p:nav:081520OCIHeroCallout_cn)
2. 创建实例
3. 通过 https://tool.chinaz.com/port 测试 22 端口是否可用 不可用使用 附加 vnic 更换 ip
4. 同时生成 SSH 密钥
5. 保存 私钥 
6. sudo chmod 400 ssh-kye.key
7. ssh -i ssh-key.key username@ip 登录主机
8. sudo -i 获得权限
9. V2Ray
10. 关闭 防火墙

```
bash <(curl -s -L https://git.io/v2ray.sh)
```

[V2Ray 脚本](https://www.itblogcn.com/article/406.html)

---------

## V2Ray搭建

**安装命令：**

ip 不可用时,通过vpc更新 ip
不要使用一键脚本 ，手动一步一步安装

```bash
bash <(curl -s -L https://git.io/v2ray.sh)
```

## SSR

----

1. 执行脚本 安装 ssr

```shell
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh

chmod +x shadowsocks-all.sh

./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

## 安装 warp 支持 ChatGPT
```shell
bash <(curl -fsSL git.io/warp.sh) d
```



8. 开放端口

Ubuntu系统

开放所有端口

```shell
iptables -P INPUT ACCEPT \n
iptables -P FORWARD ACCEPT \n
iptables -P OUTPUT ACCEPT \n
iptables -F \n
```

Ubuntu镜像默认设置了Iptable规则，关闭它

```
apt-get purge netfilter-persistent
reboot
```

或者强制删除

```
rm -rf /etc/iptables 
reboot
```

## 开启 bbr

```shell
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
```


[2021年注册永久免费甲骨文云Oracle Cloud并创建免费实例最全攻略](https://xunihao.net/867.html)

