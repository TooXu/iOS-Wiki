1. 注册 [oracle](https://www.oracle.com/cn/cloud/free/?source=:ow:o:p:nav:081520OCIHeroCallout_cn&intcmp=:ow:o:p:nav:081520OCIHeroCallout_cn)
2. 创建实例
3. 同时生成 SSH 密钥
4. 保存 私钥 
5. ssh -i ssh-key.key username@ip 登录主机
6. sudo -i 获得权限
7. 执行脚本 安装 ssr

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
rm -rf /etc/iptables &amp;&amp; reboot
```



[2021年注册永久免费甲骨文云Oracle Cloud并创建免费实例最全攻略](https://xunihao.net/867.html)

