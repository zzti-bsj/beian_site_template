# 网站备案个人主页部署说明

这是一个纯静态的个人主页，用于网站备案使用。

## 📁 文件结构

```
备案专用网站/
├── index.html          # 主页面
├── style.css           # 样式文件
└── deploy/             # 部署配置文件
    ├── nginx.conf      # Nginx 配置示例
    ├── ecosystem.config.json  # PM2 配置示例
    └── README.md       # 本文档
```

## 🚀 部署方式

### 方式一：使用 Nginx 部署（推荐）

1. **安装 Nginx**
   ```bash
   # Ubuntu/Debian
   sudo apt update && sudo apt install nginx

   # CentOS/RHEL
   sudo yum install nginx

   # macOS
   brew install nginx
   ```

2. **上传网站文件**
   ```bash
   # 将整个文件夹上传到服务器
   scp -r 备案专用网站 user@server:/var/www/
   ```

3. **配置 Nginx**
   ```bash
   # 复制配置文件
   sudo cp deploy/nginx.conf /etc/nginx/sites-available/filing-site

   # 修改配置文件中的域名和路径
   sudo nano /etc/nginx/sites-available/filing-site

   # 创建软链接启用站点
   sudo ln -s /etc/nginx/sites-available/filing-site /etc/nginx/sites-enabled/

   # 测试配置
   sudo nginx -t

   # 重载 Nginx
   sudo systemctl reload nginx
   ```

### 方式二：使用 PM2 部署

1. **安装 Node.js 和 PM2**
   ```bash
   # 安装 Node.js（如果未安装）
   curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
   sudo apt install -y nodejs

   # 安装 PM2 和 http-server
   sudo npm install -g pm2 http-server
   ```

2. **上传网站文件**
   ```bash
   scp -r 备案专用网站 user@server:/home/user/
   ```

3. **配置并启动 PM2**
   ```bash
   # 修改配置文件中的路径
   nano deploy/ecosystem.config.json

   # 启动服务
   pm2 start deploy/ecosystem.config.json

   # 设置开机自启
   pm2 startup
   pm2 save
   ```

### 方式三：使用 Docker 部署

1. **创建 Dockerfile**
   ```dockerfile
   FROM nginx:alpine
   COPY . /usr/share/nginx/html
   EXPOSE 80
   ```

2. **构建并运行**
   ```bash
   docker build -t filing-site .
   docker run -d -p 80:80 --name filing-site filing-site
   ```

## ✏️ 自定义修改

修改 [index.html](index.html) 中的以下内容：

- **您的名字** - 替换为你的真实姓名
- **全栈开发者 / 设计爱好者** - 替换为你的职业或简介
- **关于我** 部分 - 修改个人介绍
- **技能** 标签 - 添加或删除技能标签
- **联系方式** - 更新你的联系信息
- **ICP备案号** - 填入你的备案号

## 🔧 配置反向代理（使用 Nginx）

如果使用 PM2 部署（端口 8080），可以用 Nginx 做反向代理：

```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

## 📋 备案检查清单

- [ ] 网站可正常访问
- [ ] 页面底部显示 ICP 备案号
- [ ] 网站内容符合备案要求
- [ ] 联系方式真实有效
- [ ] 域名已解析到服务器 IP
- [ ] 服务器 80 端口已开放

## 🔍 PM2 常用命令

```bash
# 启动服务
pm2 start deploy/ecosystem.config.json

# 查看状态
pm2 status

# 查看日志
pm2 logs filing-site

# 重启服务
pm2 restart filing-site

# 停止服务
pm2 stop filing-site

# 删除服务
pm2 delete filing-site
```

## 🌐 访问网站

部署完成后，通过以下方式访问：
- 直接访问域名：http://your-domain.com
- 本地测试：http://localhost:8080（使用 PM2）

## 📝 注意事项

1. 备案期间网站必须可访问
2. 备案号要在页面底部明显位置展示
3. 网站内容要真实，不得有违规信息
4. 确保服务器稳定运行

---

有问题请检查 Nginx 或 PM2 的日志文件。
