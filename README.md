# Lenovo T490 macOS with OpenCore

### Status: WIP

This repo contains information for getting macOS 10.15 Catalina working on a Lenovo T490 laptop.

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
| Display   | 14" Non-touch 1920x1080                 |
| GPU       | Intel UHD 620                           |
| Camera    | 720p with Windows Hello IR sensor       |
| WLAN      | Intel Wireless-AC 9560 2x2ac with BT5.0 |
| Battery   | Single 50Wh                             |

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
- [x] Mac-like booting interface for multiboot.

### Working, sort of

- [ ] Wifi works but requires a kext with hardcoded ssid and password, upload speed is non-existent
- [ ] Trackpad and gestures
- [ ] CPU power management
- [ ] Sleep/wake

### Not working at the moment

- [ ] HDMI
- [ ] SD card reader

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
