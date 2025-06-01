# Ubuntu 部署 SOCKS5 代理（无认证版）完整流程

📌 适用环境：
操作系统：Ubuntu 18.04 / 20.04 / 22.04

IP：公网可访问

默认端口：1080

默认状态：匿名开放（无密码）

---

## 📦 环境准备

建议使用 Ubuntu 服务器

1✅、第一步：安装 Dante 服务端
```bash
apt update && apt install -y dante-server dos2unix
```

2✅、第二步：上传`danted.conf`文件到`/etc`文件下

3✅、第三步：将换行符转换为 Unix 格式（防止 CRLF 错误）
```bash
dos2unix /etc/danted.conf
```

4✅、第四步：启动服务并设置自启动
```bash
systemctl restart danted
systemctl enable danted
```

6✅、第五步：检查监听端口是否成功
```bash
ss -tunlp | grep 1080
```
```bash
tcp LISTEN 0 511 0.0.0.0:1080 ...
```

7✅、第六步：放行防火墙与云平台端口（重要）
```bash
ufw allow 1080/tcp
```
```bash
如果是阿里云 / 腾讯云 / 华为云：
前往控制台 → 安全组规则 → 入方向规则
添加以下一条规则：

协议	端口范围	授权对象
TCP	1080	0.0.0.0/0
```
8✅、第七步：测试 SOCKS5 是否可用
```bash
curl --socks5 公网IP:1080 http://ip.sb
```

## 📦 最终代理地址

```bash
socks5://你的IP:1080
```
