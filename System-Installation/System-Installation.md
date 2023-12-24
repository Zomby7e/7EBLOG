- # 用各种方法装系统

  > 计算机常识系列：如何用手中的设备，最大程度地确保旧系统出问题的时候还能上网查询问题以及恢复到一个可用的操作系统。内容包括 Android Windows GNU/Linux；电脑可以刷手机、手机也可以给装电脑系统。
  
  **注意：我只是提供思路，不对任何数据丢失或者机器变砖负责。**
  
  ## GNU/Linux
  
  ### 通用办法
  
  > 用各种系统安装 Linux
  
  ### 最简单的方案
  
  只需要下载 [balenaEtcher](https://www.balena.io/etcher) 的便携版本，Windows 和 Linux 都可以点击运行，选择需要烧录的系统镜像，选择 U 盘，进行烧写即可。推荐把它和系统镜像放在你的移动硬盘里备用。
  
  ### 其他方案
  
  1. dd 以及它的例子：
  
     ```bash
     dd if=/home/user/debian.iso of=/dev/sdc
     ```
  
     推荐的原因当然是各大发行版自带，最容易找到。
  
  2. Ventoy:
  
     兼容相当多的操作系统，用官方的安装工具安装到 U 盘，U 盘就可以用于启动 Windows/Linux，并且可以继续用于保存文件。
  
     推荐的原因是：可在 Windows/Linux 创建启动盘，支持启动的操作系统很多，并且启动另一种系统无需重新烧录 U 盘。
  
  3. Rufus (Windows only):
  
     可以烧录 Windows/Linux 系统到 U 盘，但是仅限于 Windows 系统。
  
     推荐原因：软件尺寸小，简单好用，适合 Windows 过渡到 Linux 的用户。
  
  4. UNetbootin (电脑系统):
  
     兼容性不错的图形化烧录工具。
  
  5. 各大发行版的图形化烧录工具：
  
     - Fedora Media Writer
     - SUSE Studio Imagewriter
     - Startup Disk Creator
  
  6. EtchDroid (Android only)
  
     手机烧写 Linux 镜像的另一个方案。
  
  ## Windows
  
  > 用各种系统安装 Windows
  
  不同于 Linux，官方的 Windows 安装向导不带图形化分区工具，Linux 镜像烧录工具不一定能启动 Windows，目前最新的 Windows 11 必须使用联网账户。下面是解决这些问题的思路。
  
  ### 最推荐的方案
  
  最推荐的方案当然是 Ventoy + 自制 Windows PE. （自制 PE 镜像可以参考我的另一篇文章）
  
  优点：图形分区工具、备份原系统下的文件，U 盘依然可以日常用于保存普通文件。
  
  1. 选择一个你偏好的 Windows PE，我的选择使用自制版本，你也可以选择一个被广泛使用的版本。
  2. 把 Ventoy 烧写到 U 盘。
  3. 用 DiskGenius 快速分区（现代的计算机应使用 GPT 分区表，启动方式当然是 UEFI）
  4. 用 Dism++ 的系统恢复功能安装系统。
  
  注意：
  
  Windows 有命令行分区工具，但是很难用。如果你的硬盘使用了 MBR 分区表，安装工具很可能说你的电脑不支援 Windows, 所以还是推荐用 DiskGenius 的快速分区，它会自动创建 EFI 分区以及其他需要的分区。
  
  ### 其他方案
  
  1. WoeUSB (Linux)
  
     Linux 下烧录 Windows 镜像的工具。
  
  2. Rufus (Windows only)
  
     容量小，可以烧写 Windows 镜像也可以烧写 Linux 的。
  
  3. Ubuntu PE
  
     这是一个 Linux 发行版，可以用来安装 Windows.
  
  4. EtchDroid (Android only)
  
     手机烧写 Windows 镜像的另一个方案。
  
  ## 电脑数据备份/恢复方案
  
  ### 通常的方案
  
  - Windows 系统：
  
    用一个第三方制作的 Windows PE 系统即可直接复制文件，如果文件被删除还可以使用 PE 系统携带的文件恢复工具（如果有的话）。
  
  - Linux 系统：
  
    启动一个 live 系统，用 mount 命令挂载原系统的磁盘，再 chroot 到原来的系统，可以修复出错的系统文件、复制资料、还可以用来重置 root 密码。
  
  ### 专用的数据恢复发行版
  
  [SystemRescue](https://www.system-rescue.org/) 是一个基于 Arch Linux 系统的救援工具包，专为修复崩溃后的系统所设计。
  
  它内置了 Xfce 桌面环境以及各种实用工具：包括分区工具、数据恢复工具 Test-disk、网络备份工具和文本编辑器等。支援相当多的文件系统，包括 NTFS.
  
  你可以烧录一份用于紧急情况下的数据救援，不过更推荐多买一块移动硬盘用于备份。
  
  ## Android
  
  > 无论如何，更换手机上操作系统的前置且必须的必须条件是解锁安卓手机的 Bootloader.
  
  需要准备：
  
  - 手机 ROM 以及卡刷包
  - 数据线
  - 推荐一个 USB 存储设备（USB 转接口）
  
  需要注意的是：
  
  - 不只要**备份**工作文档或者生活照片，还要备份基带、原厂系统镜像等，防止手机无法使用。
  
  - 高通设备通常可以比较方便地获得设备的 Linux 内核源码，所以如果你想方便地使用第三方系统，尽量选择高通，并且手机厂商允许你解开 Bootloader 锁的设备。
  
  ### 电脑刷机
  
  1. 充满电，在刷机之前备份好重要的数据，首先需要 [SDK Platform Tools](https://developer.android.com/studio/releases/platform-tools) 来连接手机，以及需要下载好的第三方 recovery (通常是 twrp)，准备好刷机包。刷机包一般在 XDA 论坛发布，著名的第三方系统之一是 Lineage OS.
  
  2. 进入第三方 recovery
  
     > 参考：recovery 类似于 Windows PE/Linux LiveCD，可以用于装系统、备份文件等。Bootloader 类比为电脑的 BIOS，可以透过它启动其他系统。
  
     ```bash
     # 某些 Unix-like 系统可能需要超级用户权限
     # 先将设备启动到 bootloader (手机电源键组合也可进入)
     adb reboot bootloader
     # 启动到第三方 recovery
     fastboot boot twrp.img
     ```
  
  3. 进入第三方 recovery，备份基带（按需），双清（恢复出场设置），把 zip 格式的刷机包传进手机内置共享存储，如果因为某些原因不能传，用外置 U 盘。
  
  4. 重启并且初始化系统后，按需刷入 Magisk，如果不能解密手机，还是要外置 U 盘。
  
  ### 用手机刷另一台手机
  
  > 极端条件下的解决方案。
  
  1. 刷好 Magisk 后：去 Fox's Magisk Module Manager 寻找模块，搜索 adb 或者 fastboot 并且安装，这个模块应该包含了 adb 和 fastboot 两个命令行工具。
  
  2. 在 F-droid 安装 Terminal Emulator，进入后获取 root 权限：
  
     ```bash
     su
     ```
  
  3. 用转接头 + 数据线连接另一台手机，其他操作类似电脑。
  
  ### 备份手段和恢复工具
  
  > 这里以小米设备为例，其他设备的硬盘分区、备份方式可能各有不同。
  
  - 备份和恢复基带（丢失可能会导致不能上网）：
  
    最简单的办法就是：在 twrp 中解密手机内部共享存储，备份 EFS 分区（其他设备可能有不同的分区），打包成 zip 文档并且备份到别的地方，需要的时候重新解压到手机，再用 twrp 恢复。（推荐这个办法，滑动备份和恢复基本上是傻瓜式操作。）
  
    比较麻烦的办法就是写一个 bash 脚本用于自动备份和恢复，如果没有备份或者恢复出错，9008 或者寻找售后服务吧。
  
    可以在 twrp 中用 adb 备份，也可以获取 root 后在安卓系统，方便优先。
  
  - 备份/恢复原厂操作系统：
  
    小米手机有官方线刷包，它是一系列的脚本和镜像。线刷包包含了官方的 boot 分区、recovery 分区以及官方的操作系统等等……
  
    如果你找不到官方的线刷包，就用 twrp 备份吧，不管是 boot 分区还是 recovery，按需备份。
  
  - 操作系统损坏但是备份数据：
  
    用 twrp 解密，将所需文件备份到 USB 存储设备/电脑。
  
  