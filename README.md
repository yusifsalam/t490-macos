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

