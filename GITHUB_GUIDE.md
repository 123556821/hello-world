# GitHub 详细使用指南（新手版）

适用账号：`123556821`（Alervioo）。目标是建立一套能长期使用的 Git / GitHub 工作流。

---

## 0. 当前已完成的配置

| 项目 | 状态 |
| --- | --- |
| GitHub 账号 | 已连接，用户名 `123556821`，昵称 `Alervioo` |
| 本地 Git 用户名 | `Alervioo` |
| 本地 Git 邮箱 | `306363626+123556821@users.noreply.github.com`（隐私邮箱，避免暴露真实邮箱） |
| 默认分支 | `main` |
| SSH 密钥 | 未检测到 `~/.ssh`，需按第 2 节生成 |
| 远程仓库创建 | 受当前连接权限限制，暂未完成（见文末说明） |

查看当前配置：

```bash
git config --global --list
```

---

## 1. 完善 GitHub 个人资料（网页端）

1. 打开 https://github.com/settings/profile
2. 建议填写：
   - Name：`Alervioo`
   - Bio：`La gloire à mes genoux`
   - Website / Blog：有个人主页则填，没有可留空
   - Location：可选
   - Pronouns：可选
3. 头像：https://github.com/settings/profile → Upload new picture
4. 保存后打开 https://github.com/123556821 确认公开资料。

---

## 2. SSH Key 生成与绑定（推荐，免每次输密码）

### 2.1 生成密钥

Windows PowerShell / Git Bash 中执行：

```bash
ssh-keygen -t ed25519 -C "306363626+123556821@users.noreply.github.com"
```

- 保存位置直接回车：默认 `~/.ssh/id_ed25519`
- Passphrase：建议设置一个口令；留空则无口令，便利性高但安全性低

若已存在同名文件，先确认是否需要保留，不要直接覆盖。

### 2.2 复制公钥

```bash
# Windows Git Bash
cat ~/.ssh/id_ed25519.pub

# 或 PowerShell
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub
```

输出以 `ssh-ed25519` 开头，整行复制。

### 2.3 添加到 GitHub

1. 打开 https://github.com/settings/keys
2. 点击 `New SSH key`
3. Title：例如 `Windows-PC`
4. Key type：`Authentication Key`
5. Key：粘贴刚才的公钥
6. `Add SSH key`

### 2.4 验证

```bash
ssh -T git@github.com
```

成功提示：`Hi 123556821! You've successfully authenticated...`

### 2.5 克隆时使用 SSH 地址

```bash
git clone git@github.com:123556821/hello-world.git
```

---

## 3. 创建与克隆仓库

### 网页创建

1. https://github.com/new
2. Repository name：`hello-world`
3. Description：可选
4. 选择 `Public`
5. 勾选 `Add a README file`
6. `Create repository`

### 命令行克隆

```bash
# SSH（推荐）
git clone git@github.com:123556821/hello-world.git

# HTTPS
git clone https://github.com/123556821/hello-world.git

cd hello-world
```

---

## 4. 日常提交流程

```bash
# 1. 查看改动
git status
git diff

# 2. 暂存
git add README.md        # 指定文件
git add .                # 全部改动（含新文件）

# 3. 提交
git commit -m "feat: 添加 README 模板"

# 4. 推送
git push origin main

# 5. 拉取远程更新
git pull origin main
```

提交信息建议格式：

- `feat: 新功能`
- `fix: 修 bug`
- `docs: 文档改动`
- `chore: 杂项`

---

## 5. 分支管理

```bash
# 新建并切换分支
git switch -c feature/demo

# 查看分支
git branch -a

# 切换分支
git switch main

# 推送分支
git push -u origin feature/demo

# 合并分支（在 main 上执行）
git switch main
git merge feature/demo

# 删除分支
git branch -d feature/demo
git push origin --delete feature/demo
```

规则：`main` 保持稳定，新功能在独立分支开发。

---

## 6. Fork 与 Pull Request（参与别人的项目）

1. 在目标仓库点 `Fork`
2. 克隆自己的 fork
3. 新建分支并修改、提交、推送
4. 打开 `https://github.com/123556821/<仓库>`，点 `Compare & pull request`
5. 填写说明，提交 PR
6. 维护者评审后合并

同步上游更新：

```bash
git remote add upstream https://github.com/原作者/仓库.git
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

---

## 7. Issue 与项目协作

- `Issues`：记录 bug、需求、想法；用标签 `bug` / `enhancement` / `question` 分类
- `Projects`：看板管理任务
- `Discussions`：开放讨论
- `Wiki`：项目文档

---

## 8. 必备文件

| 文件 | 作用 |
| --- | --- |
| `README.md` | 项目门面：介绍、安装、用法 |
| `.gitignore` | 忽略临时文件、依赖、密钥 |
| `LICENSE` | 开源协议 |
| `CONTRIBUTING.md` | 贡献指南 |

`.gitignore` 示例：

```gitignore
# 环境变量与密钥
.env
.env.*
*.pem

# 依赖
node_modules/
__pycache__/
.venv/

# 编辑器
.vscode/
.idea/
.DS_Store
```

---

## 9. 常用命令速查

| 命令 | 说明 |
| --- | --- |
| `git status` | 查看工作区状态 |
| `git log --oneline --graph` | 查看提交历史 |
| `git diff` | 查看未暂存改动 |
| `git add <file>` | 暂存文件 |
| `git commit -m "msg"` | 提交 |
| `git push` | 推送 |
| `git pull` | 拉取并合并 |
| `git fetch` | 只拉取不合并 |
| `git restore <file>` | 丢弃未暂存改动 |
| `git reset --soft HEAD~1` | 撤销上一次提交，保留改动 |
| `git stash` | 临时保存改动 |
| `git stash pop` | 恢复临时改动 |

---

## 10. 安全与习惯

- 私钥 `~/.ssh/id_ed25519` 绝不外传，也不要提交到仓库
- 令牌、密码只放环境变量或密钥管理服务
- 每次提交前 `git status` 检查一遍
- 小步提交，信息写清楚
- `main` 分支保持可发布状态

---

## 11. 当前阻塞项与解决方式

创建仓库时接口返回：`403 Resource not accessible by integration`。
原因：当前 GitHub 连接的应用权限不足（缺少创建仓库权限），不是账号问题。

解决方式任选其一：

1. 在 GitHub 应用授权页为当前连接补充 `Administration` / 仓库创建权限，然后重试；
2. 手动打开 https://github.com/new 创建 `hello-world`（Public + README）；
3. 提供具有 `repo` 权限的 Personal Access Token，由命令行创建并推送。

仓库建好后，把本目录的 `README.md` 与 `GITHUB_GUIDE.md` 复制进去即可：

```bash
cp README.md GITHUB_GUIDE.md /path/to/hello-world/
cd /path/to/hello-world
git add .
git commit -m "docs: 添加 README 模板与 GitHub 使用指南"
git push origin main
```
