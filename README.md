# 🔧 CPUFriend 使用教程

<p align="center">
  <img src="https://img.shields.io/badge/工具-CPUFriend-blue" alt="CPUFriend" />
  <img src="https://img.shields.io/badge/平台-macOS-0071C5?logo=apple&logoColor=white" alt="macOS" />
  <img src="https://img.shields.io/badge/用途-CPU_电源管理-green" alt="Usage" />
</p>

一份简洁的 CPUFriend 使用指南，帮助你优化 Hackintosh 的 CPU 电源管理。

---

## 📦 准备文件

![准备文件](https://github.com/user-attachments/assets/8acff8ca-8129-4f32-ae29-0d68a3600c28)

---

### Step 1：下载 CPUFriend

1. 从 [acidanthera/CPUFriend](https://github.com/acidanthera/CPUFriend) 下载最新版本
2. 将 `CPUFriend.kext` 和 `ResourceConverter.sh` 拖到桌面

---

### Step 2：获取 MAC-xxxxxx.plist 文件

1. 打开「访达」，前往路径：

   ```
   /System/Library/Extensions/IOPlatformPluginFamily.kext/Contents/PlugIns/X86PlatformPlugin.kext/Contents/Resources/
   ```

2. 找到与你电脑 **Board ID** 对应的 `.plist` 文件，拖到桌面

---

### Step 3：生成自定义电源管理文件

在终端运行以下命令生成自定义 `CPUFriendDataProvider.kext`：

```bash
# 将 /path/to/your/plist 替换为你实际的 plist 文件路径
./ResourceConverter.sh --kext /path/to/your/plist.plist
```

> 生成的 `CPUFriendDataProvider.kext` 需与 `CPUFriend.kext` 同时放入 OC/Clover 的 kexts 目录。

---

## ✅ 检查是否生效

- 使用 Intel Power Gadget 查看 CPU 频率和功耗
- 使用 Hackintool 查看 CPU 变频档位

---

<div align="center">
  <img src="https://img.shields.io/badge/参考-acidanthera-00A3E0?logo=github" alt="Reference">
</div>
