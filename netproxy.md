# 通过代理连接 GitHub 的配置方法

## 问题描述

在某些网络环境下，直接访问 GitHub 可能会遇到连接问题，如：
- `fatal: unable to access 'https://github.com/...': Recv failure: Connection was reset`
- `Failed to connect to 127.0.0.1 port xxxx after xxxx ms: Couldn't connect to server`

## 解决方案

### 1. 配置 Git 代理

#### 方法一：配置 HTTP/HTTPS 代理
```bash
# 配置 HTTP 代理
git config --global http.proxy http://127.0.0.1:7897

# 配置 HTTPS 代理
git config --global https.proxy http://127.0.0.1:7897
```

#### 方法二：配置 SOCKS5 代理
```bash
# 配置 SOCKS5 代理
git config --global http.proxy socks5://127.0.0.1:1080
git config --global https.proxy socks5://127.0.0.1:1080
```

### 2. 查看和管理代理配置

#### 查看当前代理配置
```bash
# 查看所有 Git 配置
git config --global --list

# 只查看代理相关配置
git config --global --list | findstr proxy
```

#### 取消代理配置
```bash
# 取消 HTTP 代理
git config --global --unset http.proxy

# 取消 HTTPS 代理
git config --global --unset https.proxy
```

### 3. 常见代理端口

不同代理软件的常见端口：
- **Clash**: 7890 (HTTP), 7891 (SOCKS5)
- **V2Ray**: 1080 (SOCKS5), 8080 (HTTP)
- **Shadowsocks**: 1080 (SOCKS5)
- **自定义**: 7897 (本次使用的端口)

### 4. 其他 Git 优化配置

为了提高连接稳定性，可以配置以下参数：

```bash
# 增加缓冲区大小
git config --global http.postBuffer 1048576000

# 设置低速限制
git config --global http.lowSpeedLimit 0
git config --global http.lowSpeedTime 999999

# 临时禁用 SSL 验证（不推荐用于生产环境）
git config --global http.sslVerify false
```

### 5. 验证连接

配置完成后，可以通过以下命令验证：

```bash
# 测试克隆公共仓库
git clone https://github.com/octocat/Hello-World.git

# 测试网络连接
ping github.com
```

### 6. 故障排除

#### 问题：代理端口连接失败
- 确认代理软件正在运行
- 检查代理端口是否正确
- 尝试不同的代理端口

#### 问题：SSL 证书验证失败
```bash
# 临时解决方案（不推荐）
git config --global http.sslVerify false

# 推荐解决方案：更新证书或使用正确的代理配置
```

## 成功案例

本文档记录的配置在以下环境中测试成功：
- **操作系统**: Windows 10
- **Git 版本**: 最新版本
- **代理端口**: 7897
- **测试仓库**: https://github.com/scilid/agent-hub.git

## 注意事项

1. **安全性**: 在公共网络或不信任的环境中使用代理时要格外小心
2. **性能**: 使用代理可能会影响 Git 操作的速度
3. **配置管理**: 建议定期检查和更新代理配置
4. **备份**: 在修改 Git 配置前，建议备份当前配置

---

*创建时间: 2025年6月10日*  
*作者: scilid*  
*用途: Agent Hub 项目学习环境配置*

## 问题描述

在某些网络环境下，直接访问 GitHub 可能会遇到连接问题，如：
- `fatal: unable to access 'https://github.com/...': Recv failure: Connection was reset`
- `Failed to connect to 127.0.0.1 port xxxx after xxxx ms: Couldn't connect to server`

## 解决方案

### 1. 配置 Git 代理

#### 方法一：配置 HTTP/HTTPS 代理
```bash
# 配置 HTTP 代理
git config --global http.proxy http://127.0.0.1:7897

# 配置 HTTPS 代理
git config --global https.proxy http://127.0.0.1:7897
```

#### 方法二：配置 SOCKS5 代理
```bash
# 配置 SOCKS5 代理
git config --global http.proxy socks5://127.0.0.1:1080
git config --global https.proxy socks5://127.0.0.1:1080
```

### 2. 查看和管理代理配置

#### 查看当前代理配置
```bash
# 查看所有 Git 配置
git config --global --list

# 只查看代理相关配置
git config --global --list | findstr proxy
```

#### 取消代理配置
```bash
# 取消 HTTP 代理
git config --global --unset http.proxy

# 取消 HTTPS 代理
git config --global --unset https.proxy
```

### 3. 常见代理端口

不同代理软件的常见端口：
- **Clash**: 7890 (HTTP), 7891 (SOCKS5)
- **V2Ray**: 1080 (SOCKS5), 8080 (HTTP)
- **Shadowsocks**: 1080 (SOCKS5)
- **自定义**: 7897 (本次使用的端口)

### 4. 其他 Git 优化配置

为了提高连接稳定性，可以配置以下参数：

```bash
# 增加缓冲区大小
git config --global http.postBuffer 1048576000

# 设置低速限制
git config --global http.lowSpeedLimit 0
git config --global http.lowSpeedTime 999999

# 临时禁用 SSL 验证（不推荐用于生产环境）
git config --global http.sslVerify false
```

### 5. 验证连接

配置完成后，可以通过以下命令验证：

```bash
# 测试克隆公共仓库
git clone https://github.com/octocat/Hello-World.git

# 测试网络连接
ping github.com
```

### 6. 故障排除

#### 问题：代理端口连接失败
- 确认代理软件正在运行
- 检查代理端口是否正确
- 尝试不同的代理端口

#### 问题：SSL 证书验证失败
```bash
# 临时解决方案（不推荐）
git config --global http.sslVerify false

# 推荐解决方案：更新证书或使用正确的代理配置
```

## 成功案例

本文档记录的配置在以下环境中测试成功：
- **操作系统**: Windows 10
- **Git 版本**: 最新版本
- **代理端口**: 7897
- **测试仓库**: https://github.com/scilid/agent-hub.git

## 注意事项

1. **安全性**: 在公共网络或不信任的环境中使用代理时要格外小心
2. **性能**: 使用代理可能会影响 Git 操作的速度
3. **配置管理**: 建议定期检查和更新代理配置
4. **备份**: 在修改 Git 配置前，建议备份当前配置

---

*创建时间: 2025年6月10日*  
*作者: scilid*  
*用途: Agent Hub 项目学习环境配置* 