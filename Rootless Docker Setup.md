# 首先使用sudo用户新建用户群组
```
sudo apt update
sudo apt install -y uidmap dbus-user-session
sudo adduser user1
sudo loginctl enable-linger dev_user1 # 可选，ssh断连后仍常驻后台
```

# 切换到普通用户模式
```
su - user1
curl -fsSL https://get.docker.com/rootless | sh # 内网执行时，可能需要提前export外网代理
```

# 安装完成后配置环境变量
```
echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc
echo 'export DOCKER_HOST=unix://$XDG_RUNTIME_DIR/docker.sock' >> ~/.bashrc
```

# 让环境变量立即生效
```
source ~/.bashrc
```

# 启动 Docker 并设置开机自启
```
systemctl --user start docker
systemctl --user enable docker
```

# 验证 docker 是否正常运行，内网可能无法拉取docker镜像
```
docker run hello-world
```
