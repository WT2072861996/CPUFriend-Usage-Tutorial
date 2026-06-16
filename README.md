# ⚡ CPUFriend 完全指南

**Hackintosh CPU 电源管理优化 · 频率调节 · 功耗管理 · 自定义 DataProvider**

<div align="center">

![macOS](https://img.shields.io/badge/Platform-macOS-0071BC?style=flat-square)
![Tool](https://img.shields.io/badge/Tool-CPUFriend-blue?style=flat-square)
![Purpose](https://img.shields.io/badge/Purpose-CPU_Power_Management-green?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

**[📖 概述](#概述) · [🛠️ 前置要求](#前置要求) · [📋 步骤](#详细步骤) · [✅ 验证](#验证) · [❓ FAQ](#常见问题)**

</div>

---

## 📌 快速概览

| 项目 | 说明 |
|:---:|:-----|
| **CPUFriend** | 内核扩展，用于 CPU 电源管理定制 |
| **用途** | 启用 macOS 原生 XCPM，实现动态频率调节 |
| **原理** | 注入自定义 CPU 频率/电压表 |
| **收益** | 更低功耗 · 更好散热 · 系统更稳定 |
| **难度** | ⭐ 简单（自动化工具） |
| **必需性** | ⭐⭐⭐ 推荐（性能/稳定性) |

---

## 🎯 概述

在 Hackintosh 中，系统默认使用 Apple 定义的 CPU 频率表。这些预设参数可能：

- 🔴 CPU 不会降低频率（高功耗）
- 🔴 无法自适应负载（性能浮动）
- 🔴 散热器风扇持续高转（噪音）

**CPUFriend 解决方案**：

✅ 根据实际 CPU 创建自定义频率表  
✅ 启用动态调频（XCPM）  
✅ 优化功耗和性能平衡  

---

## 🛠️ 前置要求

### 📋 必需文件

| 文件 | 来源 | 说明 |
|:-----|:-----|:-----|
| `CPUFriend.kext` | [acidanthera/CPUFriend](https://github.com/acidanthera/CPUFriend) | 主程序 |
| `ResourceConverter.sh` | 同上仓库 | 转换脚本 |
| `MAC-xxxxxx.plist` | 运行中的 macOS | CPU 资源文件 |

### 💻 系统要求

- ✅ macOS 已成功启动和运行
- ✅ 拥有基本的命令行使用经验
- ✅ 已启用 SSDT-PLUG（CPU 电源管理）

### 🔍 检查系统准备

```bash
# 验证 XCPM 已启用
sysctl -a | grep kern.hv_support
# 输出：kern.hv_support: 1 ✅

# 查看当前 CPU 频率（需 Intel Power Gadget）
# 应显示动态变化
```

---

## 📋 详细步骤

### 1️⃣ 下载 CPUFriend

```bash
# 克隆官方仓库
git clone https://github.com/acidanthera/CPUFriend.git
cd CPUFriend

# 查看最新版本
ls Release/
```

### 2️⃣ 定位 CPU 资源文件

```bash
# 找到 Board ID（根据你的 SMBIOS）
# 例：iMac19,1 → Mac-AA3F2EB81D572B0D

# 路径
/System/Library/Extensions/IOPlatformPluginFamily.kext/Contents/PlugIns/X86PlatformPlugin.kext/Contents/Resources/

# 查看可用文件
ls /System/Library/Extensions/IOPlatformPluginFamily.kext/Contents/PlugIns/X86PlatformPlugin.kext/Contents/Resources/ | grep MAC

# 例如找到：Mac-AA3F2EB81D572B0D.plist
```

### 3️⃣ 复制文件到工作目录

```bash
# 在桌面创建工作目录
mkdir ~/Desktop/CPUFriend_Work
cd ~/Desktop/CPUFriend_Work

# 复制 CPUFriend.kext
cp -R ~/Downloads/CPUFriend/Release/CPUFriend.kext .

# 复制脚本
cp ~/Downloads/CPUFriend/ResourceConverter.sh .
chmod +x ResourceConverter.sh

# 复制 CPU plist
cp "/System/Library/Extensions/IOPlatformPluginFamily.kext/Contents/PlugIns/X86PlatformPlugin.kext/Contents/Resources/Mac-AA3F2EB81D572B0D.plist" .
```

### 4️⃣ 生成 CPUFriendDataProvider.kext

```bash
# 方式 A：完全自动（推荐新手）
./ResourceConverter.sh --kext ./Mac-AA3F2EB81D572B0D.plist

# 方式 B：低性能模式（笔记本/低噪）
./ResourceConverter.sh --kext --low-power ./Mac-AA3F2EB81D572B0D.plist

# 方式 C：高性能模式（台式机）
./ResourceConverter.sh --kext --high-power ./Mac-AA3F2EB81D572B0D.plist

# 输出应包含：CPUFriendDataProvider.kext
```

### 5️⃣ 部署到 EFI

```bash
# 复制两个 kext 到 EFI 目录
cp -R CPUFriend.kext /Volumes/EFI/EFI/OC/Kexts/
cp -R CPUFriendDataProvider.kext /Volumes/EFI/EFI/OC/Kexts/

# 在 config.plist 中添加引用
# （使用 OCAT 或 ProperTree）
```

### 6️⃣ config.plist 配置

在 `Kernel → Add` 数组中添加：

```xml
<dict>
    <key>Enabled</key>
    <true/>
    <key>BundlePath</key>
    <string>CPUFriend.kext</string>
    <key>Comment</key>
    <string>CPU Power Management</string>
    <key>ExecutablePath</key>
    <string>Contents/MacOS/CPUFriend</string>
    <key>PlistPath</key>
    <string>Contents/Info.plist</string>
</dict>
<dict>
    <key>Enabled</key>
    <true/>
    <key>BundlePath</key>
    <string>CPUFriendDataProvider.kext</string>
    <key>Comment</key>
    <string>Custom CPU Frequency Data</string>
    <key>ExecutablePath</key>
    <string></string>
    <key>PlistPath</key>
    <string>Contents/Info.plist</string>
</dict>
```

### 7️⃣ 重启并验证

```bash
# 重启系统
sudo reboot

# 启动后检查 kext 是否加载
kextstat | grep -i cpufriend
# 应显示：CPUFriend.kext 和 CPUFriendDataProvider.kext
```

---

## ✅ 验证

### 🔧 使用 Hackintool

1. 下载 [Hackintool](https://github.com/benbaker76/Hackintool)
2. 打开应用
3. 去 **Power** 标签
4. 应显示 CPU 频率表
5. 确保 P-States 数量 > 1（代表动态调频工作）

### 📊 使用 Intel Power Gadget

1. 下载 [Intel Power Gadget](https://software.intel.com/en-us/articles/intel-power-gadget)
2. 打开应用
3. 监控 CPU 频率变化
4. 应显示：空闲时低频 → 满载时高频

### 💻 命令行检查

```bash
# 查看 CPU 频率（使用脚本）
while true; do
  echo "CPU 频率：" $(sysctl -n hw.productname) $(sysctl -n hw.cpufrequency_current)
  sleep 1
done

# 或用 Geekbench 测试性能
```

---

## ❓ 常见问题

### Q1：找不到 CPU plist 文件？

**A**：你的 SMBIOS 型号可能不匹配。

```bash
# 检查你设置的 SMBIOS
# 然后在此处查找对应文件
/System/Library/Extensions/IOPlatformPluginFamily.kext/Contents/PlugIns/X86PlatformPlugin.kext/Contents/Resources/

# 如果目录为空，说明 SMBIOS 不兼容
```

### Q2：生成后 kext 无法加载？

**A**：检查以下几点：

1. ✅ kext 权限正确（755）
2. ✅ config.plist 中 Enabled 为 true
3. ✅ 依赖 kext 已加载（Lilu 必需）
4. ✅ 没有重复条目

### Q3：系统不稳定/重启？

**A**：CPUFriendDataProvider 可能设置过激进。

```bash
# 解决方案：
1. 移除 CPUFriendDataProvider.kext
2. 重新生成，使用 --low-power 标志
3. 重启测试
```

### Q4：CPU 频率没有变化？

**A**：可能未启用 XCPM。检查：

```bash
# 验证 SSDT-PLUG 已加载
ioreg -l | grep -i "plugin-type"
# 应显示：plugin-type = 1

# 如未显示，添加 SSDT-PLUG.aml（见 SSDT 指南）
```

---

## 🔗 参考资源

### 官方项目

- **[CPUFriend 官方仓库](https://github.com/acidanthera/CPUFriend)** - 源码和文档
- **[Lilu.kext 依赖](https://github.com/acidanthera/Lilu)** - 内核补丁框架（必需）

### 相关指南

- [SSDT-PLUG 电源管理](https://dortania.github.io/Getting-Started-With-ACPI/) - 必读
- [Hackintool 使用指南](https://github.com/benbaker76/Hackintool) - 诊断工具
- [OpenCore 配置参考](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Configuration.pdf)

### 相关工具

| 工具 | 功能 |
|:-----|:-----|
| **Hackintool** | CPU 频率表查看 |
| **Intel Power Gadget** | 实时频率监控 |
| **Geekbench** | 性能基准测试 |

---

## ⚠️ 注意事项

1. **备份重要**：操作前备份 EFI
2. **顺序重要**：CPUFriendDataProvider 必须在 CPUFriend 之后加载
3. **系统特定**：每个 CPU 型号的 plist 文件都不同
4. **XCPM 前置**：必须先启用 SSDT-PLUG
5. **测试充分**：部署后要充分测试系统稳定性

---

## 📄 许可证

MIT License - 详见 [LICENSE](LICENSE)

> ⚠️ 本教程仅供学习和研究使用

---

<p align="center">
  <b>为 Hackintosh 社区贡献 ❤️</b><br/>
  <sub>最后更新：2026年 · Acidanthera CPUFriend · OpenCore 兼容</sub>
</p>
