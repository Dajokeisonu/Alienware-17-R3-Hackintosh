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


