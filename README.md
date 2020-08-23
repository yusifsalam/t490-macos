# Lenovo T490 macOS with OpenCore

### Status: WIP

This repo contains information for getting macOS 10.15 Catalina working on a Lenovo T490 laptop.

The compatibility is good for the most part, most of the stuff works like it would on a real macbook, including camera, audio, trackpad, iCloud services. The experience is pleasant, as the laptop is smooth and responsive under macOS Catalina. Battery life isn't great (from personal experience Arch Linux is better and Windows 10 the best of the three), but that can probably be fixed with undervolting. The Intel WiFi card is soldered onto the motherboard, which means it can't be replaced with a Broadcom one, but the Intel card is now functional albeit not operating at full speeds - I am getting 50/10 mbit up/down on a 200/20 connection, which is fine for most use cases.

Currently running:

| Component      | Version |
| -------------- | ------- |
| macOS Catalina | 10.15.6 (19G2021) |
| OpenCore       | 0.6.0             |
| BIOS version   | 0.1.66            |
| EC version     | 0.1.19            |

## Hardware info

| Component | Model                                   |
| --------- | --------------------------------------- |
| CPU       | Intel i5-8265U Whiskey Lake             |
| Memory    | 16GB 2400Mhz                            |
| Storage   | WDC PC SN720 512GB                      |
| Display   | 14" non-touch 1920x1080                 |
| GPU       | Intel UHD 620                           |
| Camera    | 720p with Windows Hello IR sensor       |
| WLAN      | Intel Wireless-AC 9560 2x2ac with BT5.0 |
| Battery   | Single 3-cell 50Wh                      |
| Touchpad  | Synaptics TM3471-010                    |

Note on NVME storage: Samsung PM981 drives will not work out of the box if at all, consider replacing the drive if you have one ([issue](https://github.com/yusifsalam/t490-macos/issues/3))

## Status

### Working

- [x] Keyboard. Volume control natively, brightness control with software
- [x] Battery indicator
- [x] Audio
- [x] Ethernet
- [x] iCloud services - iMessage, FaceTime, AppStore
- [x] GPU acceleration
- [x] Camera
- [x] Microphone
- [x] Bluetooth
- [x] Mac-like booting interface for multiboot
- [x] Sleep/wake
- [x] Trackpad and gestures
- [x] Native CPU power management
- [x] HDMI video and audio 1080p, 1440p

### Working, sort of

- [ ] Wifi works but not at full speeds
- [ ] Audio jack - glitches after wake from sleep. I don't use it so won't fix but will merge pull requests

### Not working at the moment

- [ ] SD card reader - I don't use it so won't fix but will merge pull requests
- [ ] AirDrop, Continuity - wifi card can't be replaced, so this is unlikely to ever work

### Not tested

- [ ] USB type C video
- [ ] HDMI video at 4K
- [ ] Thunderbolt
- [ ] Sidecar
- [ ] FileVault
- [ ] DRM content playback - I don't have Netflix or AppleTV+

## Kexts

| Kext                   | Version | Remark                                   |
| ---------------------- | ------- | ---------------------------------------- |
| AppleALC               | 1.5.0   | Fixes onboard audio                      |
| CPUFriend              | 1.2.2   | Power management                         |
| CPUFriendDataProvider  | -       | Frequency vector for CPUFriend           |
| IntelBluetoothFirmware | 1.1.2   | Fixes bluetooth                          |
| IntelBluetoothInjector | 1.1.2   | Companion for IntelBluetoothFirmware     |
| IntelMausiEthernet     | 2.5.1d1 | Fixes ethernet                           |
| itlwm                  | 1.0.1   | Wifi fix, not for everyday use           |
| Lilu                   | 1.4.7   | Kext patcher                             |
| NoTouchID              | 1.0.4   | Disable TouchID                          |
| NVMEFix                | 1.0.4   | Fix for NVME SSDs                        |
| SMCBatteryManager      | 1.0     | Battery indicator                        |
| SMCLightSensor         | 1.0     | Ambient light sensor                     |
| SMCProcessor           | 1.1.6   | CPU temp monitoring                      |
| SMCSuperIO             | 1.1.6   | Monitor fan speed, not working           |
| USBInjectAll           | 0.7.5   | Inject all USB, only for troubleshooting |
| USBMap                 | -       | Inject only mapped USB                   |
| VirtualSMC             | 1.1.6   | SMC chip emulation                       |
| VoodooPS2Controller    | 2.1.6   | Enable keyboard, mouse, trackpad         |
| WhateverGreen          | 1.4.2   | Graphics                                 |

## ACPI patches

| Patch                  | Remark                         |
| ---------------------- | ------------------------------ |
| SSDT-AWAC              | Fix AWAC                       |
| SSDT-BAT               | Fix battery indicator          |
| SSDT-EXT1-FixShutdown  | Fix shutdown on reboot         |
| SSDT-EXT3-LedReset-TP  | Fix LED not working after wake |
| SSDT-EXT4-WakeScreen   | Fix screen not waking          |
| SSDT-GPIO              | Trackpad fix                   |
| SSDT-GPRW              | Fix immediate wake after sleep |
| SSDT-HPET              | Fix irq conflicts              |
| SSDT-PLUG              | x86 plugin injection fix       |
| SSDT-PNLF-CFL          | Backlight fix                  |
| SSDT-PTSWAK            | Fix sleep issues               |
| SSDT-SBUS-MCHC         | SBUS fix                       |
| SSDT-USBX              | USBX patch                     |

## Pre-Install & BIOS settings
First, read the [Dortania OC guide](https://dortania.github.io/OpenCore-Install-Guide/). The guide will take you through the creation of installation USB and drive formatting. Update to the latest firmware (the easiest way is to use fwupd on linux). 

Then do the following BIOS settings:
- Disable secure chip
- Enable Intel Virtualization and VT-d
- Disable secure boot and fast boot
- Disable Intel SGX control
- Disable Device Guard
- Disable wake on LAN/thunderbolt
- Set boot mode to UEFI only, disable CSM support

Now you can install macOS on your APFS or HFS+ formatted drive. 

## Post-install
- [Fix iServices](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html#generate-a-new-serial) if you want to use iMessage or FaceTime.
- Install [HeliPort](https://github.com/zxystd/HeliPort) to control the wifi kext from within macOS
- Put your wifi ssid and password inside itlwm kext's Info.plist to automatically connect to wifi without using HeliPort
- Disable hibernation, since it doesn't work properly on hackintoshes
- Make your own USB map kext
- Generate your own CPU frequency vectors using [CPUFriendFriend](https://github.com/corpnewt/CPUFriendFriend). The one included here is set to Balance power and CPU lowest frequency set to 500 MHz 
- Install [Karabiner-Elements](https://github.com/pqrs-org/Karabiner-Elements) for brightness keys (and other keyboard rebindings). I recommend applying changes only to the keyboard, since I've had some conflicts with my Logitech mouse. 
- (Optional) [Rectangle](https://github.com/rxhanson/Rectangle) for window management
- (Optional) [LuLu](https://github.com/objective-see/LuLu) for network traffic control

## CREDITS

- [Acidanthera](https://github.com/acidanthera)
- [Dortania OC guide](https://dortania.github.io/OpenCore-Install-Guide/)
- [Rehabman's battery patch guide](https://www.tonymacx86.com/threads/guide-how-to-patch-dsdt-for-working-battery-status.116102/) and [Rehabman's ACPI hotpatching guide](https://www.tonymacx86.com/threads/guide-using-clover-to-hotpatch-acpi.200137/)
- [CorpNewt's tools](https://github.com/corpnewt)
- [Daliansky's OC-little repo](https://github.com/daliansky/OC-little)
- [Tyler Nguyen's x1c-hackintosh repo](https://github.com/tylernguyen/x1c6-hackintosh)
- [Vojtěch Jungmann's T480-OpenCore-Hackintosh repo](https://github.com/EETagent/T480-OpenCore-Hackintosh)
