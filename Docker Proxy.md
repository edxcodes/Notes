# 配置 Docker 代理
在内网部署Docker时，无法直接使用export挂外网代理用于拉取Docker镜像，需要做如下配置：

```
systemctl --user edit docker.service
EDITOR=vim systemctl --user edit docker.service # 二选一
```

# 在配置中输入代理

```
[Service]
Environment="HTTP_PROXY=http://a00123123:12345678@proxyhk.huawei.com:8080"
Environment="HTTPS_PROXY=http://a00123123:12345678@proxyhk.huawei.com:8080"
Environment="NO_PROXY=localhost,127.0.0.1,docker-registry.somecorporation.com"

```

# 重启并确认代理已正确配置

```
systemctl --user daemon-reload # 扫描新文件
systemctl --user restart docker # 重启
docker info | grep Proxy # 返回应能看到 Proxy 相关内容
```

# 如遇网络与证书信任问题，需配置信任注册表`~/.config/docker/daemon.json`

```
{
  "insecure-registries" : ["docker.io", "registry-1.docker.io", "production.cloudflare.docker.com"]
}
```

# 重启服务

```
systemctl --user restart docker
```

# 验证 Docker 配置已正确配置

```
docker run hello-world
```
