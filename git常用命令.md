# Git 常用命令手册（小白友好版）

Git 是目前最流行的**分布式版本控制系统**，用于管理代码的修改历史、协作开发和版本回溯。本手册整理了 Git 最常用的命令，涵盖**初始化、提交、分支、远程、撤销**等场景，附**用途、语法、示例**，帮你快速上手！

## 一、前置概念（必懂）

在使用命令前，先理解 Git 的核心模型：

- **工作区（Working Directory）**：你编辑文件的本地目录（比如 `my-project` 文件夹）。
- **暂存区（Staging Area/Index）**：临时存放待提交的修改（相当于“草稿箱”）。
- **本地仓库（Local Repository）**：`.git` 隐藏文件夹，保存所有历史版本。
- **远程仓库（Remote Repository）**：GitHub/Gitee 等线上仓库，用于协作。

## 二、基础命令（初始化与文件操作）

### 1. 初始化仓库

**用途**：将本地目录变成 Git 仓库（生成 `.git` 隐藏文件夹）。
​**语法**​：`git init [目录路径]`
​**示例**​：

bash

复制

```bash
# 在当前目录初始化仓库
git init

# 在指定目录初始化（比如 ./my-project）
git init ./my-project
```

### 2. 查看文件状态

**用途**：检查工作区/暂存区的修改状态（哪些文件改了、哪些在暂存区、哪些未跟踪）。
​**语法**​：`git status`
​**示例**​：

bash

复制

```bash
git status
# 输出示例：
# On branch main
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git restore <file>..." to discard changes in working directory)
#         modified:   README.md
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         src/index.js
```

### 3. 添加文件到暂存区

**用途**：将工作区的修改放入“草稿箱”，准备提交。
​**语法**​：

- `git add <文件名>`：添加单个文件（比如 `git add README.md`）。
- `git add .`：添加当前目录**所有修改/新增**的文件（最常用）。
- `git add <目录名>`：添加某个目录下的所有修改（比如 `git add src/`）。
  ​**示例**​：

bash

复制

```bash
# 添加所有修改到暂存区
git add .
```

### 4. 提交到本地仓库

**用途**：将暂存区的修改保存为**本地版本**（生成一个提交记录）。
​**语法**​：`git commit -m "提交说明"`
​**关键**​：`-m` 后面的说明要**清晰简洁**​（比如“修复登录按钮样式”“新增用户模块”），方便后续回溯。
​**示例**​：

bash

复制

```bash
git commit -m "初始化项目：添加 README 和基础结构"
```

### 5. 查看提交历史

**用途**：查看本地仓库的所有提交记录（包括提交人、时间、说明、哈希值）。
​**语法**​：

- `git log`：查看完整历史（按 `q` 退出）。
- `git log --oneline`：查看简洁历史（单行显示，推荐）。
- `git log --graph`：查看分支合并的图形化历史。
  ​**示例**​：

bash

复制

```bash
git log --oneline
# 输出示例：
# a1b2c3d (HEAD -> main) 初始化项目：添加 README 和基础结构
# d4e5f6a 第一次提交：创建 README.md
```

## 三、分支管理（核心协作功能）

分支是 Git 的灵魂，用于**隔离不同功能的修改**（比如“开发新功能”和“修复 bug”互不影响）。

### 1. 查看本地分支

**用途**：列出所有本地分支（`*` 标记当前所在分支）。
​**语法**​：`git branch`
​**示例**​：

bash

复制

```bash
git branch
# 输出示例：
# * main
#   dev
#   feature/login
```

### 2. 创建分支

**用途**：基于当前分支创建新分支（不会切换到新分支）。
​**语法**​：`git branch <分支名>`
​**示例**​：

bash

复制

```bash
# 创建名为 feature/search 的分支
git branch feature/search
```

### 3. 切换分支

**用途**：切换到指定分支（开始在该分支上工作）。
​**语法**​：

- `git checkout <分支名>`：传统方式（Git 1.x 常用）。
- `git switch <分支名>`：Git 2.23+ 推荐（更直观，“切换”的语义更明确）。
  ​**示例**​：

bash

复制

```bash
# 切换到 feature/search 分支（两种方式任选其一）
git checkout feature/search
# 或
git switch feature/search
```

### 4. 创建并切换分支（一步到位）

**用途**：同时创建新分支并切换过去（最常用）。
​**语法**​：`git checkout -b <分支名>` 或 `git switch -c <分支名>`
​**示例**​：

bash

复制

```bash
# 创建并切换到 dev 分支
git switch -c dev
```

### 5. 合并分支

**用途**：将指定分支的修改合并到当前分支（比如把 `feature/login` 的修改合并到 `main`）。
​**步骤**​：

1. 切换到目标分支（比如 `main`）：`git switch main`
2. 合并指定分支：`git merge <要合并的分支名>`
   ​**示例**​：

bash

复制

```bash
# 把 feature/login 分支的修改合并到 main
git switch main
git merge feature/login
```

### 6. 删除分支

**用途**：删除本地已合并的分支（未合并的分支需加 `-D` 强制删除）。
​**语法**​：

- `git branch -d <分支名>`：删除已合并的分支（安全）。
- `git branch -D <分支名>`：强制删除未合并的分支（危险，慎用）。
  ​**示例**​：

bash

复制

```bash
# 删除已合并的 feature/search 分支
git branch -d feature/search
```

## 四、远程仓库操作（协作关键）

远程仓库（比如 GitHub）用于**共享代码**和**团队协作**。

### 1. 添加远程仓库

**用途**：将本地仓库与线上远程仓库关联（`origin` 是远程仓库的默认名称）。
​**语法**​：`git remote add origin <远程仓库地址>`
​**示例**​：

bash

复制

```bash
# 关联 GitHub 仓库（地址从 GitHub 仓库页复制）
git remote add origin https://github.com/your-username/your-repo.git
```

### 2. 推送本地分支到远程

**用途**：将本地分支的修改上传到远程仓库（首次推送需加 `-u` 设置默认上游）。
​**语法**​：`git push -u origin <本地分支名>`
​**后续推送**​：若已设置上游，直接 `git push` 即可。
​**示例**​：

bash

复制

```bash
# 首次推送 main 分支到远程（设置 origin/main 为上游）
git push -u origin main

# 后续推送（直接 push）
git push
```

### 3. 拉取远程更新

**用途**：从远程仓库获取最新修改，并**合并到本地分支**（相当于 `git fetch` + `git merge`）。
​**语法**​：`git pull origin <远程分支名>`
​**示例**​：

bash

复制

```bash
# 拉取远程 main 分支的更新并合并到本地 main
git pull origin main
```

### 4. 获取远程更新（不合并）

**用途**：仅获取远程仓库的修改，但不合并到本地（用于查看远程变化）。
​**语法**​：`git fetch origin`
​**示例**​：

bash

复制

```bash
# 获取远程所有分支的更新
git fetch origin
```

### 5. 查看远程仓库地址

**用途**：确认当前仓库关联的远程地址。
​**语法**​：`git remote -v`
​**示例**​：

bash

复制

```bash
git remote -v
# 输出示例：
# origin  https://github.com/your-username/your-repo.git (fetch)
# origin  https://github.com/your-username/your-repo.git (push)
```

## 五、撤销与回溯（救急必备）

### 1. 撤销工作区的修改

**用途**：丢弃工作区中某个文件的修改（回到最后一次 `git add` 或 `git commit` 的状态）。
​**语法**​：`git checkout -- <文件名>`
​**示例**​：

bash

复制

```bash
# 丢弃 README.md 的修改
git checkout -- README.md
```

### 2. 撤销暂存区的修改

**用途**：将暂存区的文件移回工作区（比如不小心 `git add` 了不该加的文件）。
​**语法**​：`git reset HEAD <文件名>`
​**示例**​：

bash

复制

```bash
# 将 src/index.js 从暂存区移回工作区
git reset HEAD src/index.js
```

### 3. 撤销最后一次提交

**用途**：回退到上一次提交（有两种模式：保留修改/彻底删除）。
​**语法**​：

- `git reset --soft HEAD^`：撤销提交，保留修改到暂存区（适合“修改提交说明”）。
- `git reset --hard HEAD^`：彻底撤销提交，修改回到工作区（**慎用**，会丢失最后一次提交的修改）。
  ​**说明**​：`HEAD^` 表示“上一个提交”，`HEAD~n` 表示“前 n 个提交”。
  ​**示例**​：

bash

复制

```bash
# 彻底撤销最后一次提交（回到上一次的状态）
git reset --hard HEAD^
```

### 4. 安全撤销提交（推荐团队协作）

**用途**：生成一个新的提交来“抵消”指定提交的修改（不会改变历史，适合已推送到远程的提交）。
​**语法**​：`git revert <提交哈希>`
​**示例**​：

bash

复制

```bash
# 撤销哈希为 a1b2c3d 的提交
git revert a1b2c3d
```

## 六、标签管理（版本发布）

标签用于**标记重要版本**（比如“v1.0.0 发布”），比提交哈希更易读。

### 1. 查看标签

**用途**：列出所有本地标签。
​**语法**​：`git tag`
​**示例**​：

bash

复制

```bash
git tag
# 输出示例：
# v1.0.0
# v1.1.0
```

### 2. 创建标签

**语法**：

- `git tag <标签名>`：创建轻量标签（无说明，推荐少用）。
- `git tag -a <标签名> -m "标签说明"`：创建附注标签（有说明，推荐）。
  ​**示例**​：

bash

复制

```bash
# 创建附注标签（标记 v1.0.0 发布）
git tag -a v1.0.0 -m "正式发布 1.0 版本"
```

### 3. 推送标签到远程

**用途**：将本地标签上传到远程仓库（默认不会自动推送）。
​**语法**​：`git push origin <标签名>` 或 `git push origin --tags`（推送所有标签）。
​**示例**​：

bash

复制

```bash
# 推送 v1.0.0 标签到远程
git push origin v1.0.0
```

### 4. 删除标签

**用途**：删除本地/远程标签。
​**语法**​：

- `git tag -d <标签名>`：删除本地标签。
- `git push origin --delete <标签名>`：删除远程标签。
  ​**示例**​：

bash

复制

```bash
# 删除本地 v1.0.0 标签
git tag -d v1.0.0

# 删除远程 v1.0.0 标签
git push origin --delete v1.0.0
```

## 七、常用技巧与注意事项

1. **提交说明规范**：用“动作+内容”格式（比如“fix: 修复登录页密码框样式”“feat: 新增购物车功能”），方便后续排查问题。
2. **解决冲突**：`git pull` 或 `git merge` 时可能遇到冲突，需手动编辑冲突文件（标记为 `<<<<<<<`、`=======`、`>>>>>>>` 的部分），然后 `git add` 和 `git commit` 解决。
3. **别名简化命令**：可以用 `git config` 设置别名（比如 `git config --global alias.st status`，之后用 `git st` 代替 `git status`）。

## 总结：Git 命令速查表

|      场景      |            命令             |
| :------------: | :-------------------------: |
|   初始化仓库   |         `git init`          |
|    查看状态    |        `git status`         |
|  添加到暂存区  |         `git add .`         |
|   提交到本地   |   `git commit -m "说明"`    |
|  查看提交历史  |     `git log --oneline`     |
| 创建并切换分支 |   `git switch -c 分支名`    |
|  推送本地分支  | `git push -u origin 分支名` |
|  拉取远程更新  |  `git pull origin 分支名`   |
| 撤销工作区修改 |  `git checkout -- 文件名`   |
|  安全撤销提交  |    `git revert 提交哈希`    |

跟着本手册练习几次，你就能熟练掌握 Git 的核心操作啦！遇到问题可随时查 Git 官方文档（https://git-scm.com/doc）～ 😊