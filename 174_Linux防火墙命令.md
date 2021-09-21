```bash
systemctl status firewalld		 				#查看firewall防火墙状态
systemctl start firewalld.service			#打开firewall防火墙
systemctl stop firewalld.service			#关闭firewall防火墙
firewall-cmd --reload									#重启firewal防火墙
systemctl enable firewalld.service		#设置firewall防火墙开机自启
systemctl disable firewalld.service		#禁止firewall开机启动
firewall-cmd --list-ports							#查看firewall防火墙开放端口

firewall-cmd --zone=public --add-port=80/tcp --permanent 			#永久开放端口（需重启）
firewall-cmd --zone=public --remove-port=8081/tcp --permanent #永久移除端口（需重启）

firewall-cmd --zone=public --add-port=80/tcp									#临时开放端口（重启之前生效）
firewall-cmd --zone=public --remove-port=8081/tcp							#临时移除端口（重启之前生效）
		


命令含义:
–zone #作用域
–add-port=80/tcp #添加端口，格式为：端口/通讯协议
–permanent #永久生效，没有此参数重启后失效
```

