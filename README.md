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
1.open core引导
![截屏2025-03-27 03 07 43](https://github.com/user-attachments/assets/165f7268-f1f6-44d0-9eec-767038db1fe9)

- 驱动加载顺序
![截屏2025-03-27 03 00 28](https://github.com/user-attachments/assets/df7f8473-3487-4c9e-ae3a-f05ba6e73780)

2.clover引导
![截屏2025-03-27 03 12 55](https://github.com/user-attachments/assets/30ec45fb-71f0-4628-84b7-af55589910e3)
#
到这里这里就完成了重启电脑就好了

[技术支持](https://m.tb.cn/h.6d6akvV?tk=e85zeFZn3IX)



