# Alienware-17r3-Hackintosh

With the help of CorpNewt and the talent over at **[/r/ Hackintosh Paradise](https://discord.gg/vwrZYVej7g)**  I was able to successfully create a stable working vanilla Hackintosh.  I will be going over my process and what is needed to accomplish getting macOS booted on your Alienware 17R3. I will not be providing any links to my EFI folder as this will take away from your learning experience.  Having a Hackintosh requires knowledge of what you are doing in order to maintain a stable system and be able to update successfully.

![screenshot](Screen Shot 2021-01-03 at 12.45.11 AM.png)

# Specs

- **CPU:** Intel Core i7-6700HQ
- **RAM:** 32 GB DDR4 2133 Mhz
- **IGPU:** Intel HD 530
- **dGPU:** NVIDIA GTX 970m
- **Wifi:** Card: Dell Wireless 1560 aka DW1560
- **OS:** macOS Big Sur, Windows 10 Pro, Ubuntu Budgie
- **Storage:** 2 Samsung 970 EVO Plus NVME SSD 1TB,1 Samsung 860 EVO 1TB

# Bios Configuration

- **SATA OPERATION** ACHI
- **USB WAKE SUPPORT** Disabled
- **FIRMWARE TPM** Disabled
- **SECURE BOOT** Disabled

# Kexts

- **AirportBcrmFixup:** Since I replaced the original Killer Wireless Wifi card that originally comes with the laptop.  I am using now a DW1560. AirportBcmFixup is an open source kernel extension providing a set of patches required for non-native Airport Broadcom Wi-Fi cards. 

- **AppleALC:** Is an open source kernel extension enabling native macOS HD audio for not officially supported codecs without any filesystem modifications. With Creative codec my layout id is **1** that doesnt mean that it will be the same for you. Layout 0, 1, 2, 3, 4, 5, 6, 9, 10, 11, 12 are all the codecs that you can choose from.

- **AtherosE2200Ethernet:** Is a Qualcomm Atheros Killer E2200 driver for macOS.  Simply enables your ethernet adapter to work.

- **BrcmPatchRAM:** Is a macOS driver which applies PatchRAM updates for Broadcom RAMUSB based devices. It will apply the firmware update to your Broadcom Bluetooth device on every startup / wakeup, identical to the Windows drivers. The firmware applied is extracted from the Windows drivers and the functionality should be equal to Windows.  With macOS 10.5 or higer you will want to use **BrcmPatchRAM3.kext**. You will also want to use **BrcmFirmwareData.kext** and **BrcmBluetoothInjector.kext**. These will all be in the links below. This will get bluetooth working along with continuty and handoff.

- **CpuFriend:**  Is a Lilu plug-in for dynamic power management data injection.  In the links below you will use a script to set your power management to your liking.  This will create a kext called **CPUFriendDataProvider.kext**.

- **Lilu:** An open source kernel extension bringing a platform for arbitrary kext, library, and program patching throughout the system for macOS. As of right now in macOS Big Sur, Lilu is only semi-working as there is an issue with userspace patching. 

- **VoodooPS2Controller:** This will allow your keyboard and trackpad to operate.  Gestures will now work with the older version of this kext that I use.  Acidanthera has an updated one but my trackpad clickers do not work with that one.

- **WhateverGreen:** Is a Lilu plugin providing patches to select GPUs on macOS. Requires Lilu 1.4.0 or newer.

- **VirtualSMC** Is an advanced Apple SMC emulator in the kernel. Requires Lilu for full functioning. The following plugin kexts that come with VirtualSMC are the ones I use.  **SMCBatteryManager.kext:** battery percentage, **SMCDellSensors.kext:** sensors, and **SMCProcessor.kext:** for finer measurement of the CPU.

# Drivers

- **HFSPlus.efi:** Is needed for seeing HFS volumes(ie. macOS Installers and Recovery partitions/images).

- **OpenRuntime.efi:** Is a replacement for AptioMemoryFix.efi (opens new window), used as an extension for OpenCore to help with patching boot.efi for NVRAM fixes and better memory management.

- **AudioDXE.efi:** For Bootchime

- **OpenCanopy.efi** This is OpenCore's optional GUI. 

# SSDT'S

- **SSDT-USBX & SSDT-UIAC:** The SSDT's represent usb power and mapping of your ports.  They are created while using CorpNewt's UsbMapping tool which will be linked down below.  In combination with this these SSDT"S you will also use a kext that is created as well.

- **SSDT-PNLF:**. This is to be used in combination with WhateverGreen to enable brightness control in macOS.

- **SSDT-NoHybGfx** This bad boy disables your Nvidia Gpu in macOS so you can get proper sleep working. Our Nvidia Gpu is pointless being enabled as it drains more battery and cannot function on anything higher than High SIerra.    It can work in High Sierra but due to Nvidia Optimus we cannot display from our laptop screen only via the HDMI port.  Do note but using the SSDT you will also sacrifice using our tb3 port for display out via igpu.  This is a double edged sword as you are either willing to give up sleep or video out.  I prefer to have working sleep.

- **SSDT-HPET:** Fixes IRQ Conflicts.  We will be utilizing one of CorpNewt's tools in the link below to create this SSDT which is called SSDTTIME.

- **SSDT-PLUG:** Allows for native CPU power management on Haswell and newer CPU's.

- **SSDT-GPRW:** This is a special SSDT for solving instant wake by hooking GPRW or UPRW. 

# ACPI Patches

- **Change EC0 to EC:** Find ``<4543305F>`` Replace ``<45435F5F>`` This renames ECO to EC which is your embedded controller.

- **Change_PRW to XPRW:** Find ``<140F5F50525700A4475052570A6D0A04>`` Replace ``<140F5850525700A4475052570A6D0A04>`` This is a custom patch that prevents laptop from waking on sleep.

- **Change_CRS to XCRS:** Find ``<5F43525308A019>`` Replace ``<5843525308A019>`` Created with SSDTTIME FixHPET

- **RTC IRQ 8 Patch:** Find ``<2200017900>`` Replace ``<2200007900>`` Created with SSDTTIME FixHPET

- **TIMR IRQ 0 Patch:** Find ``<2201007900>`` Replace ``<2200007900>`` Created with SSDTIME FixHPET

# My Boot Args

- ```keepsyms=1``` This is a companion setting to debug=0x100 that tells the OS to also print the symbols on a kernel panic. That can give some more helpful insight as to what's causing the panic itself.

- ```cdfon``` This enable HDMI 2.0 patches 

- ```alcid=1```  This makes our layout-id = 1 for audio 

- ```alcdelay=1000``` Sometimes race conditions can occur where your hardware isn't initialized in time for AppleHDAController resulting in no sound output. To get around this, you can either:

- ```debug=0x100``` This disables macOS's watchdog which helps prevents a reboot on a kernel panic. That way you can hopefully glean some useful info and follow the breadcrumbs to get past the issues.

# Links

- [OpenCore:](https://github.com/acidanthera/OpenCorePkg) This is the bootloader which we will be using to boot macOS.  Be sure to review the docs as they contain valuable information.  

- [Config.Plist Setup:](https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/skylake.html#starting-point) This is the one and only guide I recommend. It is thorough and will go over all the basic format of the config.plist and everything you will need. 

- [ProperTree:](https://github.com/corpnewt/ProperTree) ProperTree is a cross-platform GUI plist editor written using Python (compatible with both 2.x and 3.x) and Tkinter.  This is your all in one plist editor which even has themes now.  You can also utilize the OCSnapshot function in ProperTree which will import all your SSDt's, Drivers, and Kexts into you config.plist which I must say is super convenient! 

- [MaciASL](https://github.com/acidanthera/MaciASL) A native AML compiler and IDE for macOS, with syntax coloring, tree navigation, automated patching, online patch file repositories, and iASL binary updates. Written entirely in Cocoa, conforms to macOS guidelines.

- [Kext Repo:](https://onedrive.live.com/?authkey=%21APjCyRpzoAKp4xs&id=FE4038DA929BFB23%21455036&cid=FE4038DA929BFB23) This is the OneDrive of all the dev builds of the kexts that we use. 

- [gfxutil](https://github.com/acidanthera/gfxutil) A tool to work with Device Properties commonly found in Apple Mac firmwares by mcmatrix.

- [USBMap Tool:]( https://github.com/corpnewt/USBMap) Python script for mapping USB ports in macOS and creating a custom injector kext. 

- [SSDTTIME:](https://github.com/corpnewt/SSDTTime) A simple tool designed to make creating SSDTs simple. Supports macOS, Linux and Windows.  This is where we will create our **SSDT-HPET & SSDT-PLUG**. It will also generate custom patches for your IRQ Conflicts which can be applied in the patches section of ACPI on your config.plist. 

- [CPUFriend:]( https://github.com/corpnewt/CPUFriendFriend) This Py script will inspect the frequency vectors of the X86PlatformPlugin plist matching your SMBIOS configuration and leverage acidanthera's CPUFriend ResourceConverter to help you optimize your power management configuration.  

- [MountEFI:]( https://github.com/corpnewt/MountEFI) It's a script that mounts your EFI simply. 

- [OCConfigCompare:](https://github.com/corpnewt/OCConfigCompare )Is a python script to compare two plists and list missing keys in either.  This is very useful when you are trying to figure out whats missing or what errors OC is giving you prior to boot. 

- [OpenCore Sanity Check:]( https://opencore.slowgeek.com) This is an alternative method from Corp's OCConfigCompare Script to drag and drop your plist in and see what is missing and what the recommendations are. 

- [rEFInd:](https://www.rodsbooks.com/refind/) This is a bootloader for your laptop to multiboot multiple operating systems.  I highly recommend this as your booting option as Opencore will trick windows and linux into thinking that it is a real macbook.  This can cause issues as it is not. 

# Unresolved Issues

- **Shutdown Issue:** While in macOS if I power off with the power adapter not connected the laptop will not turn back on without putting the adapter back in.  I believe the laptop is going into some werid sleep state or what not.  If anyone else has experienced this please reach out to me.  I would love one day to find a solution for this.

- **Mic Issues:** If I reboot the laptop when I enter macOS the mic will not work.  If I put the laptop to sleep and wake it up the mic works again.

- **Sleep Issue** If I reboot from Windows or Linux to macOS and try to put the laptop to sleep, it will go to sleep but will kernel panic on wake.  The same applys if I do the opposite from rebooting from macOS into Windows or Linux.  The sleep issue will presist in those OS's as well.  The only fix is another reboot to that same operating system.



























