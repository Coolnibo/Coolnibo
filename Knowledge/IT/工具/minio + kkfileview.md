
# 部署

## Docker部署kkfileview

### kkfileview-base
制作kkfileview基础运行环境，里面装着office的工具，是必须要具备的，提前做好。


```Dockerfile
FROM ubuntu:24.04  
  
RUN sed -i 's@//.*archive.ubuntu.com@//mirrors.aliyun.com@g' /etc/apt/sources.list.d/ubuntu.sources &&\  
    sed -i 's@//security.ubuntu.com@//mirrors.aliyun.com@g' /etc/apt/sources.list.d/ubuntu.sources &&\  
    sed -i 's@//ports.ubuntu.com@//mirrors.aliyun.com@g' /etc/apt/sources.list.d/ubuntu.sources &&\  
    apt-get update &&\  
    export DEBIAN_FRONTEND=noninteractive &&\  
    apt-get install -y --no-install-recommends openjdk-8-jre tzdata locales xfonts-utils fontconfig libreoffice-nogui &&\  
    echo 'Asia/Shanghai' > /etc/timezone &&\  
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime &&\  
    localedef -i zh_CN -c -f UTF-8 -A /usr/share/locale/locale.alias zh_CN.UTF-8 &&\  
    locale-gen zh_CN.UTF-8 &&\  
    apt-get install -y --no-install-recommends ttf-mscorefonts-installer &&\  
    apt-get install -y --no-install-recommends ttf-wqy-microhei ttf-wqy-zenhei xfonts-wqy &&\  
    apt-get autoremove -y &&\  
    apt-get clean &&\  
    rm -rf /var/lib/apt/lists/*  
  
# 内置一些常用的中文字体，避免普遍性乱码  
ADD fonts/* /usr/share/fonts/chinese/  
  
RUN cd /usr/share/fonts/chinese &&\  
    # 安装字体  
    mkfontscale &&\  
    mkfontdir &&\  
    fc-cache -fv  
  
ENV LANG=zh_CN.UTF-8 LC_ALL=zh_CN.UTF-8
```

#### x86 
```shell  
docker build --tag keking/kkfileview-base:4.4.0 .
```

#### arm/amd



## 前端项目接入 kkfileview

```js
window.open(kkfilew服务地址 + 可以访问的minio地址)
```


## nginx代理

