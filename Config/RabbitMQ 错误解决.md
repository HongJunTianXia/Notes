## 安装好mq后，查看状态，错误
```
S D:\RabbitMQ Server\rabbitmq_server-3.7.0\sbin> .\rabbitmqctl status  
Status of node rabbit@DESKTOP-ECFDCQB ...  
Error: unable to perform an operation on node 'rabbit@DESKTOP-ECFDCQB'. Please see diagnostics information and suggestions below.  
  
Most common reasons for this are:  
  
 * Target node is unreachable (e.g. due to hostname resolution, TCP connection or firewall issues)  
 * CLI tool fails to authenticate with the server (e.g. due to CLI tool's Erlang cookie not matching that of the server)  
 * Target node is not running  
  
In addition to the diagnostics info below:  
  
 * See the CLI, clustering and networking guides on http://rabbitmq.com/documentation.html to learn more  
 * Consult server logs on node rabbit@DESKTOP-ECFDCQB  
```
## 解决办法:
将C:\Users\tracyclock\.erlang.cookie 文件拷贝到C:\Windows\System32\config\systemprofile替换掉.erlang.cookie文件
