# docker
some docker kills


# 常见问题解决方法
## 一. 国内下载docker镜像网速很慢,可以使用阿里的docker加速器

您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器：
创建docker文件夹
```
sudo mkdir -p /etc/docker
```

然后编辑
```
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://cu2hdwk5.mirror.aliyuncs.com"]
}
EOF
```
配置文件重新加载
```
sudo systemctl daemon-reload
```
重启docker
```
sudo systemctl restart docker
```



## 二.DOCKER 给运行中的容器添加映射端口
1. 获得容器IP
```
docker inspect container_name | grep IPAddress
```
2.  iptable转发端口
 例如将容器的8000端口映射到Docker主机的8001端口
```
iptables -t nat -A  DOCKER -p tcp --dport 8001 -j DNAT --to-destination 172.17.0.19:8000
```
172.17.0.19 为该docke容器ip

## 三.docker建立容器时候设置log
容器的日志支持多种方式，可以通过配置--log-driver=VALUE来选择不同的驱动方式，
包含none, json-file,syslog,journald,gelf,fluentd,awslogs,splunk,etwlogs,gcplogs。
1. none: 顾名思义不会做任何日志输出
2. json-file: 把标准输出日志通过以json格式文件的方式展示出来，这也是docker默认的日志输出方式。
```
--log-opt max-size=[0-9+][k|m|g] #文件的大小
--log-opt max-file=[0-9+] #文件数量
--log-opt labels=label1,label2 #加入label参数
--log-opt env=env1,env2 #加入env参数
```



## 四. 安装docker compose
