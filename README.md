# Lenovo T490 macOS with OpenCore

### Status: WIP

This repo contains information for getting macOS 10.15 Catalina working on a Lenovo T490 laptop.

The compatibility is good for the most part, most of the stuff works like it would on a real macbook, including camera, audio, trackpad, iCloud services. The experience is pleasant, as the laptop is smooth and responsive under macOS Catalina. Battery life isn't great (from personal experience Arch Linux is better and Windows 10 the best of the three), although it is very possible that battery life will improve after time, as spotlight indexing and photo library analysis complete their first run - I haven't been using this macOS install long enough to know. There are wake locks that I haven't identified that keep waking up the laptop during sleep. Network interfaces are limited to ethernet and tethered connection via phone hotspot, since the wifi driver is not stable enough as a daily driver and unfortunately the wifi card is soldered onto the motherboard.

Currently running:

| Component      | Version |
| -------------- | ------- |
| macOS Catalina | 10.15.4 |
| OpenCore       | 0.5.8   |

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
| Touchpad  | Synaptics                               |

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

### Working, sort of

- [ ] Wifi works but requires a kext with hardcoded ssid and password, upload speed is non-existent
- [ ] Trackpad and gestures
- [ ] CPU power management

### Not working at the moment

- [ ] HDMI
- [ ] SD card reader - won't fix, since I don't use it
- [ ] Audio jack - won't fix, since I don't use it

### Not tested

- [ ] USB type C video
- [ ] Thunderbolt
- [ ] Sidecar
- [ ] FileVault

## Kexts

| Kext                   | Version | Remark                                   |
| ---------------------- | ------- | ---------------------------------------- |
| AppleALC               | 1.4.9   | Fixes onboard audio                      |
| CPUFriend              | 1.2.0   | Power management                         |
| IntelBluetoothFirmware | 1.0.3   | Fixes bluetooth                          |
| IntelBluetoothInjector | 1.0.3   | Companion for IntelBluetoothFirmware     |
| IntelMausiEthernet     | 2.5.1d1 | Fixes ethernet                           |
| itlwm                  | -       | Wifi fix, not for everyday use           |
| Lilu                   | 1.4.4   |                                          |
| NoTouchID              | 1.0.3   | Disable TouchID                          |
| NVMEFix                | 1.0.2   | Fix for NVME SSDs                        |
| SMCBatteryManager      | 1.0     | Battery indicator                        |
| SMCLightSensor         | 1.0     | Ambient light sensor                     |
| SMCProcessor           | 1.0     | CPU temp monitoring                      |
| SMCSuperIO             | 1.1.4   | Monitor fan speed, not working           |
| USBInjectAll           | 0.7.5   | Inject all USB, only for troubleshooting |
| USBMap                 | -       | Inject only mapped USB                   |
| VirtualSMC             | 1.1.4   | SMC chip emulation                       |
| VooDooInput            | 1.0.5   | Trackpad driver, not used                |
| VooDooPS2Controller    | 2.1.4   | Enable keyboard, mouse, trackpad         |
| WhateverGreen          | 1.3.9   | Graphics                                 |

## ACPI patches

| Patch                  | Remark                                             |
| ---------------------- | -------------------------------------------------- |
| DSDT                   | Patched DSTD file, original has compilation errors |
| SSDT-AWAC              | Fix AWAC                                           |
| SSDT-EC                | Embedded Controller Rename, not used               |
| SSDT-GPIO              | Trackpad fix                                       |
| SSDT-PLUG              | Plugin fix, not working (?)                        |
| SSDT-PNLF-CFL          | Backlight fix                                      |
| SSDT-SBUS-MCHC         | SBUS fix                                           |
| SSDT-Thinkpad_Trackpad | Trackpad patch                                     |
| SSDT                   | Frequency vector for CPUFriend                     |
