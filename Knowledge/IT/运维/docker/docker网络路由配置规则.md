文章标签： `docker 运维` `容器`
docker 默认的网络段是 172.17.0.0/16 网段，生产环境中部署docker可能会与现网IP冲突。可以通过修改docker默认网络地址解决地址冲突的问题。

查看路由配置规则
~~~
ip route
ip route ls 
~~~

1、停止所有容器
```
docker rm $(docker ps -a -q)
```

2、停止docker
```
systemctl stop docker
```

3、修改daemon.json文件
修改/etc/docker/daemon.json（daemon.json没有就新建）文件，加入以下代码，按需修改172.30.0.0/16网络地址。

- default-address-pools 用于设置 IP 地址池范围，格式为数组，可指定多个 IP 地址池范围；
- base 用于指定 IP 地址池的范围，格式为 CIDR 格式；
- size 用于指定分配子网的掩码长度
~~~
{
   "default-address-pools":[
       {"base":"172.30.0.0/16","size":24}
   ]
}
~~~

4、重新加载配置，并启动docker
```
systemctl daemon-reload
systemctl restart docker
```