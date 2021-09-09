# Lenovo T490 macOS with OpenCore

### Status: WIP

#### Update: This project will no longer be actively maintained, since I recently got an M1 MacBook. I may still update OpenCore and the kexts every once in a while but no promises about that.

This repo contains information for getting macOS 11 Big Sur working on a Lenovo T490 laptop.

The compatibility is very good for the most part, and it behaves as a real MacBook Pro, including camera, audio, trackpad, iCloud services. In general, the experience is pleasant, as the laptop is smooth and responsive under macOS. Battery life is acceptable (around 6h with the brightness set to half). The Intel WiFi card is soldered onto the motherboard, which means it can't be replaced with a Broadcom one, but the Intel card is now functional - I am getting 150/20 mbit up/down on a 200/20 connection. With the latest AirportItlwm kext even Handoff and continuity features are working, except for AirDrop.

Currently running:

| Component     | Version |
| ------------- | ------- |
| macOS version | 11.5.2    |
| OpenCore      | 0.7.3   |
| BIOS version  | 1.73    |
| EC version    | 1.23    |

## Hardware info

| Component | Model                                   |
| --------- | --------------------------------------- |
| CPU       | Intel i5-8265U/i7-8565U Whiskey Lake    |
| Memory    | 16GB/32GB 2400Mhz                       |
| Storage   | WDC PC SN720 512GB                      |
| Display   | 14" non-touch 1920x1080                 |
| GPU       | Intel UHD 620                           |
| Camera    | 720p with Windows Hello IR sensor       |
| WLAN      | Intel Wireless-AC 9560 2x2ac with BT5.0 |
| Battery   | Single 3-cell 50Wh                      |
| Touchpad  | Synaptics TM3471-010                    |

Note on NVME storage: Samsung PM981 drives will not work out of the box if at all, consider replacing the drive if you have one.

## Status

### Working

- [x] Keyboard (including all media keys)
- [x] Battery indicator
- [x] Display auto brightness
- [x] Audio
- [x] Ethernet
- [x] iCloud services - iMessage, FaceTime, iCloud Drive
- [x] GPU acceleration
- [x] Camera
- [x] Microphone
- [x] Bluetooth
- [x] Mac-like booting interface for multiboot
- [x] Sleep/wake
- [x] Trackpad and gestures
- [x] Native CPU power management
- [x] HDMI video and audio up to 1440p
- [x] Handoff, continuity
- [x] AirPlay
- [x] FileVault
- [x] DRM content playback (Netflix, Apple TV+)
- [x] Thunderbolt - works with Lenovo Thinkpad Thunderbolt 3 Dock (tested Ethernet, display over DisplayPort and HDMI, USB ports)

### Working, sort of

- [ ] Wifi works but not at full speeds
- [ ] Audio jack - glitches after wake from sleep. I don't use it so won't fix but will merge pull requests
- [ ] USB-C video output works, but no audio

### Not working at the moment

- [ ] HDMI video at 4K
- [ ] SD card reader - I don't use it so won't fix but will merge pull requests
- [ ] AirDrop

### Not tested

- [ ] Sidecar

## Kexts

| Kext                   | Version     | Remark                                   |
| ---------------------- | ----------- | ---------------------------------------- |
| AirportItlwm           | 2.0.0 alpha | WiFi kext                                |
| AppleALC               | 1.6.4       | Fixes onboard audio                      |
| BrightnessKeys         | 1.0.2       | Fix brightness keys                      |
| CPUFriend              | 1.2.4       | Power management                         |
| CPUFriendDataProvider  | -           | Frequency vector for CPUFriend           |
| IntelBluetoothFirmware | 2.0.0       | Fixes bluetooth                          |
| IntelBluetoothInjector | 2.0.0       | Companion for IntelBluetoothFirmware     |
| IntelMausiEthernet     | 2.5.1d1     | Fixes ethernet                           |
| Lilu                   | 1.5.6       | Kext patcher                             |
| NoTouchID              | 1.0.4       | Disable TouchID                          |
| NVMEFix                | 1.0.9       | Fix for NVME SSDs                        |
| SMCBatteryManager      | 1.2.7       | Battery indicator                        |
| SMCLightSensor         | 1.2.7       | Ambient light sensor                     |
| SMCProcessor           | 1.2.7       | CPU temp monitoring                      |
| SMCSuperIO             | 1.2.7       | Monitor fan speed                        |
| USBInjectAll           | 0.7.5       | Inject all USB, only for troubleshooting |
| USBMap                 | -           | Inject only mapped USB                   |
| VirtualSMC             | 1.2.7       | SMC chip emulation                       |
| VoodooRMI              | 1.3.3       | Trackpad driver                          |
| VoodooSMBUS            | 3.0 dev     | SMBUS driver                             |
| VoodooPS2Controller    | 2.2.5       | Enable keyboard                          |
| WhateverGreen          | 1.5.3       | Graphics                                 |
| YogaSMC                | 1.5.1       | ACPI driver                              |

## ACPI patches

| Patch                 | Remark                         |
| --------------------- | ------------------------------ |
| SSDT-ALS0             | Fix display auto brightness    |
| SSDT-AWAC             | Fix AWAC                       |
| SSDT-BAT              | Fix battery indicator          |
| SSDT-EXT1-FixShutdown | Fix shutdown on reboot         |
| SSDT-EXT3-LedReset-TP | Fix LED not working after wake |
| SSDT-EXT4-WakeScreen  | Fix screen not waking          |
| SSDT-GPRW             | Fix immediate wake after sleep |
| SSDT-HPET             | Fix irq conflicts              |
| SSDT-PLUG             | x86 plugin injection fix       |
| SSDT-PNLF-CFL         | Backlight fix                  |
| SSDT-PTSWAK           | Fix sleep issues               |
| SSDT-THINK            | YogaSMC helper                 |
| SSDT-USBX             | USBX patch                     |

## Pre-install: Creating the installation USB stick

First, read the [Dortania OC guide](https://dortania.github.io/OpenCore-Install-Guide/). The guide will take you through the creation of installation USB and drive formatting. Update your laptop to the latest BIOS firmware from your existing OS (the easiest way is to use fwupd on linux).

## Getting a valid Mac serial key

- [Fix iServices](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html#generate-a-new-serial) if you want to use iMessage, FaceTime, iCloud. You need a valid, unique Mac serial key (the config.plist in this repository does not have one as all Mac devices - including hackintosh - need a unique serial) to be able to use Apple's cloud services and authentication. If you don't, you won't be able to login with an Apple ID, thus no App Store either! To generate a serial and update directly the config.plist, you can use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS). Use SMBIOS _MacBookPro15,4_ as it is the closest MacBook to our internal hardware.

## Compatible BIOS settings

- Disable secure chip
- Enable Intel Virtualization and VT-d
- Disable secure boot and Fast boot
- Disable Intel SGX control
- Disable Device Guard
- Disable wake on LAN/Thunderbolt
- Set boot mode to UEFI only
- Disable CSM support

Now you can boot from your USB stick. If it fails to boot, try a different USB stick, double check your BIOS settings.

## Post install

- Disable hibernation, since it doesn't work properly on hackintoshes

```
sudo pmset autopoweroff 0
sudo pmset powernap 0
sudo pmset standby 0
sudo pmset proximitywake 0
sudo pmset tcpkeepalive 0
```

- [Make your own USB map kext](https://dortania.github.io/OpenCore-Post-Install/usb/)
- Generate your own CPU frequency vectors using [CPUFriendFriend](https://github.com/corpnewt/CPUFriendFriend). The one included here is set to Balance power and CPU lowest frequency set to 500 MHz
- Install [YogaSMCNC and YogaSMCPane](https://github.com/zhen-zen/YogaSMC) to control fans, battery, leds and more
- (Optional) [Rectangle](https://github.com/rxhanson/Rectangle) for window management similar to Windows (but better)
- (Optional) [LuLu](https://github.com/objective-see/LuLu) for network traffic control

For how to update OpenCore and kexts, read [this guide](https://dortania.github.io/OpenCore-Post-Install/universal/update.html#_5-boot). Personally, I use MountEFI to mount EFI partitions, ProperTree to edit plist files, and OCConfigCompare to compare my config files to the latest sample configs from OpenCore. I also always test drive my updated EFI with a USB stick before moving it to the EFI partition of the main SSD.

## CREDITS

- [Acidanthera](https://github.com/acidanthera)
- [Dortania OC guide](https://dortania.github.io/OpenCore-Install-Guide/)
- [Rehabman's battery patch guide](https://www.tonymacx86.com/threads/guide-how-to-patch-dsdt-for-working-battery-status.116102/) and [Rehabman's ACPI hotpatching guide](https://www.tonymacx86.com/threads/guide-using-clover-to-hotpatch-acpi.200137/)
- [CorpNewt's tools](https://github.com/corpnewt)
- [OpenWireless and itlwm](https://github.com/OpenIntelWireless/itlwm)
- [VoodooRMI](https://github.com/VoodooSMBus/VoodooRMI)
- [YogaSMC](https://github.com/zhen-zen/YogaSMC)
- [Daliansky's OC-little repo](https://github.com/daliansky/OC-little)
- [Tyler Nguyen's x1c-hackintosh repo](https://github.com/tylernguyen/x1c6-hackintosh)
- [Vojtěch Jungmann's T480-OpenCore-Hackintosh repo](https://github.com/EETagent/T480-OpenCore-Hackintosh)
