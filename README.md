# openclaw-
openclaw安装步骤和注意事项
🦞 基于 Node.js 的本地 AI 智能助手，支持接入多种大模型 API，实现对话、自动化任务与技能扩展。
📋 环境要求
组件	说明
操作系统	Windows 10/11 (64位)
Node.js	v22.x LTS 或更高版本
Git	2.x 或更高版本 (安装脚本会自动引导)
网络	需能访问 GitHub 及所选模型服务商 API
权限	安装过程中需以管理员身份运行 PowerShell


🚀 快速安装
1. 安装 Node.js
访问 Node.js 官网 下载 v22.x LTS 版本的 .msi 安装包 (64-bit)。

安装注意事项：

右键点击安装包，选择 "以管理员身份运行"

确保勾选 "Add to PATH"

⚠️ 不要勾选 "Automatically install the necessary tools..."

其余选项保持默认，一路 Next 完成安装


node -v
npm -v    



如果双击 .msi 无反应，可用命令行安装：

cmd
msiexec /i "E:\路径\to\node-v22.x.x-x64.msi"

2. 安装 Git (可选)
OpenClaw 安装脚本会自动引导便携版 Git。如在网络受限环境下失败，可手动安装：

下载地址：https://git-scm.com/download/win

国内镜像：https://registry.npmmirror.com/binary.html?path=git-for-windows/

安装时注意：

默认编辑器选择 Notepad

PATH 环境选择 "Git from the command line and also from 3rd-party software"

HTTPS 传输选择 "Use the native Windows Secure Channel library"



3. 安装 OpenClaw
右键开始菜单 → "终端(管理员)" 或 "Windows PowerShell(管理员)"，依次执行：

powershell
# 1. 设置执行策略
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force

# 2. 运行官方安装脚本
iwr -useb https://openclaw.ai/install.ps1 | iex
等待出现 OpenClaw installed successfully 提示

openclaw onboard

如何获取火山引擎 API Key 和接入点 ID
登录 火山方舟控制台

左侧菜单 → "API Key 管理" → 创建 API Key

左侧菜单 → "在线推理" → 创建推理接入点

选择模型 (如 Doubao-Seed-2.0-pro)，创建后复制 ep- 开头的接入点 ID

阿里云百炼 (备选)
配置项	值
Base URL	https://dashscope.aliyuncs.com/compatible-mode/v1
Model ID	qwen-plus 或 qwen-turbo
🔐 安全提醒：API Key 只粘贴到终端配置中，切勿通过聊天、截图或文档分享。

🌐 启动与登录
启动服务
powershell
# 前台启动 (查看日志)
openclaw

# 后台启动 (推荐)
openclaw daemon start
登录仪表盘
浏览器访问 http://127.0.0.1:18789

输入初始化时生成的 Access Token

或运行 openclaw dashboard 生成自动登录链接

🔧 常用命令速查
命令	功能
openclaw	前台启动
openclaw daemon start	后台启动 (开机自启)
openclaw daemon stop	停止后台服务
openclaw gateway status	查看网关状态
openclaw dashboard	生成自动登录链接
openclaw doctor	环境诊断
openclaw update	更新版本
openclaw model list	列出已配置模型
openclaw token rotate	更换访问令牌
npx kill-port 18789	释放端口
❓ 常见问题排查
现象	原因	解决步骤
openclaw 命令未识别	环境变量未刷新	关闭终端后以管理员身份重新打开 PowerShell
网关无法访问 (ECONNREFUSED)	网关服务未运行	openclaw daemon stop && npx kill-port 18789 && openclaw daemon start
验证模型失败 (400/404)	API Key 或模型 ID 错误	确认 API Key 正确；火山引擎需使用 ep- 开头的接入点 ID
"Unknown model"	默认模型配置错误	openclaw model list 查看可用模型，更新 openclaw.json 中 agents.defaults.model.primary
网关启动提示 missing gateway.mode	配置项缺失	在 openclaw.json 中添加 "gateway": {"mode": "local"}
断开连接 (1006)	进程崩溃或网络波动	重启网关；关闭代理/VPN；检查日志
🛡️ 安全建议
定期轮换 Access Token：

powershell
openclaw token rotate
定期更换 API Key：登录模型服务商控制台删除旧 Key 并创建新 Key

不要泄露凭证：Token 和 API Key 切勿通过聊天、截图或提交到公开仓库

本地使用：建议仅允许 127.0.0.1 回环访问，不对外暴露网关端口

🗑️ 卸载
powershell
# 停止服务
openclaw daemon stop
openclaw gateway stop

# 卸载程序
npm uninstall -g openclaw

# 清理残留 (可选)
Remove-Item -Recurse -Force "$env:USERPROFILE\.openclaw"
Remove-Item -Recurse -Force "$env:USERPROFILE\AppData\Local\Temp\openclaw"
📁 配置文件位置
核心配置文件位于：

text
C:\Users\你的用户名\.openclaw\openclaw.json
日志文件位于：

text
C:\Users\你的用户名\AppData\Local\Temp\openclaw\
🔗 相关链接
OpenClaw 官方文档：https://docs.openclaw.ai

Node.js 官网：https://nodejs.org

Git 下载：https://git-scm.com

火山方舟控制台：https://console.volcengine.com/ark/

阿里云百炼控制台：https://bailian.console.aliyun.com/



