# shell-scripts
Linux Shell Scripts

#OpenVZ平台Google BBR一键安装脚本,注意：安装失败的话，可能后台没有开启TUN/TAP
使用方法，已测试通过的系统：Ubuntu 14.04 x64、Ubuntu 16.04 x64、CentOS 6 x64、CentOS 7 x64只支持64位系统，要求glibc版本2.14以上。

wget --no-check-certificate https://raw.githubusercontent.com/kuoruan/shell-scripts/master/ovz-bbr/ovz-bbr-installer.sh

chmod +x ovz-bbr-installer.sh

./ovz-bbr-installer.sh
#判断bbr是否正常启动可以尝试ping 10.0.0.2，如果能通，说明bbr已经启动。
多端口加速
安装的时候只配置了一个加速端口，但是你可以配置多端口加速，配置方法非常简单。修改文件
# vi /usr/local/haproxy-lkl/etc/port-rules
在文件里添加需要加速的端口，每行一条，可以配置单个端口或者端口范围，以#开头的行将被忽略。 例如：8800或者8800-8810配置完成之后，只需要重启haproxy-lkl即可。
注：最初版本的实现是需要再开一个新端口，后来经人提醒，我又看了一下HAproxy的配置说明，可以直接代理后端端口，不必再开新端口。请注意，使用该方法后，如果HAproxy进程异常退出，会造成无法连接原有端口。所以，请确保在退出 HAproxy时是通过命令正常退出的，在退出时会自动清理原有的防火墙规则。
使用systemctl或者service命令来启动、停止和重启HAporxy-lkl：

systemctl {start|stop|restart} haproxy-lkl
service haproxy-lkl {start|stop|restart}
/usr/local/haproxy-lkl/etc/haproxy.cfg这个文件是通过port-rules自动生成的，每次启动都会重新生成，所以直接修改它的配置没用。 如果想要自定义配置，请修改启动文件：/usr/local/haproxy-lkl/sbin/haproxy-lkl
