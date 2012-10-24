1. 简介

usb_modeswitch是一个模 式切换工具，用于控制含有多个USB子设备的USB设备。如果你使用过3G的无线上网卡，你应该会很清楚的了解到这一点。具体点来说，目前一些新的USB 设备在内部含有windows驱动，当你第一次插入的时候，它作为一个闪存，并提示你安装驱动。
在安装驱动之后，驱动会自动切换USB设备的模式，存储设备将会消失（大多数情况），新的设备将会产生（如USB类型的Modem）。这种特征被无线设备的制造商称其为“免CD”的设备。
目前许多这种设备都可以在Linux的驱动下工作，如"usb-storage"（存储设备的驱动模块）和"options"（高速Modem的驱动模块)，接下来的事情就是如何从存储设备到Modem的切换。
USB_ModeSwitch从1.0.3以后的版本集成到udev（设备管理器）上，使得其工作完全自动化。你可以通过修改配置文件来设置usb_modeswitch的参数。
您可以从本文的参考链接中获取最新的版本。需要注意的是安装时你需要安装usb-modeswitch-data的包，其中包含了设备数据库和规则文件。

2. 如何使用

usb_modeswitch由几个组件来共同协同工作。
* /lib/udev/rules.d/40-usb_modeswitch.rules - udev的规则文件，如果设备ID（制造商/产品）被识别就启动usb_modeswitch。
* /lib/udev/usb_modeswitch - 一个shell脚本调用实际的usb_modeswitch. 
* /usr/sbin/usb_modeswitch_dispatcher - 检查设备并使用选择的设备文件来运行二进制程序，需要"tcl"才能运行。
* /etc/usb_modeswitch.conf - 全局的配置文件，用于调试时启用日志或禁止切换。
* /etc/usb_modeswitch.d - 该文件夹包含了针对每一个设备的独立的设置信息文件，用设备的ID来命名，如果您的设备ID出现在文件名字中，那么即使型号不同也有机会正常工作。
* /usr/sbin/usb_modeswitch - 完成切换工作的二进制程序。
通 常在切换模式后会有超过一个的串口被创建，并不是所有的端口都是标准的串口，也有一些会接受AT指令，但最终只有一个有效的端口为中断传输端口。从版本 1.1.2，usb_modeswitch会为正确的端口创建一个符号链接，/dev/gsmmodem，在这之前 ModemManager（NetworkManager的组件）必需检测能够正确工作的端口。

3. 调试方法

更改/etc/usb_modeswitch.conf文件。
#EnableLogging=0
EnableLogging=1
这会输出日志到/var/log/usb_modeswitch_xxx，xxx为设备名。

参考链接：
http://www.draisberghof.de/usb_modeswitch/