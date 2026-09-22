# 工地数字地图管理云平台 — GitHub Pages 部署指南

> 本文档记录从"零"到"外网可访问"的完整流程，适用于 Windows / macOS / Linux。
> 任何人按本文档操作，可在自己的 GitHub 账号下部署一个独立的可访问站点。

---

## 📑 目录

1. [前置准备](#1-前置准备)
2. [项目结构](#2-项目结构)
3. [GitHub 账号注册](#3-github-账号注册)
4. [本地环境配置](#4-本地环境配置)
5. [GitHub 认证](#5-github-认证)
6. [创建 GitHub 仓库](#6-创建-github-仓库)
7. [推送代码](#7-推送代码)
8. [启用 GitHub Pages](#8-启用-github-pages)
9. [验证部署](#9-验证部署)
10. [后续更新流程](#10-后续更新流程)
11. [安全加固建议](#11-安全加固建议)
12. [常见问题排查](#12-常见问题排查)

---

## 1. 前置准备

### 1.1 需要的账号

| 账号 | 是否必须 | 用途 | 注册地址 |
|------|----------|------|----------|
| GitHub 账号 | ✅ 必须 | 托管代码 + Pages 部署 | https://github.com/signup |

### 1.2 需要安装的软件

| 软件 | 是否必须 | 用途 | 下载地址 |
|------|----------|------|----------|
| Git | ✅ 必须 | 推送代码到 GitHub | https://git-scm.com/downloads |
| GitHub CLI (`gh`) | ✅ 必须 | 创建仓库 + 启用 Pages | 见下方安装说明 |

#### 安装 GitHub CLI（Windows）

```powershell
# 方式 A：使用 winget（推荐）
winget install --id GitHub.CLI --silent --accept-package-agreements --accept-source-agreements

# 方式 B：使用 Chocolatey
choco install gh -y

# 方式 C：手动下载
# 访问 https://cli.github.com/ 下载安装包
```

#### 安装 GitHub CLI（macOS）

```bash
brew install gh
```

#### 安装 GitHub CLI（Linux）

```bash
# Debian/Ubuntu
sudo apt install gh

# 其他发行版参考 https://cli.github.com/manual/installation
```

### 1.3 验证安装

```bash
git --version
# 预期输出：git version 2.x.x

gh --version
# 预期输出：gh version 2.x.x
```

---

## 2. 项目结构

将项目文件整理为如下结构（**项目根目录示例** `D:\projects\工地数字地图`）：

```
工地数字地图/
├── index.html              # 主页面（必需）
├── README.md               # 项目说明（可选但推荐）
├── 工地数字地图应用项目总览表.xlsx   # 数据文件（可选）
└── .gitignore              # Git 忽略配置（推荐）
```

### 2.1 `.gitignore` 推荐内容

```gitignore
# OS
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/
*.swp
*.swo

# Logs
*.log
```

---

## 3. GitHub 账号注册

1. 打开 https://github.com/signup
2. 填写用户名、密码、邮箱
3. 完成验证码
4. 选择 Free 免费计划
5. 完成邮箱验证

> ⚠️ **用户名一旦确定不可轻易更改**，请慎重选择。最终部署 URL 是：
> `https://你的用户名.github.io/仓库名/`

---

## 4. 本地环境配置

### 4.1 配置 Git 用户信息

将下面命令中的 `你的用户名` 和 `你的邮箱` 替换为你的真实信息：

```bash
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱@example.com"
git config --global init.defaultBranch main
```

> 💡 **GitHub 推荐使用 `用户名@users.noreply.github.com`** 作为提交邮箱来保护隐私：
> 示例：`yaoyongcheng@users.noreply.github.com`
> 设置路径：GitHub → Settings → Emails

### 4.2 进入项目目录

```bash
cd "D:\projects\工地数字地图"
```

---

## 5. GitHub 认证

### 5.1 通过设备码登录（推荐）

```bash
gh auth login --web --git-protocol https
```

执行此命令后，终端会输出一行类似：

```
! One-time code (XXXX-XXXX) copied to clipboard
Open this URL to continue in your web browser: https://github.com/login/device
```

### 5.2 在浏览器完成授权

1. 打开浏览器
2. 访问 `https://github.com/login/device`
3. 登录你的 GitHub 账号
4. 输入 8 位一次性代码（`XXXX-XXXX`）
5. 点击 **"Authorize github"** 按钮

### 5.3 验证认证成功

```bash
gh auth status
```

预期输出：

```
github.com
  ✓ Logged in to github.com account 你的用户名 (keyring)
  - Active account: true
  - Git operations protocol: https
  - Token scopes: 'gist', 'read:org', 'repo'
```

> ✅ 必须看到 `'repo'` 权限，否则无法推送代码。

---

## 6. 创建 GitHub 仓库

### 方式 A：通过命令行创建（推荐）

```bash
gh repo create 你的仓库名 --public --source=. --remote=origin --push
```

参数说明：
- `你的仓库名`：例如 `site-map`、`construction-map`
- `--public`：公开（GitHub Pages 免费版必须 public）
- `--source=.`：从当前目录创建
- `--remote=origin`：自动添加远程仓库地址
- `--push`：自动提交并推送当前目录文件

### 方式 B：通过网页创建

1. 访问 https://github.com/new
2. 填写：
   - **Repository name**: `你的仓库名`
   - **Public/Private**: 选择 **Public**（GitHub Pages 免费版要求）
   - **Add a README file**: ❌ 不要勾选（本地已有）
   - **Add .gitignore**: ❌ 不要勾选（本地已有）
3. 点击 **Create repository**
4. 按页面提示推送本地代码（见下一步）

---

## 7. 推送代码

### 7.1 初始化本地仓库（仅当本地尚未初始化时）

```bash
cd "D:\projects\工地数字地图"
git init
git checkout -b main 2>/dev/null || git branch -M main
git add .
git commit -m "init: 首次部署"
```

### 7.2 添加远程仓库

如果你用了方式 A 创建仓库，可跳过此步骤。

```bash
git remote add origin https://github.com/你的用户名/你的仓库名.git
```

### 7.3 推送到 GitHub

```bash
git push -u origin main
```

**预期输出**：

```
...
To https://github.com/你的用户名/你的仓库名.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

---

## 8. 启用 GitHub Pages

### 8.1 通过 API 启用（推荐）

```bash
gh api -X POST "repos/你的用户名/你的仓库名/pages" \
  -f "source[branch]=main" \
  -f "source[path]=/"
```

**预期输出**（核心字段）：

```json
{
  "html_url": "https://你的用户名.github.io/你的仓库名/",
  "https_enforced": true,
  "public": true,
  "status": "building",
  "source": {"branch": "main", "path": "/"}
}
```

### 8.2 通过网页启用

1. 访问 `https://github.com/你的用户名/你的仓库名/settings/pages`
2. **Source**: 选择 `Deploy from a branch`
3. **Branch**: 选择 `main` / `(root)`
4. 点击 **Save**

---

## 9. 验证部署

### 9.1 等待首次构建

GitHub Pages 首次构建需要 **1~3 分钟**。实时查询状态：

```bash
gh api "repos/你的用户名/你的仓库名/pages/builds/latest" \
  | grep -E "status|duration"
```

期望 `status: built`。

### 9.2 测试访问

```bash
curl -s -o /dev/null -w "HTTP: %{http_code}\n" \
  "https://你的用户名.github.io/你的仓库名/"
```

期望 `HTTP: 200`。

### 9.3 浏览器打开验证

打开浏览器访问：

```
https://你的用户名.github.io/你的仓库名/
```

看到你的页面 → 部署成功 🎉

---

## 10. 后续更新流程

### 10.1 修改本地文件

```bash
cd "D:\projects\工地数字地图"
# 编辑文件...
git add .
git commit -m "描述你的修改"
git push
```

GitHub Pages 会**自动重新部署**，1~2 分钟后生效。

### 10.2 启用 Branch Protection（强烈建议）

如果你想防止误删/误改（启用后连你自己都必须走 PR）：

```bash
gh api -X PUT "repos/你的用户名/你的仓库名/branches/main/protection" \
  -F "required_status_checks=null" \
  -F "enforce_admins=true" \
  -F "required_pull_request_reviews[required_approving_review_count]=1" \
  -F "restrictions=null" \
  -F "required_signatures=false" \
  -F "required_linear_history=false" \
  -F "allow_force_pushes=false" \
  -F "allow_deletions=false" \
  -F "block_creations=false" \
  -F "required_conversation_resolution=false" \
  -F "lock_branch=false" \
  -F "allow_fork_syncing=false"
```

⚠️ 启用后，每次改动需走 PR：

```bash
git checkout -b update-描述
# 修改...
git add . && git commit -m "..."
git push -u origin update-描述
# 在 GitHub 网页创建 PR 并合并
```

### 10.3 临时修改（绕过 Branch Protection）

如需紧急直接修改主分支：

```bash
# 临时禁用保护
gh api -X DELETE "repos/你的用户名/你的仓库名/branches/main/protection"
# 推送修改
git push origin main
# 重新启用保护
gh api -X PUT "repos/你的用户名/你的仓库名/branches/main/protection" \
  -F "enforce_admins=true" \
  -F "required_pull_request_reviews[required_approving_review_count]=1" \
  -F "allow_force_pushes=false" \
  -F "allow_deletions=false"
```

---

## 11. 安全加固建议

### 11.1 🚨 不要上传敏感数据

**绝不要上传**包含以下信息的文件：

- ❌ 真实姓名 + 手机号
- ❌ 真实身份证号 / 银行卡号
- ❌ 内部 IP 地址 / 服务器地址
- ❌ API Key / Secret / Token
- ❌ 真实精确 GPS 坐标 + 项目名称
- ❌ 客户信息 / 合同内容
- ❌ 数据库连接字符串

GitHub Pages 部署后是**完全公开**的，搜索引擎会收录，无法真正撤回。

### 11.2 🛡️ 推荐的安全措施

| 措施 | 操作 | 效果 |
|------|------|------|
| 添加 `robots.txt` 阻止收录 | 在根目录建文件，内容 `User-agent: * Disallow: /` | 阻止搜索引擎 |
| 启用 Branch Protection | 见 10.2 | 防止误删/误改 |
| 添加 LICENSE 文件 | 仓库根目录加 `LICENSE` | 声明版权 |
| 敏感信息脱敏 | 推 GitHub 前替换 | 避免泄露 |
| 不在 commit message 写敏感信息 | 提交说明保持中性 | 避免历史泄露 |

### 11.3 🔒 真正的私有部署方案

如果数据敏感，必须用以下方案之一：

| 方案 | 成本 | 难度 |
|------|------|------|
| GitHub Pro 私有仓库 + Pages | $4/月 | 简单 |
| Vercel/Netlify 私有部署 | 免费（有限度） | 简单 |
| 自建服务器 + 域名 + 防火墙 | ¥500+/年起 | 复杂 |
| Cloudflare Access + Pages | 免费 50 用户 | 中等 |

---

## 12. 常见问题排查

### Q1: `gh: command not found`

✅ 解决**：使用完整路径 `C:\Program Files\GitHub CLI\gh.exe`，或将 GitHub CLI 安装路径加入 PATH。

Windows 快速修复：

```powershell
$env:PATH += ";C:\Program Files\GitHub CLI"
```

### Q2: 推送时要求输入用户名密码

✅ 原因**：GitHub 已不再支持密码推送。
**解决**：
1. 使用 `gh auth login` 登录后，GitHub CLI 会自动管理凭证
2. 或创建 Personal Access Token：https://github.com/settings/tokens/new
3. 在 push 时用 token 替代密码

### Q3: Pages 状态一直是 `building`

✅ 解决**：
- 等待 1-3 分钟（首次构建慢）
- 检查 `https://github.com/你的用户名/你的仓库名/settings/pages` 有无错误
- 查看 Actions 标签页有无构建日志

### Q4: 访问页面 404

✅ 检查**：
- 仓库必须是 **Public**（免费版要求）
- Pages 是否已 build 完成
- URL 是否正确：`https://用户名.github.io/仓库名/`
- 仓库名不要含中文（虽然支持但有时出问题）

### Q5: 字体/图表加载失败（特别是国内）

✅ 原因**：HTML 中可能引用了 Google Fonts。
**解决**：在 `<head>` 中替换为国内 CDN：

```html
<!-- 原版 -->
<link href="https://fonts.googleapis.com/css2?family=Orbitron..." rel="stylesheet">

<!-- 国内替代 -->
<link href="https://fonts.font.im/css2?family=Orbitron..." rel="stylesheet">
```

### Q6: 想换绑自定义域名

✅ 解决**：
1. 在域名 DNS 添加 CNAME 记录指向 `你的用户名.github.io`
2. 在 GitHub 仓库根目录添加 `CNAME` 文件（无后缀），内容写你的域名
3. 等待 DNS 生效（最长 24 小时）

---

## 附录 A：完整脚本（一键执行）

适合**新机器**首次部署。**将变量替换为你的真实信息后粘贴运行**：

```bash
# ===== 配置变量 =====
GITHUB_USER="你的GitHub用户名"
REPO_NAME="你的仓库名（如 site-map）"
LOCAL_DIR="D:\\projects\\工地数字地图"

# ===== 环境检查 =====
git --version || { echo "请先安装 Git"; exit 1; }
gh --version || { echo "请先安装 GitHub CLI"; exit 1; }

# ===== 配置 Git =====
git config --global user.name "$GITHUB_USER"
git config --global user.email "${GITHUB_USER}@users.noreply.github.com"
git config --global init.defaultBranch main

# ===== 登录 GitHub =====
gh auth login --web --git-protocol https

# ===== 进入项目目录 =====
cd "$LOCAL_DIR" || { echo "目录不存在"; exit 1; }

# ===== 初始化仓库 + 提交 =====
git init
git checkout -b main 2>/dev/null || git branch -M main
git add .
git commit -m "init: 首次部署"

# ===== 创建远程仓库并推送 =====
gh repo create "$REPO_NAME" --public --source=. --remote=origin --push

# ===== 启用 Pages =====
gh api -X POST "repos/${GITHUB_USER}/${REPO_NAME}/pages" \
  -f "source[branch]=main" \
  -f "source[path]=/"

# ===== 输出结果 =====
echo ""
echo "✅ 部署完成！"
echo "访问地址: https://${GITHUB_USER}.github.io/${REPO_NAME}/"
echo "等待 1-3 分钟后生效"
```

---

## 附录 B：常用命令速查

```bash
# 查看仓库信息
gh repo view 你的用户名/你的仓库名

# 查看 Pages 状态
gh api "repos/你的用户名/你的仓库名/pages"

# 查看构建历史
gh api "repos/你的用户名/你的仓库名/pages/builds"

# 删除文件并推送
git rm 文件名
git commit -m "remove 文件名"
git push

# 查看提交历史
git log --oneline

# 撤销未推送的修改
git checkout -- 文件名

# 克隆别人仓库
git clone https://github.com/用户名/仓库名.git

# Fork 别人的仓库
gh repo fork 用户名/仓库名 --clone
```

---

## 附录 C：术语对照表

| 英文 | 中文 | 解释 |
|------|------|------|
| Repository (Repo) | 仓库 | GitHub 上的项目目录 |
| Branch | 分支 | 代码的独立开发线 |
| Commit | 提交 | 一次代码变更记录 |
| Push | 推送 | 把本地代码上传到远程 |
| Pull | 拉取 | 把远程代码下载到本地 |
| Merge | 合并 | 把不同分支合到一起 |
| Pull Request (PR) | 拉取请求 | 请求合并你的修改 |
| Fork | 分叉 | 复制别人的仓库到自己的账号 |
| Pages | 页面 | GitHub 的静态网站托管服务 |
| Deploy | 部署 | 把代码发布到可访问的环境 |

---

## 📜 文档版本

- **版本**: v1.0
- **最后更新**: 2026-09-22
- **适用 GitHub CLI 版本**: ≥ 2.0
- **适用 Git 版本**: ≥ 2.30

---

## ⚠️ 免责声明

本文档仅描述技术部署流程。**部署者须自行承担数据合规与隐私保护责任**。请勿上传任何包含个人隐私、商业机密或受法律保护数据（.z）的项目至公共 GitHub 仓库。