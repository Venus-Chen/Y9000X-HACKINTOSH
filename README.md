# Y9000X-HACKINTOSH
* **Y9000X-2020款黑苹果EFI文件**
* **OpenCore版本已更新到最新的V0.8.6版本**
* **已修改EFI中ACPI文件夹下的所有的acpi文件，使其完美兼容双系统，可以直接从OpenCore启动界面启动Windows(10/11)，不会再蓝屏显示ACPI_ERROR**

**配置** 
* 主板：  HM370
* CPU：  i7-9750H
* 分辨率：1080P
* 硬盘：  PM981a（黑苹果不稳定，自行添加了西数SN750 1T）
* 网卡：  Intel AX200

前期准备

**安装第二个硬盘**

**关闭secure boot**

**切换硬盘模式为 AHCI**

**由于目前的机型为16,4，可能导致安装时出现低版本的系统无法正常安装的问题，请使用10.15.6镜像安装。**

* pm981a无法安装黑苹果，所以必须更换或增加硬盘。由于有pm981a存在，系统在访问pm981a硬盘文件时会导致死机重启。必须拆下三星的这块硬盘或者屏蔽这块硬盘。本efi默认屏蔽这块硬盘且pm981在第一硬盘位，如果你选择直接拆下这块硬盘，请删除EFI/OC/ACPI/SSDT-DNVMe.aml，并且在config.plist中禁用此ACPI的配置。
* 目前的config.plist是按照CFG LOCK解锁后配置的，如未解锁，请设置`AppleCpuPmCfgLock`和`AppleXcpmCfgLock`为true，Y9000X 2020款的CFG Lock的offset为0x3E,需要修改SaSetup和CPUSetup中对应值，可以在Windows系统下面下载[这个仓库](https://github.com/xiaoMGitHub/LEGION_Y7000Series_Insyde_Advanced_Settings_Tools) 的工具用于解锁CFG Lock 
* CFG Lock解锁完成后，可以在启动界面可以选择VerifyMsrE2来检查CFG Lock是否已解锁，可以参考[这篇文章](https://blog.daliansky.net/undefined.html)（因为BigSur中MacOS的磁盘加载机制发生了变化，系统磁盘是以快照的形式加载的，如果需要使用Hackintool这个工具去查看的CFG Lock的状态会特别烦，需要关闭SIP，然后再卸载系统快照，等等，特别烦琐，最后查看完毕后还需要重新打开SIP，否则系统更新就会有问题，所以直接使用VerifyMsrE2.efi这个工具来检查即可，简单、快捷、易用），但是只能参考其中的VerifyMsrE2工具的用法，因为CFGLock.efi工具对Y9000X无效，无法使用
* **修改BIOS ROM有风险，请谨慎操作**
* **`config.plist`中的个人的机器三码信息已删除，需要自己生成后填入**
* 安装 ALCPlugFix-Swift 来修复扬声器切换异常问题, 终端使用以下命安装或卸载 ALCPlugFix
```
  bash -c "$(curl -fsSL https://gitee.com/YasuStudio/fix-speaker-y9000x/raw/master/FixSpeaker-Y9000X.sh)"
```

# 本EFI特色
* 基于本机原始DSDT制作了触控板热补丁，实现GPIO中断工作模式，采用预置变量法禁用原生备，无需重命名
* 使用重命名EC方式，笔记本更适合重命名而非仿冒EC控制器
* 添加了显示补丁，显存2048M，原生HiDPI显示
* 屏蔽PM981a，防止访问pm981a导致的系统不稳定
* 声卡加入延迟启动参数`alc-delay`,启动后声卡不加载的情况大幅度降低
* 双系统可以在开机的时候自由切换，双系统选择的延时时间为5秒

# 工作
* 屏幕显示，背光调节正常
* 耳机，麦克风，摄像头、扬声器
* Wi-Fi 蓝牙
* 触控板全手势支持，工作模式为GPIO中断
* 电源管理，USB接口正常
* 读卡器
* type-c接口可以正常接U盘，接type-c扩展坞以太网卡、HDMI，DP工作正常
* 外接显示器音频输出正常

# 不工作
* 指纹
* handoff airdrop 需要更换博通的网卡模块才可以解决

# 不完美工作
* fn热键部分正常：fn+F1(静音)、fn+F2(音量减小)、fn+F3(音量增大)、fn+F6(禁用触摸板)，其余的fn热键均不正常（键盘背光灯和电源指示灯的快捷键不受影响）
* 屏幕背光亮度调节的快捷键是：fn+K(屏幕变暗)、fn+P(屏幕变亮)
* 添加了声音修复补丁后，如果将外接电源拔除，那么过一会儿后扬声器就会没有声音，但是重新插上电源线再过一会儿后，扬声器声音又恢复正常，此问题暂时无解
* 使用AirportItlwm.kext驱动Intel AX200网卡可能会出现间接性断连，但是很快就又能重新连接成功的情况，如果重连失败，那么就需要手动重新连接，对此现象有不满的话，可以更换博通的网卡模块
