# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Go2Shell 是一个 macOS 工具集，通过 AppleScript 实现从 Finder 工具栏快速打开终端到当前目录。这是对原版 Go2Shell 的替代方案，支持最新的 macOS 系统（10.6.1 到 14.1+）。

## Architecture

项目包含 4 个预编译的 macOS 应用包（.app），每个都是独立的 AppleScript 应用：

- `Go2Shell_iTerm.app` - 在当前 iTerm2 窗口打开新标签页
- `Go2Shell_iTerm_newWin.app` - 打开新的 iTerm2 窗口（使用 `-n` 参数）
- `Go2Shell_Term.app` - 在当前 Terminal 窗口打开新标签页
- `Go2Shell_Term_newWin.app` - 打开新的 Terminal 窗口（使用 `-n` 参数）

### 应用结构

每个 .app 包含：
- `Contents/Resources/Scripts/main.scpt` - 编译后的 AppleScript
- `Contents/Resources/applet.icns` - 应用图标
- `Contents/Info.plist` - 应用配置和权限声明

### 核心逻辑

所有应用使用相同的模式：
1. 通过 Finder 获取当前窗口的目录路径
2. 使用 `open -a [Terminal|iTerm]` 命令打开终端应用到该路径
3. 新窗口版本添加 `-n` 参数强制打开新窗口

## Development Commands

### 反编译 AppleScript
```bash
osadecompile path/to/app.app/Contents/Resources/Scripts/main.scpt
```

### 编译 AppleScript
```bash
osacompile -o output.scpt input.applescript
```

### 查看应用配置
```bash
plutil -p path/to/app.app/Contents/Info.plist
```

### 修改图标
替换 `Contents/Resources/applet.icns` 文件

## Modification Guidelines

修改 AppleScript 逻辑时：
1. 先反编译现有的 .scpt 文件
2. 编辑 AppleScript 源码
3. 使用 `osacompile` 重新编译
4. 替换应用包中的 main.scpt 文件

修改应用元数据时：
- 编辑 `Info.plist` 中的 Bundle ID、版本号等信息
- 权限声明（NSAppleEventsUsageDescription 等）已配置完整

## Credits

主要实现来自 [wonderyue/Go2ShellAppleScript](https://github.com/wonderyue/Go2ShellAppleScript)，本项目修改了图标并添加了新窗口打开功能。
