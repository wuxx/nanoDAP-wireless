# nanoDAP-wl 用户手册
* [产品介绍](#产品介绍) 
* [产品特点](#产品特点)
* [MDK配置说明](#mdk配置说明)
    * [仿真器选择](#仿真器选择)
    * [目标检测](#目标检测)
    * [烧写算法](#烧写算法)
    * [复位设置](#复位设置)
* [DAPLink使用](#DAPLink使用)
    * [拖拽烧录](#拖拽烧录)
    * [固件升级](#固件升级)
* [FAQ](#faq)
    * [在win7系统下会提示无串口驱动，应该如何安装？](#q-在win7系统下会提示无串口驱动应该如何安装)
    * [烧录失败，提示 RDDI-DAP ERROR，应该如何解决？](#q-烧录失败提示-rddi-dap-error应该如何解决)
    * [无法检测到目标，提示communication failure，应该如何解决？](#q-无法检测到目标提示communication-failure应该如何解决)
    * [CMSIS-DAP/DAPLink目前支持哪些芯片的调试烧录？](#q-cmsis-dapdaplink目前支持哪些芯片的调试烧录)
    * [DAPLink目前支持哪些芯片的U盘拖拽烧录？](#q-daplink目前支持哪些芯片的u盘拖拽烧录)

# 产品介绍
nanoDAP-WL 是实验室推出的基于cmsis-dap的无线仿真器，仿真器包括发射机/接收机，基于2.4G无线通信，可对20m范围内的目标进行调试下载、单步调试等操作，在某些有线仿真器不便调试的场景，如目标始终处于移动状态（飞行器、小车、机器人等），目标已经组装成产品形态，并且已安装在墙上或者高处等。此时使用无线仿真器能较好的解决这些场景下调试问题，有效提高研发效率。
![screenshot](https://github.com/wuxx/nanoDAP/blob/master/doc/nanoDAP2.jpg)

# 产品特点
- 支持SWD协议，典型的基于ARM Cortex-M系列芯片均支持SWD调试，常见的芯片如STM32系列，GD32系列，ATMEL-SAM系列，NORDIC-NRF51/52系列，NXP-LPC系列等芯片均支持SWD调试下载。(配图 https://github.com/blacksphere/blackmagic/wiki)
- 支持JTAG协议，配合开源调试器OpenOCD可支持全球范围内几乎所有SoC芯片的调试，如ARM Cortex-A系列、DSP、FPGA、MIPS等，因为SWD协议只是ARM自己定义的私有协议，而JTAG则是国际IEEE 1149标准。 
- 接收机支持向目标板供电（5V、3.3V），以及从目标板取电（5V、3.3V）两种方式进行工作
- 支持MDK/IAR/OpenOCD，支持Windows/Linux/Mac 下进行调试开发
- 软件基于CMSIS-DAP实现，使用USB HID协议，无需安装驱动即可下载调试
- 调试引脚与nanoDAP一致，后续可进行固件升级，支持无线串口调试


# 使用场景
1. 用于调试飞行器，小车，机器人，由于调试目标为通常处于移动状态，若使用传统下载器不仅下载比较麻烦，而且无法进行单步调试。  
2. 目标板已经组装好外壳，成为产品形态，此时传统的有线方式不便调试。  
3. 产品安装在高处，如路灯、高塔等位置，此时使用有线方式不便调试。

# 使用步骤
1. 将接收器和目标单板相接，并上电，此时可以观察到接收器开始蓝灯闪烁，等待和发射器建立连接
2. 将发射器和PC相接，上电后发射器会开始搜索接收器并蓝灯闪烁，当成功搜索到接收器后并建立连接后，则发射器和接收器均为处于蓝灯常亮的状态。
3. 亦可先连接发射器，再连接接收器，只需最终观察到双方蓝灯都从闪烁到蓝灯常亮的状态变化即说明连接正常建立。
4. 建立连接后，即可开始进行调试，此时只需想象发射器和接收器之间连接了一根虚拟的信号线即可，使用方式和nanoDAP仿真器完全一样，具体用法请见（nanoDAP用户手册）
5. 若由于目标超出通信距离（一般电磁环境下20m左右），则会出现发射器红灯常亮，此时若需要恢复通信，需要将发射器重新插拔一下，然后注意观察蓝灯连接状态即可。

## LED状态
为方便说明，称发射机称之为dap-host，因其与host PC连接。接收机则称为dap-target，因其与目标单板相接。

LED | 说明
---|---
dap-host 蓝灯慢速闪烁 | 等待和dap-target连接
dap-host 蓝灯常亮| 已经和dap-target成功建立连接
dap-host 蓝灯快速闪烁 | 正在和dap-target交换数据
dap-host 红灯常亮 | 通信出错
dap-target 蓝灯慢速闪烁 | 等待和dap-host连接
dap-target 蓝灯常亮 | 已经和dap-host成功建立连接
dap-target 蓝灯快速闪烁 | 正在和dap-host交换数据
dap-target 红灯常亮 | 通信出错

# FAQ
### Q: 在win7系统下会提示无串口驱动，应该如何安装？  
在本github仓库的driver/windows7_serial_driver/ (https://github.com/wuxx/nanoDAP/tree/master/driver/windows7_serial_driver) 目录下有CMSIS-DAP.inf，请尝试安装此驱动，大部分情况下可以正常安装使用，若仍然无法安装，请尝试目录下的另一驱动windows7_serial_driver.rar，根据压缩包里的指导进行操作即可。   
### Q: 烧录失败，提示 RDDI-DAP ERROR，应该如何解决？
主要是杜邦线的问题，请尝试换短一些的杜邦线，或者把紧密连在一起的杜邦线拉开，一般即可正常解决。
### Q: 无法检测到目标，提示communication failure，应该如何解决？
请首先排查硬件接线是否正确(GND, CLK, IO, 3V3)，然后检查目标板电源是否正常，若目标板由仿真器供电，由于USB最大输出电流只有500mA，请排查是否可能目标板供电不足。
### Q: CMSIS-DAP/DAPLink目前支持哪些芯片的调试烧录？
 典型的使用场景为对单片机进行编程调试，理论上Cortex-M系列的内核均可以使用DAP进行烧录调试，典型的芯片如STM32全系列的芯片，GD32全系列，nRF51/52系列等。
### Q: DAPLink目前支持哪些芯片的U盘拖拽烧录？
 目前DAPLink支持拖拽烧录的芯片并不算特别多，将来随着ARM社区支持力度将会增加更多芯片支持，目前支持U盘拖拽烧录的的芯片列表如下  
- stm32f072rb  
- stm32f103rb  
- stm32f207zg  
- stm32f401re  
- stm32f411re  
- stm32f429zi  
- stm32f746zg  
- stm32l476rg  
您可以自行编译固件，或者从本仓库的firmware目录下直接获取即可，假若您有自己的芯片平台需要支持拖拽烧录，可以参考目前的代码作修改。

### Q: 在linux下可以使用DAP仿真器进行调试吗？
 linux下可以使用openocd配合DAP仿真器进行调试，openocd是目前全世界最流行，最强大的开源调试器上位机，由于openocd是跨平台的，你也可以在windows下使用openocd，通过编写适当的配置脚本，可以实现对芯片的调试、烧录等操作。由于涉及内容较多，更多说明请读者自行搜索，或者留言咨询。  


有任何问题或者建议，请在本仓库的[Issues](https://github.com/wuxx/nanoDAP/issues)页面中提出，我们会持续跟进解决。