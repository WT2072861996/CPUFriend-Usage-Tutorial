# CPUFriend-Usage-Tutorial
## 准备文件
![截屏2025-03-27 01 50 39](https://github.com/user-attachments/assets/8acff8ca-8129-4f32-ae29-0d68a3600c28)

1.下载CPUCFriend文件[下载](https://github.com/acidanthera/CPUFriend)
- 把CPUFriend.kext--ResourceConverter.sh拖到桌面

2.MAC-xxxxxx.plist文件获取
- 访达前往这个文件地址

  ```
  /System/Library/Extensions/IOPlatformPluginFamily.kext/Contents/PlugIns/X86PlatformPlugin.kext/Contents/Resources/
  ```
- 找到和你电脑Board ID一样的plist文件拖到桌面,到这里所有文件都准备好了就可以开始下一步了

## 开始教程
1.打开终端输入:
```
cd desktop
```
2.回车继续输入;
```
chomd +x 把桌面ResourceConverter.sh文件拖进来
```
- 列: chomd +x /Users/xxx/Desktop/ResourceConverter.sh

3.回车继续输入:
```
把桌面ResourceConverter.sh文件拖进来 --kext 把桌面的Mac-xxxxxx.plist文件也拖进来
```
- 列: /Users/xx/Desktop/ResourceConverter.sh --kext /Users/xx/Desktop/Mac-xxxxx.plist

4.回车之后桌面会生成一个CPUFriendDataProvider.kext文件
  
## 引导文件配置
