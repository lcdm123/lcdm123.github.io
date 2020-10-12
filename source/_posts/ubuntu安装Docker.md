---
title: ubuntu安装Docker
date: 2020-10-12 12:36:18
tags:
---

# 安装

```y
sudo apt-get update
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
 
# step 2: 安装GPG证书
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
 
# Step 3: 写入软件源信息
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
 
# Step 4: 更新并安装Docker-CE
sudo apt-get -y update
sudo apt-get -y install docker-ce
```

#设置开机自启

### docker开机自启：sudo systemctl enable docker.service

### 容器开机自启：启动容器时加上 --restart=always

<!-- more -->

# 换源

```y
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://xx0cvw91.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```