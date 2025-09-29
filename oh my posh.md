# Oh My Posh for Windows 安装与配置指南（小白友好版）

## 一、前言

Oh My Posh 是一款**终端美化工具**，支持 PowerShell、WSL、VS Code 集成终端等，能让你的命令行界面变得炫酷又个性！本指南专为 Windows 小白设计，一步步教你安装和配置。

## 二、安装前准备

### 1. 系统要求

- Windows 10 或更高版本（推荐 Win11）
- 已安装 PowerShell 7+（或 Windows 自带的 PowerShell 5.1 也可，但推荐 7+）
- 已安装 Windows Terminal（可选但强烈推荐，界面更美观）

### 2. 工具准备（按需）

- 若用 Chocolatey 安装：需先安装 Chocolatey
- 若用 Scoop 安装：需先安装 Scoop

## 三、正式安装 Oh My Posh

### 方法 1：用 Winget 安装（推荐，最简单）

Winget 是 Windows 自带的应用商店工具（Win11 必带，Win10 需手动安装）。
打开 ​**PowerShell 7**​（或 Windows Terminal 里的 PowerShell 标签），输入以下命令并回车：

powershell

复制

```powershell
# 检查是否已安装 Winget（若提示“找不到命令”，跳过此步直接执行下一条）
winget --version

# 安装 Oh My Posh（输入 Y 确认）
winget install --id JanDeDobbeleer.OhMyPosh
```

### 方法 2：用 PowerShell 脚本安装（通用）

在 PowerShell 中输入以下命令，自动下载并安装：

powershell

复制

```powershell
# 执行安装脚本（输入 A 确认所有提示）
Invoke-Expression "& { $(irm https://ohmyposh.dev/install.ps1) }"
```

### 方法 3：用 Chocolatey/Scoop 安装（适合已用包管理器的用户）

- Chocolatey：`choco install oh-my-posh`
- Scoop：`scoop bucket add extras && scoop install oh-my-posh`

## 四、验证安装是否成功

安装完成后，重启 PowerShell 或 Windows Terminal，在 PowerShell 标签页输入：

powershell

复制

```powershell
oh-my-posh init pwsh | Invoke-Expression
```

如果看到终端出现类似下面的炫酷提示符（带图标和颜色），说明安装成功！
https://ohmyposh.dev/docs/assets/getting-started/pwsh.png

## 五、配置主题（选你喜欢的风格）

Oh My Posh 内置了很多主题，可自由切换！

### 1. 查看所有可用主题

在 PowerShell 中输入：

powershell

复制

```powershell
Get-PoshThemes
```

会列出所有主题名称和预览（比如 `agnoster`、`catppuccin`、`powerlevel10k` 等）。

### 2. 设置默认主题

#### 方式 1：临时设置（仅当前会话有效）

powershell

复制

```powershell
Set-PoshPrompt -Theme 主题名  # 例如：Set-PoshPrompt -Theme catppuccin
```

#### 方式 2：永久设置（推荐）

让每次打开终端都自动加载主题：

1. 打开 PowerShell 配置文件（若不存在会自动创建）：

   powershell

   复制

   ```powershell
   notepad $PROFILE
   ```

2. 在打开的记事本中添加一行（替换

    

   ```
   catppuccin
   ```

    

   为你喜欢的主题名）：

   powershell

   复制

   ```powershell
   Set-PoshPrompt -Theme catppuccin
   ```

3. 保存并关闭记事本，重启终端生效！

## 六、解决字体问题（关键！否则可能乱码）

大部分主题需要**特殊字体**（如 Nerd Fonts）才能正确显示图标（比如 🔗、📁 等）。若终端显示乱码或方块，按以下步骤解决：

### 1. 下载 Nerd Fonts 字体

访问 Nerd Fonts 官网，选择喜欢的字体（推荐 `FiraCode Nerd Font` 或 `Cascadia Code Nerd Font`），下载 `.zip` 包。

### 2. 安装字体到 Windows

- 解压下载的 `.zip` 包，打开文件夹。
- 右键每个 `.ttf` 或 `.otf` 字体文件 → 选择“安装”（或“为所有用户安装”）。

### 3. 配置终端使用该字体

- **Windows Terminal**：
  打开设置（`Ctrl+,`）→ 找到“外观”→“字体”→ 选择刚安装的 Nerd Font（如 `FiraCode Nerd Font Mono`）。
- **VS Code 集成终端**：
  打开设置（`Ctrl+,`）→ 搜索 `terminal.integrated.fontFamily` → 输入字体名（如 `FiraCode Nerd Font`）。

## 七、集成到常用终端

### 1. Windows Terminal（推荐）

- 打开 Windows Terminal 设置 → “配置文件”→ 选择你的 PowerShell 配置文件 → 在“命令行”中确保路径是 `pwsh.exe`（或 `powershell.exe`）。
- 若提示符不生效，重启 Windows Terminal 即可。

### 2. VS Code 集成终端

- 打开 VS Code → `Ctrl+Shift+P` 打开命令面板 → 输入 `Preferences: Open Settings (JSON)`。

- 添加以下配置（确保 PowerShell 终端使用 Oh My Posh）：

  json

  复制

  ```json
  "terminal.integrated.profiles.windows": {
    "PowerShell": {
      "source": "PowerShell",
      "icon": "terminal-powershell",
      "path": "C:\\Program Files\\PowerShell\\7\\pwsh.exe",  # 替换为你的 pwsh 路径
      "args": ["-NoExit", "-Command", "oh-my-posh init pwsh | Invoke-Expression"]
    }
  },
  "terminal.integrated.defaultProfile.windows": "PowerShell"
  ```

## 八、自定义主题（进阶，可选）

如果内置主题不满意，可自己创建主题！

1. 生成模板：

   powershell

   复制

   ```powershell
   New-PoshTheme -Name MyTheme -Path ~/.poshthemes  # 生成模板到 ~/.poshthemes 文件夹
   ```

2. 用文本编辑器打开 `~/.poshthemes/MyTheme.omp.json`，修改颜色、图标等配置。

3. 应用自定义主题：

   powershell

   复制

   ```powershell
   Set-PoshPrompt -Theme ~/.poshthemes/MyTheme.omp.json
   ```

## 九、常见问题解决

1. **终端不显示图标/乱码**：
   检查是否安装了 Nerd Fonts 并设置为终端字体！
2. **主题不生效**：
   - 确认 `$PROFILE` 配置文件已正确添加 `Set-PoshPrompt` 命令。
   - 重启终端或执行 `oh-my-posh init pwsh | Invoke-Expression` 重新加载。
3. **安装失败**：
   尝试以管理员身份运行 PowerShell 重新安装，或切换网络后重试。



### **一、下载 Nerd Font**

1. 访问 Nerd Fonts 官网，选择一款字体（如 `Cascadia Code Nerd Font`）。
2. 点击 `Download` 按钮，选择 `.zip` 格式下载。

------

### **二、安装 Nerd Font**

#### **Windows 用户**

1. 解压下载的 `.zip` 文件，找到 `.ttf` 或 `.otf` 字体文件（如 `CascadiaCodeNF-Regular.ttf`）。
2. 右键点击字体文件 → **安装**（或双击安装）。
3. 安装完成后，字体会出现在系统的字体列表中。

#### **macOS/Linux 用户**

bash

复制

```bash
# 使用 Homebrew 安装（macOS）
brew tap homebrew/cask-fonts
brew install --cask font-cascadia-code-nerd-font

# 或手动下载后双击安装
```

------

### **三、配置终端/编辑器**

#### **Windows Terminal**

1. 打开 Windows Terminal 设置（快捷键 `Ctrl + ,`）。

2. 在 `settings.json` 中找到对应配置段（如 `"profiles"` → `"list"`）。

3. 添加

    

   ```
   fontFace
   ```

    

   属性：

   json

   复制

   ```json
   {
     "name": "My Custom Profile",
     "fontFace": "Cascadia Code NF"  // 字体名称需与实际安装名称一致
   }
   ```

#### **VSCode**

1. 打开设置（快捷键 `Ctrl + ,`）。

2. 搜索 `editor.fontFamily`。

3. 在输入框中添加：

   复制

   ```markdown
   "editor.fontFamily": "Cascadia Code NF, Consolas, 'Courier New', monospace"
   ```

#### **其他终端（如 iTerm2、Alacritty）**

1. 进入终端的配置界面，找到 `Font` 或 `Text` 设置。
2. 选择刚安装的 Nerd Font（如 `Cascadia Code NF`）。

------

### **四、验证效果**

1. 重启终端或编辑器。
2. 输入 Oh My Posh 提示符（如 `PS>`），应看到图标正常显示。

------

### **常见问题排查**

- **图标不显示**：确认字体名称与配置中完全一致（注意大小写）。
- **模糊不清**：尝试启用终端的 "ClearType" 或调整 DPI 缩放设置。
- **跨平台兼容性**：若在 Linux 上使用，确保字体已复制到 `~/.local/share/fonts/` 目录。

------

选择其他字体时，只需将上述步骤中的 `Cascadia Code NF` 替换为实际字体名称即可（如 `FiraCode NF`、`JetBrainsMono NF`）。