# XiaoXin13IML_2020_hackintosh
小新13 2020款 i5 10210u黑苹果  82dk ideapad s340 

原地址https://github.com/lietxia/XiaoXinAir14IML_2019_hackintosh  
 
>air14 群 1032311345 此群有小新各种机型  

本机与air142019十分相似，但是直接用air14的efi还是会有点问题，所以这里提供一下完善方案，供同机型参考。


| CPU | i5 10210u |
| :-------: | :------------------------------------------------: |
| 核显 | 正常 |
| 独显 | mx350 不能驱动 |
| 主板 | 联想161216 |
| 硬盘 | pm981a(mzvlb512hbjq-000l2) 建议不用 |
| 声卡 | cx 8070 id=15  我定制了一个补丁，解决了耳机麦克风问题 |
| 网卡 | ac 9260 可驱动 |
| 蓝牙 | 可驱动。intel蓝牙基本可以使用，蓝牙鼠标尤其是新的蓝牙鼠标有问题，已知可用的有小米鼠标和罗技鹅卵石。 |
| 触控板 | GPIO中断，pin=4B 如果有问题重建缓存 |
| 系统版本 | Catalina,Big Sur |



# 安装办法：
1.使用黑果小兵镜像，如果无法进入系统，进入隐藏bios解锁cfglock，更改dvmt  
2.安装之后替换为air14最新的efi，替换触控板热补丁为ssdt-tpad-13  
>由于air14有很好的长期支持，这里不提供efi

# 完善：
* 据说西数的新固件解决了掉速和掉盘的问题。可以考虑换硬盘。但是就算性能很差的tr200固态外接，开机速度也和nvme相差无几。
~目前sn550性价比很高，但是也有小概率掉盘，sn750据说可能不兼容bigsur。但是两者都有冷数据降速的问题。~
海康威视性价比最高，后期换颗粒不推荐。
* 如果没有替换pm981a，使用外置硬盘，可能需要安装屏蔽pm981a的ssdt
* 硬盘：如果你硬盘是三星PM981A，建议换掉。或者尝试以下办法：<details>
http://bbs.pcbeta.com/viewthread-1814806-1-1.html 此贴硬盘为pm981，批次为00000，此硬盘需要根据机型定制热补丁。据说pm981a不同批次（即000L2）对苹果的兼容性不同(且pm981的兼容性比pm981a要好)，因此有的批次可能确实无法安装。我的硬盘为mzvlb512hbjq-000l2（此批次无法更新固件），外接之后无论是否安装补丁都可以安装或使用macos。内置的情况下，如果安装补丁则无法安装或使用，反而不安装补丁则可以利用恢复法安装且进入系统。经测试性能很好，虽然偶尔内核崩溃且性能有一些不稳定，但是可满足非重度使用需求。
![img](img/pm981a.jpg)</details>  
* 如果声卡不能用需要重置nvram
* 进入隐藏bios需要关闭onekeybattery 
* bigsur目前可能有一些问题。bigsur格式化的硬盘在windows下编辑可能会导致硬盘错误。catalina不会出现此现象。我已经因此损失了一个硬盘的数据。但是硬盘没有损坏，损坏后无法在windows下格式化，可以通过linux格式化解决。或者利用paragon hard disk manager mac和windows磁盘修复命令
* Windows重启进入Mac会导致声卡出错，解决办法是先关机再开机（冷重启）
* 如果hdmi有问题，需要增加boot arg igfxonln=1 如果仍然有问题，增加igfxagdc=0  
* ~如果触控板不能用，可以1.更新驱动  
2.使用3月26日的触控板ssdt~
* 驱动触控板需要按照i2cservices，i2cgpio，i2c这样的顺序
* 目前voodooinput已经整合，如果不删除，可能会前后冲突，需要删除冲突项。
* intel网卡驱动可用，但是需要删除pci里0x1c和0x1c/0x0两项。
* 目前intel可连802.11n5g，不可连接wifi5。速度大约在100mbps。只是上行速度略慢，缺少airdrop等功能。
* 可以考虑换dw1820a，在此机器上不需要屏蔽针脚。速度大约是400mbps。如果想用随航需要添加此网卡的pci项。不要启用airportbrcm4360_injector.kext更好的有dw1830 dw1560。据说博通网卡bigsur下都可能出问题。
* ~鉴于intel网卡已经稳定，usb网卡已经没有必要。usb网卡comfast 811ac可用。usb网卡不可知连接协议，但是至少能连接802.11n的5g。速度大约是70mbps。~
* ~也可以买个usb转网线接头，连路由使用。这个转接头可能需要重新插拔才能使用~
* ~如果usb网卡驱动无法安装，使用github上的wireless usb adapter。新版本bigsur可以安装驱动，但是可能无法安装菜单栏应用。可以用unpkg解压安装包手动打开应用。~
* ~这个usb网卡在我的电脑上偶尔无法识别，但是重新插拔可以解决，可能是接口不严。~
* ~~如果偶尔进不去系统，可以用黑果小兵的ssdt代替，并且配置好oc。不要复制names3 deepldle pci0.lpcbwakeaoac~~
* 如果使esp分区取消隐藏，可能会导致windows无法休眠，重建esp分区
* 由于esp分区不适合取消隐藏，可以再分一个引导分区放oc，而且重置nvram时也能防止被清除
* 能进入系统的最低条件是，使用新版官方opencore，安装ssdtec，lilu，virtualsmc，openruntime和hfsplus或者删除uefi此项，misc security vault选择optional。  
clover的一个方案是，安装ssdtec，lilu，virtualsmc，注入smbios。使用apfs.efi(此驱动来自黑果小兵，具体未知)，或者选择preboot进系统。可以参照oc配置quirks。clover现在是基于opencore开发的，在我这开机较慢。相对来说不太好用。
* 远景opencore mod可以不注入acpi补丁进入windows。使用图形gui需要pickermode选external，uefi启用opencanopy。图形界面几乎不会增加开机时间
* 本电脑如果续航不足，可以考虑买一个诱骗线和支持65w快充的移动电源。
* 如果无法登陆app store，把网卡设置为en0，这个可以百度
* 经测试catalina使用nas做时间机器十分简单,无需netatalk等软件包,也不需要openwrt,只需要一个共享文件夹.bigsur就算使用软件包时间机器也无法使用.可能是出于某种安全问题.
* ~openwrt可以配置时间机器 https://openwrt.org/zh/docs/guide-user/services/nas/netatalk_configuration?s[]=netatalk  ntfs会出现各种问题，最好用ext4.如果不是高速无线网络,可以考虑用网线连接,或者硬盘连接电脑恢复,由于恢复模式无法识别ext4,完全恢复时可以将镜像拷贝出来使用hdiutil attach挂载,或者利用现有的纯净版恢复镜像安装新系统,安装fuse-ext,使用终端设置镜像为时间机器,利用迁移助理恢复  
实际上也可以用smb备份，但是无法识别。可以用硬盘连接设置为时间机器后用迁移助理恢复。反而不用考虑ext4的问题。~
* 目前bigsur已经整合ntfs挂载工具
* 最好关了快速启动，而且不要使用休眠否则mac读写ntfs可能会损坏数据


# 感谢：
lietxia,win1010525维护的efi
