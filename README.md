# Lenovo s340 Big Sur OpenCore 

## SYSTEM

|||
|----------------|------------------------------------------------------------|
|Processor:| Intel Core  i5-8265U (Whiskey Lake) |
|Memory:          |4GB Soldered + 4gb  |         
|Graphics:         |Integrated Intel UHD Graphics 620|
|Wireless Network:          |Broadcom BCM94322|
|Audio:        |Realtek ALC257 |
|Camera:          |Inbuilt|

What's working:
Keyboard
Trackpad, gestures.
Audio, Headphones
Internal graphics with fill acceleration.
Brightness control.
Sleep
Sleep and Wake with Lid Open and Close
Inbuilt Camera

What's Not Working:
Card reader.
HDMI


Installation
BIOS:
Ensure your disk is in AHCI mode. Here are the settings that I used.


Keyboard:
Keyboard should already be working at this point using VoodooPS2Controller.

Brightness and Volume shortcuts:
Volume already works. For brightness fix, use the SSDT-Q1112 patch.
Also apply the _Q11->XQ11 and _Q12->XQ12 rename patch via clover config.

Trackpad:
Install latest VoodooI2C.kext and VoodooI2CHID.kext
Add SSDT-GPI0 and SSDT-XOSI hotpatches to Clover/ACPI/patched
Make sure change _OSI to XOSI patch is enabled in your config.plist.

Internal Graphics:
Should already be working with the config.plist. If you have slightly different configuration; use hackintool to generate your own patch.

Backlight:
Add SSDT-PNLFCFL.aml to clover/ACPI/patched folder
Displays under System Preferences should now have a brightness slider.


Battery Status:
Install latest SMCBatteryManager.kext to clover/kext

Processor features:
Install SMCProcessor.kext to clover/kext

Audio
To enable Intel HD Audio add AppleALC.kext.
alcid = 11

Inbuilt Camera
Works. No extra files needed.

CPU power management
Works. No extra files needed.
