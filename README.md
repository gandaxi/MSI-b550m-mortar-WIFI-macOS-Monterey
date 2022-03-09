更新：
驱动版本更新

新增：
锐龙电源管理及监控驱动（AMDRyzenCPUPowerManagement.kext、SMCAMDProcessor.kext）https://github.com/trulyspinach/SMCAMDProcessor

NVME兼容驱动（NVMeFix.kext）https://github.com/acidanthera/NVMeFix

2022.03.09
==========================


微星(MSI)MAG B550M 迫击炮 WIFI

**OpenCore : 0.7.8**

**macOS ：12.1 — 12.2.1 正式版**

**SMBIOS : MacPro6,1**

### Specification

| **Component**       | **Model**                  |
| ------------------- | -------------------------- |
| CPU                 | AMD R5 3600                |
| Motherboard         | MSI(MAG) B550M MORTAR WIFI |
| Audio Chipset       | ALCS1200A                  |
| GPU                 | AMD R9 FURY X 4G           |
| Ethernet            | RTL8125B 2.5GbE            |
| WiFi & Bluetooth    | Intel WiFi 6 AX200         |

### What works

- Audio

  [AppleALC](https://github.com/acidanthera/AppleALC) ( `alcid=7 `)

- Ethernet
  [LucyRTL8125Ethernet](https://github.com/Mieze/LucyRTL8125Ethernet)

- USB

- Wi-Fi
  [itlwm](https://github.com/OpenIntelWireless/itlwm)
  AirportItlwm.kext set the `MaxKernel` field to `20.99.9` (BigSur)
  AirportItlwm12.kext set the `MinKernel` field to `21.00.0` (Monterey)

- Bluetooth
  [IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware)
  IntelBluetoothInjector.kext set the `MaxKernel` field to `20.99.9` (BigSur)
  BlueToolFixup.kext set the `MinKernel` field to `21.00.0` (Monterey)

### NEW AMD Kernel Patches

1. Enable `ProvideCurrentCpuInfo`
   `Kernel -> Quirks -> ProvideCurrentCpuInfo`

2. Edit the core count patch to match your CPU

   [AMD Vanilla OpenCore o](https://github.com/AMD-OSX/AMD_Vanilla/tree/master)r [OpenCore-Install-Guide](https://dortania.github.io/OpenCore-Install-Guide/extras/monterey.html#amd-patches)

   > Find the three `algrey - Force cpuid_cores_per_package`
   >
   > - `kernel -> Patch -> 0  -> Replace` for macOS 10.13,10.14
   > - `kernel -> Patch -> 1  -> Replace` for macOS 10.15,11.0
   > - `kernel -> Patch -> 2  -> Replace` for macOS 12.0
   >
   > ```
   > B8000000 0000 => B8 <core count> 0000 0000
   > BA000000 0000 => BA <core count> 0000 0000
   > BA000000 0090 => BA <core count> 0000 0090
   > ```
   >
   > | CoreCount | Hexadecimal |
   > | --------- | ----------- |
   > | 6 Core    | 06          |
   > | 8 Core    | 08          | 
   > | 12 Core   | 0C          |
   > | 16 Core   | 10          |
   > | 32 Core   | 20          |
   > | 64 Core   | 40          |
   >
   > for eamlple : 3600 6 Core
   > ```
   > B8 06 0000 0000
   > BA 06 0000 0000
   > BA 06 0000 0090
   > ```
   >
please use [OpenCore Configurator](https://mackie100projects.altervista.org/opencore-configurator/) or  [OC Auxiliary](https://github.com/ic005k/QtOpenCoreConfig)  or  [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)  to generate yourself SMBIOS

### Monterey
Most MSI B550 motherboard need use the 7C94v12 version of the bios to start Monterey. Please test yourself according to your hardware.
建议将主板bios更新至2020年8月份 7C94V12 老版本。
==== 
## bios设置：
----  
#禁用（恢复默认设置不用调整）
  
  4G以上解码
（这个若必须开启，则NVRAM中boot-args删除npci=0x2000。不要同时开启这个参数和npci。）【If you are on a Gigabyte/Aorus or an AsRock motherboard, enabling this option may break certain drivers(ie. Ethernet) and/or boot failures on other OSes, if it does happen then disable this option and opt for npci instead
2020+ BIOS Notes: When enabling Above4G, Resizable BAR Support may become an available on some X570 and newer motherboards. Please ensure that Booter -> Quirks -> ResizeAppleGpuBars is set to 0 if this is enabled.】
  
  快速启动
  
  安全启动
  
  串行/COM 端口
  
  并口
  
  兼容性支持模块 (CSM)
（必须关闭，gIO启用此选项时，GPU 错误很常见）
3990X 用户特别注意：macOS 目前不支持内核中超过 64 个线程，如果看到更多，内核会恐慌。3990X CPU 总共有 128 个线程，因此需要禁用一半。对于这些情况，我们建议在 BIOS 中禁用超线程。
----
#开启（恢复默认设置不用调整）
  
EHCI/XHCI 切换
  
操作系统类型：Windows 8.1/10 UEFI 模式
  
SATA模式：AHCI

======================
