# stillive.ai DNS 配置指南

## 当前状态
- ✅ GitHub Pages 已部署：https://github.com/imwyvern/stillive-ai
- ✅ 构建状态：built
- ⏳ 等待 DNS 配置

## Cloudflare DNS 配置

登录：https://dash.cloudflare.com/
选择域名：stillive.ai

### 添加以下记录：

#### A 记录（根域名）
```
类型: A
名称: @
内容: 185.199.108.153
代理状态: DNS only（灰色云朵）
TTL: Auto
```

```
类型: A
名称: @
内容: 185.199.109.153
代理状态: DNS only
TTL: Auto
```

```
类型: A
名称: @
内容: 185.199.110.153
代理状态: DNS only
TTL: Auto
```

```
类型: A
名称: @
内容: 185.199.111.153
代理状态: DNS only
TTL: Auto
```

#### CNAME 记录（www 子域名）
```
类型: CNAME
名称: www
内容: imwyvern.github.io
代理状态: DNS only
TTL: Auto
```

## 验证

DNS 生效后（通常 5-30 分钟），运行：

```bash
dig stillive.ai +short
# 应该返回 4 个 GitHub Pages IP
```

然后访问：
- http://stillive.ai
- https://stillive.ai （GitHub 会自动申请 SSL 证书）

## 临时访问

在 DNS 生效前，可以通过修改本地 hosts 文件预览：

```bash
sudo nano /etc/hosts
# 添加：
185.199.108.153 stillive.ai
```

然后访问 http://stillive.ai

## 故障排查

如果 DNS 配置后仍无法访问：

1. 清除 DNS 缓存：
   ```bash
   sudo dscacheutil -flushcache
   sudo killall -HUP mDNSResponder
   ```

2. 检查 GitHub Pages 状态：
   ```bash
   gh api repos/imwyvern/stillive-ai/pages
   ```

3. 强制重新构建：
   ```bash
   cd /Users/wes/hireai-pitch
   git commit --allow-empty -m "trigger rebuild"
   git push origin main
   ```

## 启用 HTTPS

DNS 生效后，GitHub 会自动申请 Let's Encrypt 证书（可能需要 24 小时）。

可以在 GitHub repo settings → Pages 中手动勾选 "Enforce HTTPS"。
