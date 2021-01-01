# Alienware-17r3-Hackintosh-Guide

With the help of the talent over at **/r/ Hackintosh Paradise** https://discord.gg/vwrZYVej7g  I was able to successfully create a stable working Hackintosh.  In this guide I will be going over my process and what is needed to accomplish getting macOS booted on your Alienware 17R3.

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
- **SECURE BOOT** Disbaled

# Kexts & Drivers

- **AirportBcrmFixup:** Since I replaced the original Killer Wireless Wifi card that originally comes with the laptop.  I am using now a DW1560. AirportBcmFixup is an open source kernel extension providing a set of patches required for non-native Airport Broadcom Wi-Fi cards. 

- **AppleAlc:** Is an open source kernel extension enabling native macOS HD audio for not officially supported codecs without any filesystem modifications. With Creative codec my layout id is **1** that doesnt mean that it will be the same for you. Layout 0, 1, 2, 3, 4, 5, 6, 9, 10, 11, 12 are all the codecs that you can choose from.

- **AtherosE2200Ethernet:** Is a Qualcomm Atheros Killer E2200 driver for macOS.  Simply enables your ethernet adapter to work.

- **BrcmPatchRAM:** Is a macOS driver which applies PatchRAM updates for Broadcom RAMUSB based devices. It will apply the firmware update to your Broadcom Bluetooth device on every startup / wakeup, identical to the Windows drivers. The firmware applied is extracted from the Windows drivers and the functionality should be equal to Windows.  With macOS 10.5 or higer you will want to use **BrcmPatchRAM3.kext**. You will also want to use **BrcmFirmwareData.kext** and **BrcmBluetoothInjector.kext**. These will all be in the links below. This will get bluetooth working along with continuty and handoff.

- **CpuFriend:**  Is a Lilu plug-in for dynamic power management data injection.  In the links below you will use a script to set your powermangment to your liking.  This will create a kext called **CPUFriendDataProvider.kext**.

- **Lilu:** An open source kernel extension bringing a platform for arbitrary kext, library, and program patching throughout the system for macOS. As of right now Lilu is only semi-working as there is an issue with userspace patching. 

- **VirtualSMC** Is an advanced Apple SMC emulator in the kernel. Requires Lilu for full functioning. The following plugin kexts that come with VirtualSMC are the ones I use.  **SMCBatteryManager.kext:** battery percentage, **SMCDellSensors.kext:** sensors, and **SMCProcessor.kext:** for finer measurement of the CPU.




