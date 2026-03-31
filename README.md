# vigilant-spoon
我的个人仓库
# 校园跑腿服务系统

一个基于前后端分离架构的校园跑腿服务平台，连接骑手和师生，提供任务发布、接取、支付和资金流转等功能。

## 技术栈

### 后端
- **框架**: ASP.NET Core 8.0
- **数据库**: MySQL 8.0+
- **ORM**: Entity Framework Core
- **认证**: JWT (JSON Web Token)
- **支付**: 支付宝 SDK、微信支付 SDK

### 前端
- **框架**: React 18
- **路由**: React Router 6
- **UI库**: Ant Design 5
- **HTTP客户端**: Axios
- **构建工具**: Vite

## 系统功能

### 用户功能
- 用户注册和登录（学生/骑手角色）
- 个人信息管理
- 实名认证（预留接口）
- 评分系统
- 钱包管理（充值、提现、交易记录）

### 任务功能
- 发布跑腿任务
- 浏览任务列表（按状态筛选）
- 任务详情查看
- 接取任务（直接接单或竞价）
- 完成任务
- 取消任务
- 竞价管理

### 支付功能
- 任务支付（支付宝、微信支付、余额支付）
- 钱包充值
- 资金提现
- 交易记录查询
- 押金冻结/解冻机制

## 项目结构

```
ErrandServiceSystem/
├── Backend/
│   └── ErrandServiceSystem.Api/
│       ├── Controllers/          # API控制器
│       │   ├── AuthController.cs
│       │   ├── TaskController.cs
│       │   ├── PaymentController.cs
│       │   └── UserController.cs
│       ├── Data/                 # 数据库上下文
│       │   └── ApplicationDbContext.cs
│       ├── Models/               # 数据模型
│       │   ├── User.cs
│       │   ├── ErrandTask.cs
│       │   ├── TaskBid.cs
│       │   ├── Payment.cs
│       │   ├── Wallet.cs
│       │   └── Transaction.cs
│       ├── DTOs/                 # 数据传输对象
│       │   ├── AuthDTOs.cs
│       │   ├── TaskDTOs.cs
│       │   └── PaymentDTOs.cs
│       ├── Services/             # 业务逻辑服务
│       │   ├── IUserService.cs
│       │   ├── UserService.cs
│       │   ├── ITaskService.cs
│       │   ├── TaskService.cs
│       │   ├── IPaymentService.cs
│       │   └── PaymentService.cs
│       ├── Program.cs            # 应用入口
│       ├── appsettings.json      # 配置文件
│       └── ErrandServiceSystem.Api.csproj
├── Frontend/
│   └── errand-app/
│       ├── public/
│       ├── src/
│       │   ├── api/              # API封装
│       │   │   └── api.js
│       │   ├── components/       # 公共组件
│       │   │   └── Layout.jsx
│       │   ├── pages/            # 页面组件
│       │   │   ├── Login.jsx
│       │   │   ├── Register.jsx
│       │   │   ├── Home.jsx
│       │   │   ├── TaskList.jsx
│       │   │   ├── TaskDetail.jsx
│       │   │   ├── CreateTask.jsx
│       │   │   ├── MyTasks.jsx
│       │   │   ├── Wallet.jsx
│       │   │   └── Profile.jsx
│       │   ├── App.jsx
│       │   ├── main.jsx
│       │   └── index.css
│       ├── package.json
│       ├── vite.config.js
│       └── index.html
└── README.md
```

## 运行环境

### 软件要求
- **操作系统**: Windows 10/11
- **.NET SDK**: 8.0 或更高版本
- **Node.js**: 18.0 或更高版本
- **MySQL**: 8.0 或更高版本
- **npm**: 9.0 或更高版本（随Node.js安装）

### 安装步骤

#### 1. 安装 .NET SDK
下载并安装 .NET 8.0 SDK：
https://dotnet.microsoft.com/download/dotnet/8.0

验证安装：
```bash
dotnet --version
```

#### 2. 安装 Node.js
下载并安装 Node.js 18+：
https://nodejs.org/

验证安装：
```bash
node --version
npm --version
```

#### 3. 安装 MySQL
下载并安装 MySQL 8.0+：
https://dev.mysql.com/downloads/mysql/

安装完成后，确保 MySQL 服务已启动。

## 部署步骤

### 后端部署

#### 1. 配置数据库
打开 `Backend/ErrandServiceSystem.Api/appsettings.json`，修改数据库连接字符串：

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=ErrandServiceDB;User=root;Password=your_password;"
  }
}
```

将 `your_password` 替换为你的MySQL root密码。

#### 2. 配置JWT密钥
修改 `JwtSettings` 部分，设置更安全的密钥：

```json
{
  "JwtSettings": {
    "SecretKey": "YourSuperSecretKeyForJWTTokenGeneration123456789",
    "Issuer": "ErrandServiceSystem",
    "Audience": "ErrandServiceUsers",
    "ExpirationInMinutes": 60
  }
}
```

#### 3. 配置支付接口（可选）
如需集成真实的支付接口，修改 `Payment` 配置：

```json
{
  "Payment": {
    "Alipay": {
      "AppId": "your_alipay_appid",
      "PrivateKey": "your_alipay_private_key",
      "PublicKey": "your_alipay_public_key",
      "GatewayUrl": "https://openapi.alipay.com/gateway.do"
    },
    "WeChatPay": {
      "AppId": "your_wechat_appid",
      "MchId": "your_merchant_id",
      "ApiKey": "your_api_key",
      "CertPath": "path_to_cert",
      "PrivateKeyPath": "path_to_private_key",
      "PublicKeyPath": "path_to_public_key"
    }
  }
}
```

#### 4. 还原依赖包
在 `Backend/ErrandServiceSystem.Api` 目录下运行：

```bash
dotnet restore
```

#### 5. 执行数据库迁移（首次部署）
系统使用 Code First 方式，首次运行时 Entity Framework Core 会自动创建数据库和表。

#### 6. 运行后端
```bash
cd Backend/ErrandServiceSystem.Api
dotnet run
```

后端将在 `http://localhost:5000` 启动。

Swagger API 文档访问地址：`http://localhost:5000/swagger`

### 前端部署

#### 1. 安装依赖
在 `Frontend/errand-app` 目录下运行：

```bash
cd Frontend/errand-app
npm install
```

#### 2. 启动开发服务器
```bash
npm run dev
```

前端将在 `http://localhost:3000` 启动。

#### 3. 构建生产版本
```bash
npm run build
```

构建后的文件在 `dist` 目录。

## 使用说明

### 1. 注册账号
- 访问 `http://localhost:3000`
- 点击"立即注册"
- 填写用户名、邮箱、密码、手机号等信息
- 选择身份（学生或骑手）
- 点击"注册"完成注册

### 2. 登录系统
- 使用注册的用户名和密码登录
- 登录成功后会自动跳转到首页

### 3. 发布任务（学生）
- 点击"发布任务"
- 填写任务标题、描述、取货地点、送达地点
- 设置酬金和押金（可选）
- 选择截止时间
- 点击"发布任务"

### 4. 接取任务（骑手）
- 在"任务大厅"浏览任务
- 点击"查看详情"查看任务详情
- 可以选择"直接接单"或"竞价"
- 任务完成后，点击"完成任务"

### 5. 支付和充值
- 使用支付宝、微信支付或余额支付
- 在"我的钱包"进行充值和提现
- 查看交易记录

## API 接口文档

### 认证接口
- `POST /api/auth/register` - 用户注册
- `POST /api/auth/login` - 用户登录
- `GET /api/auth/me` - 获取当前用户信息

### 任务接口
- `GET /api/task` - 获取任务列表
- `GET /api/task/{id}` - 获取任务详情
- `POST /api/task` - 创建任务
- `PUT /api/task/{id}` - 更新任务
- `POST /api/task/{id}/accept` - 接取任务
- `POST /api/task/{id}/complete` - 完成任务
- `POST /api/task/{id}/cancel` - 取消任务
- `POST /api/task/{id}/bid` - 竞价
- `GET /api/task/{id}/bids` - 获取竞价列表
- `POST /api/task/bids/{bidId}/accept` - 接受竞价

### 支付接口
- `POST /api/payment` - 创建支付
- `GET /api/payment/{id}` - 获取支付详情
- `GET /api/payment/my` - 获取我的支付记录
- `POST /api/payment/recharge` - 充值
- `POST /api/payment/withdraw` - 提现
- `GET /api/payment/balance` - 获取余额
- `GET /api/payment/transactions` - 获取交易记录

### 用户接口
- `GET /api/user/{id}` - 获取用户信息
- `GET /api/user` - 获取所有用户
- `PUT /api/user/{id}` - 更新用户信息
- `POST /api/user/{id}/verify` - 认证用户

## 数据库设计

### Users（用户表）
- Id: 用户ID
- Username: 用户名
- Email: 邮箱
- PasswordHash: 密码哈希
- PhoneNumber: 手机号
- UserRole: 用户角色（Student/Rider/Admin）
- RealName: 真实姓名
- School: 学校
- Address: 地址
- IsVerified: 是否认证
- Rating: 评分
- TotalOrders: 总订单数

### ErrandTasks（任务表）
- Id: 任务ID
- PublisherId: 发布人ID
- Title: 标题
- Description: 描述
- PickupLocation: 取货地点
- DeliveryLocation: 送达地点
- Reward: 酬金
- Deposit: 押金
- Deadline: 截止时间
- Status: 状态（Pending/Accepted/Completed/Cancelled）
- RiderId: 接单人ID
- RiderNotes: 骑手备注
- AcceptedAt: 接单时间
- CompletedAt: 完成时间

### TaskBids（竞价表）
- Id: 竞价ID
- TaskId: 任务ID
- RiderId: 骑手ID
- BidAmount: 竞价金额
- Message: 备注
- Status: 状态（Pending/Accepted/Rejected）

### Payments（支付表）
- Id: 支付ID
- TaskId: 任务ID
- UserId: 用户ID
- Amount: 金额
- PaymentMethod: 支付方式
- TransactionId: 交易ID
- Status: 状态
- Description: 描述

### Wallets（钱包表）
- Id: 钱包ID
- UserId: 用户ID
- Balance: 余额
- FrozenAmount: 冻结金额
- TotalIncome: 总收入
- TotalExpense: 总支出

### Transactions（交易记录表）
- Id: 交易ID
- UserId: 用户ID
- Amount: 金额
- Type: 类型（Income/Expense/Freeze/Unfreeze）
- Description: 描述
- RelatedTaskId: 关联任务ID
- RelatedPaymentId: 关联支付ID
- BalanceBefore: 交易前余额
- BalanceAfter: 交易后余额

## 注意事项

1. **安全性**
   - 生产环境请修改JWT密钥为强密码
   - 使用HTTPS保护数据传输
   - 定期备份数据库

2. **支付功能**
   - 支付宝和微信支付需要申请商户账号
   - 当前代码为演示版本，支付直接返回成功
   - 生产环境需要对接真实的支付API

3. **性能优化**
   - 前端生产环境使用 `npm run build` 构建
   - 后端可考虑使用Redis缓存
   - 数据库建议添加索引优化查询

4. **扩展功能**
   - 可添加消息通知功能
   - 可添加任务评价和评论功能
   - 可添加地理位置功能
   - 可添加图片上传功能

## 故障排查

### 后端启动失败
- 检查 .NET SDK 版本是否为 8.0+
- 检查 MySQL 服务是否启动
- 检查数据库连接字符串是否正确

### 前端启动失败
- 检查 Node.js 版本是否为 18+
- 删除 `node_modules` 重新安装依赖
- 检查端口 3000 是否被占用

### 无法连接后端
- 检查后端是否在 `http://localhost:5000` 运行
- 检查 CORS 配置
- 查看浏览器控制台错误信息

## 联系方式

如有问题，请联系开发者。

## 许可证

本项目仅供学习和参考使用。

