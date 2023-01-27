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

不要使用一键脚本 ，手动一步一步安装

```bash
bash <(curl -s -L https://git.io/v2ray.sh)
```

详细安装教程：

https://github.com/233boy/v2ray/wiki/V2Ray%E6%90%AD%E5%BB%BA%E8%AF%A6%E7%BB%86%E5%9B%BE%E6%96%87%E6%95%99%E7%A8%8B

显示一下信息代表安装成功（可直接用以下配置进行连接）(以下配置在链接时使用)：

```
---------- V2Ray 配置信息 -------------

 地址 (IP Address) = 141.*.*.*

 端口 (Port) = 8888

 用户ID (User ID / UUID) = 38b272ba-8a91-*-*-*

 额外ID (Alter Id) = 0

 传输协议 (Network) = tcp

 伪装类型 (header type) = none

---------- END -------------

V2Ray 客户端使用教程: https://git.io/v2ray-client

提示: 输入 v2ray url 可生成 vmess URL 链接 / 输入 v2ray qr 可生成二维码链接

---------- V2Ray vmess URL / V2RayNG v0.4.1+ / V2RayN v2.1+ / 仅适合部分客户端 -------------

vmess://ewoidiI6I*****g==
```

**配置文件要注意(建议直接复制安装结果中 vmess://\**\** 地址，直接导入，避免自己填配置出错)：**

> Network(传输协议)： tcp
> type(伪装类型)：none
> 不对的话连不上！！！

好了到这里我们就搭建成功了(*^▽^*)

**相关命令：**

> `v2ray info` 查看 V2Ray 配置信息
> `v2ray config` 修改 V2Ray 配置
> `v2ray link` 生成 V2Ray 配置文件链接
> `v2ray infolink` 生成 V2Ray 配置信息链接
> `v2ray qr` 生成 V2Ray 配置二维码链接
> `v2ray ss` 修改 Shadowsocks 配置
> `v2ray ssinfo` 查看 Shadowsocks 配置信息
> `v2ray ssqr` 生成 Shadowsocks 配置二维码链接
> `v2ray status` 查看 V2Ray 运行状态
> `v2ray start` 启动 V2Ray
> `v2ray stop` 停止 V2Ray
> `v2ray restart` 重启 V2Ray
> `v2ray log` 查看 V2Ray 运行日志
> `v2ray update` 更新 V2Ray
> `v2ray update.sh` 更新 V2Ray 管理脚本
> `v2ray uninstall` 卸载 V2Ray

**vmess协议配置**

查看配置文件(**该配置在后面链接时使用**)：

```
cat /etc/v2ray/config.json
```

[![14.png](https://www.itblogcn.com/wp-content/uploads/2020/04/img_5ea11229bca76.png)](https://www.itblogcn.com/wp-content/uploads/2020/04/img_5ea11229bca76.png)

----

1. 执行脚本 安装 ssr

```shell
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh

chmod +x shadowsocks-all.sh

./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

8. 开放端口

Ubuntu系统

开放所有端口

```
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
iptables -F
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

开启 bbr

```shell
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
```



[2021年注册永久免费甲骨文云Oracle Cloud并创建免费实例最全攻略](https://xunihao.net/867.html)

