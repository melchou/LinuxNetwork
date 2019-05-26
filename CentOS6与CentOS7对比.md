CentOS6.x 与 CentOS7.x 对比

1.文件系统对比
	CentOS6.x : EXT4
		EXT4 的单个文件系统容量达到 1EB,单个文件大小则达到 16TB

	CentOS7.x : XFS
		XFS 默认支持 8EB 减1字节的单个文件系统，最大可支持的文件大小为 9EB,最大文件系统尺寸为18EB

2.防火墙、内核版本、默认数据库
	 CentOS6.x ： iptables、2.6.x-x、MySQL

	 CentOS7.x : firewalld、3.10.x-x、MariaDB

3.同步时间，修改时区，修改语言
	CentOS6.x ： ntpq -p , /etc/sysconfig/clock , /etc/sysconfig/il8n

	CentOS7.x : chronyc sources , timedatectl set-timezone Asia/Shanghai , localectl set-locale LANG=zh_CN.UTF-8

4.修改主机名
	CentOS6.x : /etc/sysconfig/network

	CentOS7.x : /etc/hostname
		同时CentOS7.x 还可以通过命令设置 ： hostnamectl set-hostname <hostname>

5.网络服务管理
	CentOS6.x : service 服务名 start|stop|restart|status(服务启动|关闭|重启|状态)
				service --status-all(查看所有服务状态)
				chkconfig 服务名 on|off(设置服务自启动|不自启动)
				chkconfig --list(查看所有服务自启动状态)

	CentOS7.x : systemctl start|stop|restart|status 服务名
				systemctl list-units(查看所有服务状态)
				systemctl enable|disable 服务名
				systemctl list-unit-files(查看所有服务自启动状态)


6.网络设置
	CentOS6.x : a.网卡名： eth0
				b.网络配置命令： ifconfig/setup
				c.网络服务： 默认使用 network 服务

	CentOS7.x : a.网卡名： ens33
				b.网络配置命令： ip/nmtui
				c.网络服务： 默认使用 NetworkManager 服务(network作为备用)


7.CentOS7.x 修改网卡名
	7.1. 修改网卡配置文件名(建议备份源文件)
		cp ifcfg-ens33 /root/
		mv ifcfg-ens33 ifcfg-eth0

	7.2. 修改网卡配置文件内容
		NAME=eth0
		DEVICE=eth0

	7.3. 修改grub配置文件
		vim /etc/default/grub
		GRUB_CMDLINE_LINUX 配置项添加 net.ifname=0 biosdevname=0

		GRUB_CMDLINE_LINUX="crashkernel=auto rd.lvm.lv=centos/root rd.lvm.lv=centos/swap rhgb quiet net.ifname=0 biosdevname=0"

	7.4. 更新grub配置文件，并加载新的参数
		grub2-mkconfig -o /boot/grub2/grub.cfg

	7.5. 重启操作系统
		reboot





