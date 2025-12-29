# AuthCore IAM

一个基于 Docker 的身份认证和访问管理系统。


# AuthCore IAM - 现代身份与访问管理系统

AuthCore IAM 是一个基于 Node.js 和 React 的现代化身份与访问管理系统，提供完整的用户管理、组织管理、项目管理和权限控制功能。

## 🚀 功能特性

- **用户管理**：完整的用户注册、登录、邮箱验证、密码重置功能
- **组织管理**：多租户组织架构，支持组织层级管理
- **项目管理**：项目创建、成员管理、角色分配
- **权限控制**：基于角色的访问控制（RBAC），细粒度权限管理
- **多语言支持**：中英文界面切换
- **响应式设计**：基于 Ant Design 的现代化 UI
- **安全性**：JWT 认证、密码加密、请求限流


## 🔐 配置默认账号

系统初始化之前在env环境文件配置超级管理员账号：
```
SUPER_ADMIN_USERNAME=admin
SUPER_ADMIN_EMAIL=you_email
SUPER_ADMIN_PASSWORD=you_email_password
SUPER_ADMIN_DISPLAY_NAME=超级管理员
```

- **用户名**：`SUPER_ADMIN_USERNAME`
- **邮箱**：`SUPER_ADMIN_EMAIL`
- **密码**：`SUPER_ADMIN_PASSWORD`


## 🏗️ 系统架构

### 后端技术栈

- **框架**：Express.js
- **数据库**：MySQL + Sequelize ORM
- **认证**：JWT
- **安全**：bcrypt、helmet、cors、rate-limiting
- **邮件**：nodemailer
- **配置**：YAML 配置文件

### 前端技术栈

- **框架**：React 18
- **UI 库**：Ant Design
- **路由**：React Router
- **HTTP 客户端**：Axios
- **状态管理**：React Context

### 权限系统

采用基于角色的访问控制（RBAC）：

- **系统级角色**：超级管理员
- **组织级角色**：组织管理员、编辑员、人事管理员、查看员、成员
- **项目级角色**：项目所有者、项目经理、开发人员、测试人员、观察者、普通成员

## 快速开始

### 1. 环境配置

首先，复制环境变量模板文件：

```bash
cp .env.example .env
```

然后编辑 `.env` 文件，配置以下必要参数：

#### 数据库配置
- `DB_PASSWORD`: 数据库用户密码
- `MYSQL_ROOT_PASSWORD`: MySQL root 用户密码
- `MYSQL_PORT`: MySQL 对外暴露端口（默认 3307，避免与本地 MySQL 冲突）

#### 应用配置
- `JWT_SECRET`: JWT 密钥，建议使用强随机字符串
- `FRONTEND_PORT`: 前端服务端口（默认 3000）
- `BACKEND_PORT`: 后端 API 端口（默认 3001）

#### 邮件配置（用于发送验证邮件等）
- `SMTP_HOST`: SMTP 服务器地址
- `SMTP_PORT`: SMTP 端口
- `SMTP_USER`: 邮箱用户名
- `SMTP_PASS`: 邮箱密码或应用专用密码
- `SMTP_FROM`: 发件人邮箱地址

### 2. 启动服务

使用 Docker Compose 启动所有服务：

```bash
docker-compose up -d || docker compose up -d
```

### 3. 访问应用

- 前端界面: http://localhost:3000
- 后端 API: http://localhost:3001

### 4. 停止服务

```bash
docker compose down
```

## 环境变量说明

| 变量名 | 描述 | 默认值 | 是否必填 |
|--------|------|--------|----------|
| `DB_HOST` | 数据库主机 | mysql | 否 |
| `DB_PORT` | 数据库端口 | 3306 | 否 |
| `DB_NAME` | 数据库名称 | authcore_iam | 否 |
| `DB_USER` | 数据库用户名 | authcore | 否 |
| `DB_PASSWORD` | 数据库密码 | - | 是 |
| `MYSQL_ROOT_PASSWORD` | MySQL root 密码 | - | 是 |
| `MYSQL_PORT` | MySQL 对外端口 | 3307 | 否 |
| `FRONTEND_PORT` | 前端服务端口 | 3000 | 否 |
| `BACKEND_PORT` | 后端服务端口 | 3001 | 否 |
| `JWT_SECRET` | JWT 密钥 | - | 是 |
| `NODE_ENV` | 运行环境 | production | 否 |
| `SMTP_HOST` | SMTP 服务器 | - | 是 |
| `SMTP_PORT` | SMTP 端口 | 587 | 否 |
| `SMTP_USER` | 邮箱用户名 | - | 是 |
| `SMTP_PASS` | 邮箱密码 | - | 是 |
| `SMTP_FROM` | 发件人邮箱 | - | 是 |

## 常见邮箱配置示例

### QQ 邮箱
```env
SMTP_HOST=smtp.qq.com
SMTP_PORT=587
SMTP_USER=your_qq_number@qq.com
SMTP_PASS=your_qq_mail_authorization_code
SMTP_FROM=your_qq_number@qq.com
```

### Gmail
```env
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_app_password
SMTP_FROM=your_email@gmail.com
```

### 163 邮箱
```env
SMTP_HOST=smtp.163.com
SMTP_PORT=587
SMTP_USER=your_email@163.com
SMTP_PASS=your_authorization_code
SMTP_FROM=your_email@163.com
```

## 注意事项

1. **安全性**: 请确保 `.env` 文件不要提交到 Git 仓库中
2. **JWT 密钥**: 建议使用至少 32 位的随机字符串作为 JWT_SECRET
3. **邮箱配置**: 大多数邮箱服务需要使用应用专用密码而不是登录密码
4. **端口冲突**: 如果本地已有服务占用相关端口，请修改对应的端口配置

## 验证配置

### 检查环境变量是否生效

查看容器中的环境变量：

```bash
# 查看应用容器的环境变量
docker compose exec app env | grep -E "(SMTP|DB|JWT)"
```

### 验证邮件配置

#### 方法1：查看容器日志
```bash
# 查看应用容器日志，寻找邮件相关的错误信息
docker compose logs app | grep -i mail
```

#### 方法2：进入容器手动测试
```bash
# 进入应用容器
docker compose exec app sh

# 在容器内查看环境变量
echo $SMTP_HOST
echo $SMTP_USER
echo $SMTP_FROM
```

#### 方法3：使用测试邮件功能
如果应用提供了测试邮件接口，可以通过 API 测试：

```bash
# 假设应用有测试邮件的接口
curl -X POST http://localhost:3001/api/test-email \
  -H "Content-Type: application/json" \
  -d '{"to": "your-test-email@example.com"}'
```

### 检查邮件配置是否正确

#### 验证 SMTP 连接
```bash
# 使用 telnet 测试 SMTP 连接（需要在本地安装 telnet）
telnet your-smtp-host 587

# 或使用 nc (netcat)
nc -zv your-smtp-host 587
```

#### 常见邮箱 SMTP 测试
```bash
# QQ 邮箱
nc -zv smtp.qq.com 587

# Gmail
nc -zv smtp.gmail.com 587

# 163 邮箱
nc -zv smtp.163.com 587
```

## 故障排除

### 数据库连接失败
- 检查数据库配置是否正确
- 确认 MySQL 容器是否正常启动：`docker-compose logs mysql`

### 邮件发送失败
- 检查 SMTP 配置是否正确：`docker-compose exec app env | grep SMTP`
- 确认邮箱服务是否开启了 SMTP 功能
- 检查是否使用了正确的应用专用密码
- 查看应用日志中的邮件错误：`docker-compose logs app | grep -i mail`
- 测试 SMTP 服务器连接：`nc -zv your-smtp-host 587`

### 如何确认邮件配置是你自己的
1. **检查环境变量**：
   ```bash
   docker compose exec app env | grep SMTP_USER
   docker compose exec app env | grep SMTP_FROM
   ```

2. **查看 .env 文件**：
   ```bash
   cat .env | grep SMTP
   ```

3. **发送测试邮件**：
   - 如果应用有测试功能，发送一封测试邮件到你的邮箱
   - 检查邮件的发件人是否是你配置的邮箱

4. **检查邮箱发件箱**：
   - 登录你配置的邮箱账户
   - 查看发件箱是否有应用发送的邮件

### 端口被占用
- 修改 `.env` 文件中的端口配置
- 或停止占用端口的其他服务

### 容器启动失败
```bash
# 查看所有容器状态
docker compose ps 

# 查看特定容器日志
docker compose logs app
docker compose logs mysql

# 重新构建并启动
docker compose down
docker compose up -d --force-recreate
```



## 📞 支持

如有问题，请提交 Issue 或联系开发者。
