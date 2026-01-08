# Receipt Screenshot Uploader 📸

一个专为收据管理设计的 Chrome 扩展，支持一键截图并自动上传到 Google Drive，适用于多账户管理场景。

## ✨ 功能特点 

- 📷 **一键截图**：快速截取当前页面的可见区域 
- ☁️ **自动上传**：直接上传到指定的 Google Drive 文件夹
- 🏷️ **智能命名**：自动从页面提取购买日期和商品ID生成文件名
- 👥 **多账户管理**：支持管理多个信用卡账户的上传位置
- 🔄 **云端配置**：账户配置存储在 Google Drive，支持多设备同步
- ⚙️ **配置管理**：内置账户管理界面，支持增删改操作

## 🎯 使用场景

适合需要管理多个信用卡账户的在线购物收据，用于：
- 📊 记账和财务管理
- 💰 费用报销
- 📁 收据归档整理

## 🚀 安装方法

### 开发环境（测试使用）

1. 下载或克隆此仓库
2. 打开 Chrome 浏览器，访问 `chrome://extensions/`
3. 开启右上角的「开发者模式」
4. 点击「加载已解压的扩展程序」
5. 选择项目文件夹

### Chrome Web Store（正式版）

*（内部发布链接将在此处更新）*

## 📖 使用说明

### 首次设置

1. **授权 Google Drive**
   - 点击扩展图标
   - 点击「🔐 授权 Google Drive」按钮
   - 登录并授权访问

2. **配置账户信息**
   - 点击「⚙️ 管理帐号」
   - 点击「➕ 新增帐号」
   - 填写持卡人名称、卡号和 Google Drive 文件夹 ID
   - 保存

### 日常使用

1. 打开需要截图的收据页面
2. 点击扩展图标
3. 从下拉菜单选择上传位置
4. 点击「📷 截图并上传」
5. 等待上传完成

### 文件命名规则

扩展会尝试从页面中提取信息来命名文件：
- **格式**：`MMDD 商品ID.png`
- **示例**：`1103 m57656194858.png`（11月3日，商品ID为m57656194858）
- **备用**：如果无法提取，使用默认文件名 `test.png`

### 页面元素要求

为了正确提取信息，页面需要包含以下 HTML 元素：
```html
<span data-partner-id="purchase-date">2025年11月3日 10:54</span>
<p data-partner-id="item-id">m57656194858</p>
```

## ⚙️ 配置管理

### 账户配置文件

配置文件 `credit_card.json` 存储在您的 Google Drive 中，格式如下：

```json
{
  "6388": {
    "card_user": "小林 美琴",
    "card_id": "6388",
    "folder_ID": "Google Drive 文件夹 ID"
  }
}
```

### 获取 Google Drive 文件夹 ID

1. 在 Google Drive 中打开目标文件夹
2. 查看浏览器地址栏
3. URL 格式：`https://drive.google.com/drive/folders/[这里是文件夹ID]`
4. 复制文件夹 ID

## 🔐 隐私权政策

### 数据收集与使用

本扩展收集以下数据：

#### 收集的数据
- **Google Drive 访问令牌**：用于上传截图（有效期1小时）
- **账户配置信息**：持卡人名称、卡号、Google Drive 文件夹 ID
- **页面数据**：购买日期和商品 ID（仅用于文件命名）

#### 数据存储
- **本地存储**：访问令牌和配置缓存存储在浏览器本地（chrome.storage.local）
- **云端存储**：配置文件和截图存储在您的 Google Drive
- **不传输到第三方**：所有数据仅在您的设备和 Google Drive 之间传输

#### 数据安全
- 访问令牌在1小时后自动过期
- 配置缓存在5分钟后自动刷新
- 所有通信使用 HTTPS 加密
- 您可以随时撤销 Google Drive 访问权限

### 权限说明

| 权限 | 用途 |
|------|------|
| `activeTab` | 截取当前标签页的可见区域 |
| `scripting` | 从页面提取购买日期和商品 ID |
| `storage` | 本地存储访问令牌和配置缓存 |
| `host_permissions` | 访问 Google Drive API 和 OAuth API |

### 第三方服务

- **Google Drive API**：用于存储截图和配置文件
  - [Google 隐私权政策](https://policies.google.com/privacy)

### 您的权利

您有权：
- 随时撤销 Google Drive 访问权限
- 删除本地存储的所有数据
- 删除 Google Drive 中的配置文件和截图
- 管理账户配置信息

### 联系方式

如有任何隐私权问题，请通过以下方式联系：
- 📧 Email: [your-email@example.com]
- 🐛 Issues: [GitHub Issues](https://github.com/yourusername/repository/issues)

---

**最后更新日期**：2026年1月7日

## 🛠️ 技术栈

- **框架**：Chrome Extension Manifest V3
- **API**：Google Drive API v3
- **认证**：OAuth 2.0 (Refresh Token)
- **语言**：Vanilla JavaScript, HTML5, CSS3

## 📁 项目结构

```
Receipt_Chrome/
├── manifest.json          # 扩展配置文件
├── index.html            # Popup 界面
├── app.js                # 核心功能实现
├── background.js         # 后台服务
├── credit_card.json      # 本地配置模板（实际使用云端版本）
├── .env                  # 环境变量（包含 OAuth 凭证）
├── icon/                 # 图标文件
│   ├── icon16.png
│   ├── icon48.png
│   └── icon128.png
└── README.md            # 项目说明
```

## 🔧 开发说明

### 环境变量

在 `.env` 文件中配置 Google OAuth 凭证：

```env
DRIVE_CLIENT_ID=your-client-id.apps.googleusercontent.com
DRIVE_CLIENT_SECRET=your-client-secret
DRIVE_REFRESH_TOKEN=your-refresh-token
```

### Google Cloud 设置

1. 前往 [Google Cloud Console](https://console.cloud.google.com/)
2. 创建项目并启用 Google Drive API
3. 创建 OAuth 2.0 凭证
4. 获取 Client ID、Client Secret 和 Refresh Token
5. 更新 `app.js` 中的 `OAUTH_CONFIG`

### 配置文件设置

1. 上传 `credit_card.json` 到 Google Drive
2. 获取文件的分享链接
3. 提取文件 ID（链接中的 `/d/[文件ID]/view` 部分）
4. 更新 `app.js` 中的 `CREDIT_CARD_FILE_ID`

## 🐛 常见问题

### Q: 授权失败怎么办？
A: 检查以下项目：
1. 确认 `app.js` 中的 OAuth 配置正确
2. 确认 Google Cloud Console 中的 OAuth 设置
3. 查看浏览器控制台的错误信息

### Q: 上传失败怎么办？
A: 可能原因：
1. Google Drive API 未启用
2. OAuth 权限不足
3. folder_ID 不正确或无权限访问
4. Access Token 已过期（点击授权按钮重新获取）

### Q: 无法提取文件名？
A: 确保页面包含正确的 HTML 元素（`data-partner-id="purchase-date"` 和 `data-partner-id="item-id"`）

## 📝 更新日志

### v1.0.0 (2026-01-07)
- ✅ 初始版本发布
- ✅ 支持截图并上传到 Google Drive
- ✅ 支持多账户管理
- ✅ 自动提取购买日期和商品 ID
- ✅ 云端配置存储

## 📄 许可证

MIT License

## 🤝 贡献

欢迎提交 Issues 和 Pull Requests！

---

**Made with ❤️ for better receipt management**
