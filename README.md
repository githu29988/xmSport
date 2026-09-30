<div align="center">

# 小米运动健康刷步数

[![小米运动](https://img.shields.io/badge/小米运动-passing-success.svg?style=flat-square&logo=xiaomi&logoWidth=20&logoColor=white)](https://github.com/chiupam/xmSport/actions)
[![JavaScript](https://img.shields.io/badge/JavaScript-ES6-yellow.svg?style=flat-square&logo=javascript)](https://www.javascript.com/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue.svg?style=flat-square&logo=typescript)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-16.x-green.svg?style=flat-square&logo=node.js)](https://nodejs.org/)
[![GitHub stars](https://img.shields.io/github/stars/chiupam/xmSport?style=flat-square&logo=github)](https://github.com/chiupam/xmSport/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/chiupam/xmSport?style=flat-square&logo=github)](https://github.com/chiupam/xmSport/network/members)
[![License](https://img.shields.io/github/license/chiupam/xmSport?style=flat-square)](LICENSE)

</div>

这是一个使用GitHub Actions自动化执行的小米运动健康刷步数项目，可以定时为小米运动账号模拟随机步数数据。

## ✨ 功能

- 🕒 自动定时执行刷步数脚本（每天UTC时间14:55和15:55，对应北京时间22:55和23:55）
- 🎲 支持自定义步数范围，随机生成步数
- 🔄 支持GitHub Actions自动化执行及手动触发
- 🛡️ 内置重试机制，提高执行成功率
- 📱 支持多种渠道推送通知结果，包括Server酱、Bark、Telegram等

## 🚀 快速开始

1. **Fork本仓库**到你的GitHub账号
2. 在仓库的**Settings > Secrets > Actions**中添加以下Secrets：

### 🔑 必需的环境变量

| 名称 | 必填 | 说明 | 默认值 |
|------|:----:|------| ----- |
| `PHONE_NUMBER` | ✅ | 小米运动/小米健康账号绑定的手机号（不含+86） | 无 |
| `PASSWORD` | ✅ | 账号密码 | 无 |
| `xmSportMinStep` | ❌ | 最小步数 | 20000 |
| `xmSportMaxStep` | ❌ | 最大步数 | 22000 |
| `ENABLE_NOTIFY` | ❌ | 是否启用通知功能，设置为`true`时启用 | false |

#### 📲 通知配置

启用通知推送功能后（`ENABLE_NOTIFY=true`），可以配置以下通知渠道（至少需要配置一个）：

| 变量名 | 说明 | 参考文档 |
| ----- | ---- | ------- |
| `SERVERCHAN_KEY` | Server酱的推送密钥 | [Server酱文档](https://sct.ftqq.com/) |
| `BARK_KEY` | Bark推送密钥或完整URL | [Bark文档](https://github.com/Finb/Bark) |
| `TG_BOT_TOKEN` | Telegram机器人Token | [Telegram Bot API](https://core.telegram.org/bots/api) |
| `TG_CHAT_ID` | Telegram接收消息的用户或群组ID | [获取Chat ID教程](https://core.telegram.org/bots/features#chat-id) |
| `DINGTALK_WEBHOOK` | 钉钉机器人的Webhook URL | [钉钉自定义机器人文档](https://open.dingtalk.com/document/robots/custom-robot-access) |
| `DINGTALK_SECRET` | 钉钉机器人的安全密钥(可选) | 同上 |
| `WECOM_KEY` | 企业微信机器人的WebHook Key | [企业微信机器人文档](https://developer.work.weixin.qq.com/document/path/91770) |
| `PUSHPLUS_TOKEN` | PushPlus推送Token | [PushPlus文档](https://www.pushplus.plus/) |

3. GitHub Actions将按计划自动运行

## ⚙️ 工作流程

GitHub Actions工作流程配置在`.github/workflows/xmsport.yml`文件中：

- ⏰ 每天14:55和15:55自动执行（UTC时间，对应北京时间22:55和23:55）
- 👆 支持手动触发工作流程并设置自定义步数范围
- 🟢 使用Node.js环境运行脚本

## 🖱️ 手动触发签到

如果你想立即测试签到功能，可以手动触发：

1. 进入 "Actions" 标签
2. 选择 "小米运动修改步数" workflow
3. 点击 "Run workflow" 按钮
4. 点击 "Run workflow" 确认运行

> 💡 **提示**：手动触发时会使用您在GitHub Secrets中配置的环境变量，如果没有配置则使用默认值。

## 📝 数据模板

仓库中已经包含了`src/data.txt`文件，其中包含小米运动的数据模板：

- ✅ 工作流会自动读取该文件内容，无需手动设置环境变量
- 🔄 如需修改数据模板，只需直接编辑该文件，推送后自动生效

## 📲 通知功能

脚本执行失败时，可以通过多种渠道接收通知：

- 需要先设置`ENABLE_NOTIFY`为`true`来启用通知功能
- 仅在修改步数失败时才会发送通知，成功时不会打扰您
- **Server酱**：微信推送，设置`SERVERCHAN_KEY`环境变量
- **Bark**：iOS推送，设置`BARK_KEY`环境变量
- **Telegram**：设置`TG_BOT_TOKEN`和`TG_CHAT_ID`环境变量
- **钉钉**：企业消息，设置`DINGTALK_WEBHOOK`和可选的`DINGTALK_SECRET`环境变量
- **企业微信**：设置`WECOM_KEY`环境变量
- **PushPlus**：微信推送，设置`PUSHPLUS_TOKEN`环境变量

如果未配置任何通知渠道，脚本将只在GitHub Actions日志中输出结果。

## 📂 文件结构

```
xmSport/
├── .github/                          # GitHub相关配置
│   └── workflows/                    # GitHub Actions工作流
│       ├── build.yml                 # TypeScript构建工作流
│       └── xmsport.yml               # 主要运行工作流
├── src/                              # 源代码目录
│   ├── types/                        # 类型定义
│   │   ├── apiService.types.ts       # API服务相关类型
│   │   ├── dataProcessor.types.ts    # 数据处理相关类型
│   │   ├── index.ts                  # 类型导出索引
│   │   ├── main.types.ts             # 主程序相关类型
│   │   └── notify.types.ts           # 通知服务相关类型
│   ├── apiService.ts                 # API服务模块，处理与小米服务器的通信
│   ├── dataProcessor.ts              # 数据处理模块，负责处理和生成数据
│   ├── index.ts                      # 主脚本文件，负责请求处理和步数提交
│   ├── local-test.ts                 # 本地测试脚本
│   ├── notify.ts                     # 通知模块，支持多种渠道推送结果
│   ├── utils.ts                      # 工具函数模块，提供各种通用功能
│   └── data.txt                      # 数据模板文件
├── dist/                             # 编译后的JavaScript文件目录
│   ├── types/                        # 编译后的类型定义
│   ├── apiService.js                 # 编译后的API服务模块
│   ├── dataProcessor.js              # 编译后的数据处理模块
│   ├── index.js                      # 编译后的主脚本文件
│   ├── local-test.js                 # 编译后的本地测试脚本
│   ├── notify.js                     # 编译后的通知模块
│   ├── utils.js                      # 编译后的工具函数模块
│   └── data.txt                      # 复制的数据模板文件
├── js-backup/                        # JavaScript原始文件备份
├── .env.example                      # 环境变量示例文件
├── .gitignore                        # Git忽略文件配置
├── LICENSE                           # 开源协议
├── package.json                      # 项目依赖和脚本配置
├── README.md                         # 项目说明文档
└── tsconfig.json                     # TypeScript配置文件

```

### 主要文件说明

- **src/index.ts**: 程序入口点，处理环境变量，调用API服务和通知服务
- **src/apiService.ts**: 包含与小米运动API交互的所有函数，包括登录、获取token和发送步数数据
- **src/dataProcessor.ts**: 负责处理数据模板，替换步数和日期
- **src/notify.ts**: 负责发送通知到多个平台(Server酱、Bark、Telegram等)
- **src/utils.ts**: 包含通用工具函数，如时间格式化、随机数生成和URL参数转换
- **src/types/**: 按模块组织的类型定义文件，提高代码的类型安全性

## 🔧 开发说明

本项目使用TypeScript开发，提供更好的类型安全和开发体验。源代码在`src`目录下，编译后的JavaScript文件在`dist`目录下。

### 自动构建

当你推送TypeScript源文件变更到GitHub仓库时，会自动触发构建工作流，将TypeScript编译为JavaScript并提交到`dist`目录。GitHub Actions工作流运行时使用的是编译后的JavaScript文件。

### 本地开发

如果你想在本地进行开发，请按照以下步骤：

1. 克隆仓库到本地
   ```bash
   git clone https://github.com/chiupam/xmSport.git
   cd xmSport
   ```

2. 安装依赖
   ```bash
   npm install
   ```

3. 编译TypeScript
   ```bash
   npm run build
   ```

4. 编辑源代码后重新构建
   ```bash
   npm run build
   ```

5. 或者启用开发模式，自动监视文件变化
   ```bash
   npm run dev
   ```

## ⚠️ 免责声明

**请仔细阅读以下声明：**

1. 本项目仅供学习和研究目的使用，不得用于商业或非法用途
2. 使用本项目可能违反小米运动/小米健康的服务条款，请自行评估使用风险
3. 本项目不保证功能的可用性，也不保证不会被小米官方检测或封禁
4. 使用本项目造成的任何问题，包括但不限于账号被封禁、数据丢失等，项目作者概不负责
5. 用户需自行承担使用本项目的全部风险和法律责任

## 📜 许可证

本项目采用 [MIT 许可证](LICENSE) 进行许可。

## 🧪 本地测试

如果你想在本地测试脚本，可以按照以下步骤操作：

1. 克隆仓库到本地
   ```bash
   git clone https://github.com/chipam/xmSport.git
   cd xmSport
   ```

2. 安装依赖
   ```bash
   npm install
   ```

3. 创建环境变量文件
   ```bash
   cp .env.example .env
   ```

4. 编辑`.env`文件，填入你的个人信息
   - 修改`PHONE_NUMBER`和`PASSWORD`为你的账号信息
   - `DATA_JSON`会通过测试脚本自动读取，无需手动设置

5. 运行测试脚本（推荐）
   ```bash
   npm test
   ```
   这个命令会自动读取`src/data.txt`文件内容并设置为环境变量

6. 调试失败通知（可选）
   ```bash
   npm run test:fail
   ```
   这个命令使用错误的账号密码来触发失败通知

### 手动方式测试（可选）

如果你希望手动控制环境变量，可以：

1. 打开`src/data.txt`文件，复制其内容
2. 将内容粘贴到`.env`文件的`DATA_JSON=`后面（注意转义特殊字符）
3. 使用以下命令运行：
   ```bash
   npm run test:env
   ``` 

## 🔄 Fork仓库自动同步

如果你Fork了此仓库，可以启用自动同步功能，保持与上游仓库的更新一致：

1. 在你Fork的仓库中，进入**Actions**标签页
2. 你会看到一个名为**自动同步上游仓库**的工作流
3. 点击**I understand my workflows, go ahead and enable them**启用工作流
4. 现在，你的Fork仓库将每天自动与此上游仓库同步

你也可以随时手动触发同步：
1. 在你Fork的仓库中，进入**Actions**标签页
2. 在左侧选择**自动同步上游仓库**工作流
3. 点击**Run workflow**按钮，然后点击绿色的**Run workflow**按钮确认

这样你的Fork仓库就会立即与上游仓库同步，获取最新的功能和修复。

> **注意**：此工作流仅在Fork的仓库中运行，不会在原始仓库中执行。 
