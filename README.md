# Lenovo s340 Big Sur OpenCore 

![alt tag](https://i.ibb.co/RzW810W/Lenovo.png "Lenovo s340")​

## SCEENSHOTS:
![alt tag](https://i.ibb.co/Sf6wDQ4/2020-09-06-20-26-03.png "Lenovo s340")​
![alt tag](https://i.ibb.co/SwCYHfc/2020-09-06-20-25-12.png "Lenovo s340")​
![alt tag](https://i.ibb.co/Gn9V3Bv/2020-09-06-15-43-19.png "Lenovo s340")​
![alt tag](https://i.ibb.co/yV2Bm52/2020-09-06-15-31-36.png "Lenovo s340")​



## SYSTEM

|||
|----------------|------------------------------------------------------------|
|Processor:| Intel Core  i5-8265U (Whiskey Lake) |
|Memory:          |4GB Soldered + 4gb  |         
|Graphics:         |Integrated Intel UHD Graphics 620|
|Wireless Network:          |Broadcom BCM94322|
|Audio:        |Realtek ALC257 |
|Camera:          |Inbuilt|

## What's working:
  - USB Ports, USB-C :white_check_mark:
  - Keyboard :white_check_mark:
  - Trackpad, gestures. :white_check_mark:
  - Audio, Headphones :white_check_mark:
  - Internal graphics with fill acceleration. :white_check_mark:
  - Brightness control. :white_check_mark:
  - Sleep :white_check_mark:
  - Sleep and Wake with Lid Open and Close :white_check_mark:
  - Inbuilt Camera :white_check_mark:

## What's Not Working:
  - Card reader. :x:
  - HDMI :bangbang:




# Installation:

## Keyboard:
Keyboard should already be working at this point using VoodooPS2Controller.

## Brightness and Volume shortcuts:
Volume already works. For brightness fix, use the SSDT-Q1112 patch.
Also apply the _Q11->XQ11 and _Q12->XQ12 rename patch via OpenCore config.

## Trackpad:
Install latest VoodooI2C.kext and VoodooI2CHID.kext
Add SSDT-GPI0 and SSDT-XOSI hotpatches to OS/ACPI 
Make sure change _OSI to XOSI patch is enabled in your config.plist.

## Internal Graphics:
Should already be working with the config.plist. If you have slightly different configuration; use hackintool to generate your own patch.

## Backlight:
Add SSDT-PNLFCFL.aml to OS/ACPI folder
Displays under System Preferences should now have a brightness slider.

## Battery Status:
Install latest SMCBatteryManager.kext to OS/kext

## Processor features:
Install SMCProcessor.kext to OS/kext

## Audio:
To enable Intel HD Audio add AppleALC.kext.
alcid = 11

## Inbuilt Camera:
Works. No extra files needed.

## CPU power management:
Works. No extra files needed.

# -Fix no HDMI output problem according to following Guide-

REF General Framebuffer Patching Guide using Hackintool
Intel Framebuffer patching using WhateverGreen

**1. Use PlistEdit pro edit config.plist under DeviceProperties/Add PciRoot(0)/Pci(0x2,0x0) change to PciRoot(0x0)/Pci(0x2,0x0), use alldata method, as below illustrated Modify the busId and Type to make HDMI and type-c to DP output work.**

>ID: 3EA50009, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00830B0A
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
GPU Name: Intel Iris Plus Graphics 655
Model Name(s):
Camelia: V3
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - LVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000001C7 - DP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000001C7 - DP

**Original: -------------------------------------->Modified:**
>00000800 02000000 98000000---------------->00000800 02000000 98000000
01**05**0900 00**04**0000 C7010000---------------->01**01**0900 00**08**0000 C7010000
02**04**0A00 00**04**0000 C7010000---------------->02**06**0A00 00**04**0000 C7010000

**or in frambuffer-con0-alldata**
>00000800 02000000 98000000 01**05**0900 00**04**0000 C7010000 02**04**0A00 00**04**0000 C7010000 ---->
00000800 02000000 98000000 01**01**0900 00**08**0000 C7010000 02**06**0A00 00**04**0000 C7010000

![alt tag](https://i.ibb.co/kDXqQpj/3.png "Lenovo s340")​
![alt tag](https://i.ibb.co/HqZzRrc/4.png "Lenovo s340")​

**2. HDMI port, Modification:** 
>framebuffer-con1-alldata DATA 01050900 00040000 C7010000--->01010900 00080000 C7010000
In Index1(con1) busId 0x05 --> 0x01 . type 0x00000400 (DP) ---> 0x00000800 (HDMI)

**3. USB Type-C to DP, Modification:**
>framebuffer-con2-alldata DATA 02040A00 00040000 C7010000--->02060A00 00040000 C7010000
In index2(con2 ) busId 0x04 --> 0x06 type 0x00000400 (DP) ---> 0x00000400 (DP) or [0x00000800 (HDMI) if you use USB Type-C to HDMI]
