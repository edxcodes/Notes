# 配置 Docker 代理
在内网部署Docker时，无法直接使用export挂外网代理用于拉取Docker镜像，需要做如下配置：

```
systemctl --user edit docker.service
# 以上默认使用 nano 编辑，可选择使用 vim 编辑
EDITOR=vim systemctl --user edit docker.service
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
