外网访问

如果浏览器中访问http://localhost:9200/没有返回预期的结果，就需要修改Elasticsearch的配置，使其支持外网访问。

首先，按Ctrl +C停止Elasticsearch

然后，打开Elasticsearch的配置文件vimconfig/elasticsearch.yml

找到network.host这一行。

将该行最前面的#去掉，修改成network.host:  0.0.0.0修改之后

按Esc，再按:wq保存并退出编辑elasticsearch配置文件

接着，重新运行./bin/elasticsearch

在浏览器中，访问http://xxxx:9200/（xxxx是运行elasticsearch的服务器的ip地址），你就能看到成功的信息啦。



（如果启动失败，报错）
ElasticSearch启动报错，bootstrap checks failed
修改elasticsearch.yml配置文件，允许外网访问。

vim config/elasticsearch.yml
# 增加

network.host: 0.0.0.0

启动失败，检查没有通过，报错

[2018-05-18T17:44:59,658][INFO ][o.e.b.BootstrapChecks    ] [gFOuNlS] bound or publishing to a non-loopback address, enforcing bootstrap checks
ERROR: [2] bootstrap checks failed
[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]

[2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

 

 

[1]: max file descriptors [65535] for elasticsearch process is too low, increase to at least [65536]

编辑 /etc/security/limits.conf，追加以下内容；
* soft nofile 65536
* hard nofile 65536
此文件修改后需要重新登录用户，才会生效

 

[2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

编辑 /etc/sysctl.conf，追加以下内容：
vm.max_map_count=655360
保存后，执行：

sysctl -p

重新启动，成功。

参考链接：https://www.cnblogs.com/yehui/p/9087845.html

#kibaba 外网访问
kibana安装后外网无法访问： 
修改config/kibaba.yml下的server.host为0.0.0.0

启动kibana后关闭shell窗口后kibana自动关闭的解决办法”: 
主要涉及到启动kibana后关闭shell窗口后kibana自动关闭的解决办法方面的内容，对于启动kibana后关闭shell窗口后kibana自动关闭的解决办法感兴趣的同学可以参考一下。

后台启动kibana(加上&) 
kibana-4.5.2-linux-x64/bin/kibana &

注意：这时加上了&虽然执行了后台启动，但是还是有日志打印出来，使用ctrl+c可以退出。 
但是如果直接关闭了Xshell,这时服务也会停止，访问http://yourip:5601就失败了。

解决方法： 
执行了kibana-4.5.2-linux-x64/bin/kibana &命令后，不使用ctrl+c去退出日志， 
而是使用exit;这样即使关闭了shell窗口kibana服务也不会挂了。
--------------------- 
作者：u010466329 
来源：CSDN 
原文：https://blog.csdn.net/u010466329/article/details/78594777 

