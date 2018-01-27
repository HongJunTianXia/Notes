* 下载并解压rpm-bundle.tar
* 将解压后的包进行安装<br/>

`mysql-community-common
 mysql-community-libs 
 mysql-community-client 
 mysql-community-server  
`
* 注意在这之前要安装 `numactl-libs`依赖, 'yum -y install numactl' 否则 `mysql server` 无法安装.
* `service mysqld restart` 重启mysql服务后, 'mysql -u root' 如果报错 `ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)' , 应修改 /etc/my.cnf ,添加
  `[mysqld]
  skip-grant-tables'后保存,`systemctl restart mysqld`重启mysql,即可解决问题.
