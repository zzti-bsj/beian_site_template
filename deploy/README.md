# 部署说明

## 部署架构

```
用户 → Nginx (80端口) → PM2 (3000端口) → 静态文件
```

## 部署步骤

### 1. 上传文件

```bash
scp -r 备案专用网站 root@your-server-ip:/root/
```

### 2. 启动 PM2

```bash
cd /root/备案专用网站
pm2 start "npx http-server . -p 3001" --name filing-site
pm2 startup && pm2 save
```

### 3. 配置 Nginx

```bash
# 修改 deploy/nginx.conf 中的域名
vi deploy/nginx.conf

# 复制配置
cp deploy/nginx.conf /usr/local/nginx/conf.d/filing-site.conf

# 重载 Nginx
/usr/local/nginx/nginx -t
/usr/local/nginx/nginx -s reload
```

## 常用命令

```bash
# PM2
pm2 status
pm2 logs filing-site
pm2 restart filing-site

# Nginx
/usr/local/nginx/nginx -t
/usr/local/nginx/nginx -s reload
```
