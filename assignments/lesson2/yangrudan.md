# Git工具安装与使用报告

**学员GitHub用户名**: yangrudan

## 1. Git安装过程

### 操作系统

macOS Sequoia 15.3.1

### 安装方法

#### 通过Homebrew安装

Homebrew是macOS专属包管理器，可高效管理Git的安装与更新，适配Intel和Apple Silicon芯片。

1. **安装Homebrew（未安装时执行）**：
打开终端（通过Spotlight搜索“终端”或按Command+Space调出搜索框输入“Terminal”），执行以下命令，过程中按提示输入系统密码并等待安装完成：
`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

2. **安装Git**：
终端执行以下命令，Homebrew会自动下载并安装最新版本Git：
`brew install git`

3. **验证安装**：
执行版本查询命令，若输出类似“git version 2.39.5”的信息，安装成功：
`git --version`


## 2. 遇到的问题及解决方法

### 问题：终端提示“git: command not found”

- **现象**：安装完成后执行Git命令，系统无法识别。

- **原因**：Git的可执行路径未添加到系统环境变量PATH中，常见于手动安装或Homebrew配置不完整的场景。

- **解决方法**：
        执行`echo $PATH`查看当前环境变量，确认是否包含Git路径。

- 若缺失，执行对应命令添加（根据安装方式选择）：
- 手动安装：`echo 'export PATH="/Applications/Git.app/Contents/Resources/bin:$PATH"' >> ~/.zshrc`
- Homebrew安装（M1/M2）：`echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc`

- 执行`source ~/.zshrc`使配置生效，重启终端后重试。


## 3. 版本信息截图


![Git版本信息截图](./git_version.png)


## 4. Git命令使用过程总结

### 基础配置（首次使用必备）

安装完成后，需配置全局用户名和邮箱（与GitHub账号一致），用于标识提交记录的作者身份：

```Plain Text

git config --global user.name "example-user"  # 你的GitHub用户名
git config --global user.email "example@xxx.com"  # 你的GitHub绑定邮箱
git config --global core.editor "nano"  # 可选：配置默认编辑器（如nano、vim）
git config --list  # 查看当前所有Git配置
```

### 核心命令使用体验与理解

#### 1. git clone：克隆远程仓库到本地

- **功能**：将远程GitHub仓库完整复制到本地，自动关联远程仓库（默认名为origin）。

- **使用命令**：
- HTTPS方式（需输入用户名和个人访问令牌）：
`git clone https://github.com/example-user/your-repo.git`
- SSH方式（配置后免密，推荐）：
`git clone git@github.com:example-user/your-repo.git`

- **实用技巧**：克隆大型仓库时，可添加`--depth 1`仅获取最新版本（减少下载量）：
`git clone --depth 1 git@github.com:example-user/your-repo.git`

#### 2. git add：添加文件到暂存区

- **功能**：将本地修改（新建、修改、删除）的文件标记为“待提交”状态，暂存区是本地与仓库的中间层。

- **使用命令**：
`git add README.md`（添加单个文件）
`git add src/`（添加指定目录）
`git add .`（添加当前目录所有修改，推荐）
`git add -u`（仅添加已跟踪文件的修改/删除，不含新建文件）

- **注意事项**：执行后通过`git status -s`查看状态，“A”表示已添加，“M”表示已修改，“D”表示已删除。

#### 3. git commit：提交更改到本地仓库

- **功能**：将暂存区的更改永久保存到本地仓库，生成包含提交信息的版本记录。

- **使用命令**：
`git commit -m "feat: 完成Git安装报告初稿"`（直接附带简洁提交信息，推荐）
`git commit`（打开默认编辑器输入多行提交信息）
`git commit --amend`（修改最近一次提交信息或补充遗漏文件）

- **最佳实践**：提交信息遵循“类型: 描述”格式，如“fix: 修复SSH连接错误”“docs: 更新安装步骤”，便于版本回溯。

#### 4. git push：推送本地更改到远程仓库

- **功能**：将本地仓库的提交同步到远程GitHub仓库，实现代码共享与备份。

- **使用命令**：
`git push origin main`（推送本地main分支到远程origin仓库）
`git push -u origin main`（首次推送绑定分支，后续可直接执行`git push`）

- **常见问题**：若远程有新提交，需先执行`git pull`拉取合并，否则会提示冲突；HTTPS方式需使用GitHub个人访问令牌（而非密码）登录。

#### 5. git pull：从远程仓库拉取最新更改

- **功能**：获取远程仓库的最新提交并合并到本地当前分支，确保本地代码与团队同步。

- **使用命令**：
`git pull origin main`（拉取远程main分支并合并）
`git pull`（已绑定分支时可省略参数）

- **冲突处理**：若出现合并冲突，Git会标记冲突文件（含“<<<<<<< HEAD”等标记），需手动编辑保留正确代码，再执行`git add 冲突文件`和`git commit`完成合并。

#### 其他常用命令

- **git branch**：分支管理
`git branch -b dev`（创建dev分支）
`git switch dev`（切换到dev分支，Git 2.23+支持）
`git branch -d dev`（删除本地dev分支）

- **git merge**：合并分支
`git switch main && git merge dev`（切换到main分支，合并dev分支更改）

- **git log**：查看提交历史
`git log --oneline`（简洁显示版本记录）
`git log --graph`（图形化展示分支合并历史）

- **git reset**：版本回退（谨慎使用）
`git reset --hard HEAD~1`（回退到上一个版本）
`git reset --hard 提交哈希值`（回退到指定版本，哈希值通过git log获取）

### 整体使用心得

macOS下Git的安装与使用更贴近Unix风格，通过Homebrew可实现Git的一键安装与版本更新，极大降低了环境配置成本。Git的核心价值在于“分布式协作”与“版本追溯”，通过暂存区（add）、本地仓库（commit）、远程仓库（push/pull）的流程设计，能清晰管控代码变更，避免团队协作中的版本混乱。

实际使用中，养成“先pull再开发、频繁提交、清晰备注”的习惯，减少冲突概率；SSH密钥配置虽需额外步骤，但能实现免密访问，提升开发效率；对于分支管理，可采用“main（稳定分支）+dev（开发分支）+fix-xxx（修复分支）”的模式，确保主分支代码的安全性。结合GitHub的Pull Request功能，还能实现代码审查与协作流程的规范化，进一步提升开发质量。

---