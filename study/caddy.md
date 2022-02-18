# caddy 入门 ubuntu 18/20

### 安装 https://caddyserver.com/docs/install
```bash
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo tee /etc/apt/trusted.gpg.d/caddy-stable.asc
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```

### 默认配置文件路径
/etc/caddy/Caddyfile

### caddy 反向代理示例
```nginx
sample.caddy.com {
  reverse_proxy 127.0.0.1:8080
}
```
等效 nginx 配置
```nginx
http {
  upstream sample.caddy.com {
    server 127.0.0.1:8080;
  }

  server {
    listen       80;
    server_name  sample.caddy.com;

    location / {
      proxy_pass http://sample.caddy.com;
      proxy_set_header Host $host;
      proxy_redirect off;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
  }

  server {
    listen       443;
    server_name  sample.caddy.com;

    # 省略 ssl 等配置一大堆
  }
}
```

### 常用命令 https://caddyserver.com/docs/running
```bash
# 启动
systemctl enable --now caddy
# 查看状态
systemctl status caddy
# 重载配置文件
systemctl reload caddy
# 停止
systemctl stop caddy
```
