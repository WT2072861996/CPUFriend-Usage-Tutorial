# CPUFriend Usage Tutorial

Optimize Hackintosh CPU power management with custom `CPUFriendDataProvider.kext`.

![macOS](https://img.shields.io/badge/Platform-macOS-0071BC)
![Tool](https://img.shields.io/badge/Tool-CPUFriend-blue)
![Usage](https://img.shields.io/badge/Purpose-CPU_Power_Management-green)

---

## Overview

This guide walks through using [CPUFriend](https://github.com/acidanthera/CPUFriend) to generate a custom CPU power management profile for your Hackintosh.

## Prerequisites

| File | Source |
|:----|:-------|
| `CPUFriend.kext` | [acidanthera/CPUFriend](https://github.com/acidanthera/CPUFriend) |
| `ResourceConverter.sh` | Same repository |
| `MAC-xxxxxx.plist` | `/System/Library/Extensions/IOPlatformPluginFamily.kext/.../X86PlatformPlugin.kext/.../Resources/` |

## Steps

**1.** Download CPUFriend and place `CPUFriend.kext` + `ResourceConverter.sh` on Desktop.

**2.** Locate your board's `.plist` file:

```
/System/Library/Extensions/IOPlatformPluginFamily.kext/Contents/PlugIns/
  X86PlatformPlugin.kext/Contents/Resources/
```

Find the file matching your Board ID and copy it to Desktop.

**3.** Generate the custom kext:

```bash
./ResourceConverter.sh --kext /path/to/MAC-xxxxxx.plist
```

**4.** Place both `CPUFriend.kext` and `CPUFriendDataProvider.kext` in your bootloader's kexts directory.

## Verification

- Use **Intel Power Gadget** to verify frequency scaling
- Use **Hackintool** → Power to check CPU frequency tables

---

*Reference: [acidanthera/CPUFriend](https://github.com/acidanthera/CPUFriend)*
