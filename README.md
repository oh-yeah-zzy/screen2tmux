# screen2tmux

将 GNU Screen 配置成 tmux 风格的配置文件，让你可以用 tmux 的快捷键操作 screen。

## 快速开始

```bash
# 复制配置文件到主目录
cp screenrc.config ~/.screenrc

# 或创建软链接
ln -s $(pwd)/.screenrc ~/.screenrc

# 启动 screen（可指定 session 名）
screen -S mysession
```

现在你可以使用 `Ctrl-b` 作为前缀键，享受 tmux 风格的操作体验！

## 主要特性

- ✅ **tmux 风格前缀键** - `Ctrl-b` 替代 screen 默认的 `Ctrl-a`
- ✅ **窗口标题管理** - 新窗口默认标题为空，手动设置后不会被覆盖
- ✅ **完整的窗口管理** - 创建、切换、重命名窗口
- ✅ **面板分割** - 水平和垂直分割，自由调整大小
- ✅ **多种导航方式** - 支持方向键和 vim 风格（h/j/k/l）
- ✅ **复制模式** - PageUp 快速查看历史输出，vi 风格键绑定
- ✅ **增强状态栏** - 显示主机名、session 名、窗口列表（编号+标题）、日期时间
- ✅ **256 色 + UTF-8** - 现代终端特性支持

### 最常用的快捷键

```
Ctrl-b c          创建新窗口
Ctrl-b "          水平分割
Ctrl-b %          垂直分割
Ctrl-b 方向键      在面板间移动
Ctrl-b PageUp     查看历史输出
Ctrl-b d          detach 会话
```

## .screenrc 配置文件详解

配置文件采用简洁的无注释格式，按功能模块组织：

### 1. 基本设置
```bash
startup_message off        # 关闭启动消息
shell -$SHELL              # 使用默认 shell
defscrollback 10000        # 滚动缓冲区 10000 行
term screen-256color       # 启用 256 色支持
mousetrack on              # 启用鼠标支持
defdynamictitle off        # 禁用动态标题，保持手动设置的窗口名不被覆盖
```

### 2. 前缀键配置
```bash
escape ^B^B                # 将前缀键从 Ctrl-a 改为 Ctrl-b（tmux 风格）
```

### 3. 窗口和面板管理
```bash
bind c screen -t ""        # 创建新窗口，默认标题为空
bind '"' split             # 水平分割（上下）
bind % split -v            # 垂直分割（左右）
bind x remove              # 关闭面板
bind & kill                # 关闭窗口
```

### 4. 窗口导航与切换
- 支持数字键直接切换（0-9）
- `Ctrl-b n/p` - 下一个/上一个窗口
- `Ctrl-b w` - 窗口列表

### 5. 面板导航
- 方向键 - `Ctrl-b ↑/↓/←/→`
- vim 风格 - `Ctrl-b h/j/k/l`

### 6. 面板大小调整
- vim 风格 - `Ctrl-b Ctrl-h/j/k/l`
- 方向键 - `Ctrl-↑/↓/←/→`（使用 ANSI 转义序列实现）

### 7. 状态栏配置
```bash
hardstatus string '%{= kG}[ %{R}%H %{g}][ %{Y}%S %{g}][%= %{= kw}%-w%{= Kr}[%n %t]%{= kw}%+w%= %{g}][%{B} %m-%d %{W}%c %{g}]'
```

状态栏显示（从左到右）：
- `[ 主机名 ]` - 红色主机名
- `[ session名 ]` - 黄色 session 名（通过 `screen -S` 设置）
- 窗口列表 - `编号 标题` 格式，当前窗口用红底白字方括号高亮
- `[ 月-日 时间 ]` - 蓝色日期 + 白色时间

示例：`[server01][ mysession ][  0 web  1 logs [2 editor] 3   ][ 10-01 18:30 ]`

### 8. 其他功能
- `Ctrl-b ,` - 重命名窗口（设置后标题不会被程序覆盖）
- `Ctrl-b [` / `PageUp` - 进入复制模式（vi 风格键绑定）
- `Ctrl-b r` - 重新加载配置
- `Ctrl-b z` - 最大化/恢复面板

## 窗口标题管理说明

本配置的一个重要特性是**窗口标题的稳定性**：

### 配置原理
- `defdynamictitle off` - 禁用动态标题功能
- `bind c screen -t ""` - 新窗口默认标题为空

### 行为说明
1. **新窗口**：使用 `Ctrl-b c` 创建的新窗口标题默认为空（只显示编号）
2. **手动设置**：使用 `Ctrl-b ,` 设置窗口标题后，标题会保持不变
3. **不被覆盖**：即使运行 vim、htop 等程序，窗口标题也不会被程序名覆盖

这与默认的 screen 行为不同，让你可以更好地组织和管理多个窗口。

## screen vs tmux

虽然配置尽量模拟 tmux 的行为，但 screen 和 tmux 在底层机制上仍有差异：

- ✅ 大部分常用快捷键完全一致
- ✅ 窗口标题管理与 tmux 类似（手动设置后保持不变）
- ⚠️ 某些高级功能（如窗格同步输入）screen 不支持
- ⚠️ 状态栏自定义能力不如 tmux 丰富

如果你需要频繁在 screen 和 tmux 之间切换，或者在只有 screen 的服务器上工作，这个配置可以让你保持一致的操作习惯。

## License

MIT