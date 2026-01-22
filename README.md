# Dell-Latitude-7410-macOS-Sequoia-Hackintosh-Opencore
OpenCore based EFI for Dell Latitude 7410

![Dell-Latitude-7410-Hackintosh](https://github.com/user-attachments/assets/fde4e919-8011-48dd-b407-30ee93ee5f16)

I am currently using macOS Sequoia 15.7.2 and personally I wouldn't recommend updating to Tahoe because of the Liquid Glass issues and bugs.

## System Configuration

- CPU:  Intel Core i7-10610U vPRO
- iGPU: Intel UHD Graphics 620 (Comet Lake)
- RAM:  16GB DDR4 2667Mhz
- SSD:  512 GB KIOXIA KBG40ZNS512G NVMe 
- WiFi: Intel® Wi-Fi 6 AX201
- Display: 14" 1920*1080 FullHD IPS
- Sound Card: Realtek ALC295

### BIOS Version

1.30.0


### Bootloader

OpenCore 1.0.0

### SMBIOS

MacBookPro16,2

### SecureBootModel 
**macOS Ventura**: j215 or j223
**macOS Sequoia**: Disabled (due to wifi patching using OCLP)

## BIOS Settings

- Boot mode: UEFI
- Fast Boot: Thorough
- SecureBoot: Disable
- SATA Mode: AHCI 

## What's working

 
 - [x] CPU Speedstep

 - [x] iGPU acceleration

 - [x] Battery Management
 
 - [x] Sleep/Wake
 
 - [x] Internal Speakers (ALC Layout ID: 33)
 
 - [x] Internal Microphone 
 
 - [x] Headphone Jack - Working with ComboJack - [https://github.com/macos86/ComboJack](https://github.com/macos86/ComboJack) (not tested on macOS Sequoia)
 
 - [x] WiFi (2.4Ghz + 5Ghz)
 
 - [x] Bluetooth

 - [x] HDMI + audio over HDMI (I didn't test it)

 - [x] Touchpad with Gestures + Buttons

 - [x] Web Camera

 - [x] USB 3.0

 - [x] MicroSD card reader 

 - [x] Native hotkeys support with Fn keys (Volume Fn+F2/F3 and Screen Brightness Fn + F6/F7)
 
 - [x] USB-C charging

 - [x] USB-C DP-alt Mode
  
 - [x] USB-C Data transfer
 
 - [x] Thunderbolt (needs to plug a device before boot)
    
 - [x] iServices (iMessage & FaceTime)
 

## What's not working

- Everything is working great so far!

## What's not tested yet

- HDMI
- Thunderbolt

## Wifi Patching for macOS sequoia
- Install macOS sequoia using the EFI folder provided here
- After successful installation, install and open OCLP (OpenCore Legacy Patcher): 
  https://github.com/dortania/OpenCore-Legacy-Patcher/releases/tag/2.4.1
  ![Dell-Latitude-7410-Hackintosh](https://github.com/user-attachments/assets/ab940a87-79ae-49bd-b5ef-145104bb85f2)
  Click on Start Root Patching and after it is done, Reboot!
  ![Dell-Latitude-7410-Hackintosh](https://github.com/user-attachments/assets/35ef6434-7854-426f-a2d9-b09d6d4f93f7) 
- Now after rebooting the last step is to open your config.plist in your .plist editor of choice and find the DeviceProperties tab. Under Add tab, add a hashtag # in front of the name of your network card's Device Path where the model is **BCM4360 802.11ac Wireless Network Adapter**:
  ![Dell-Latitude-7410-Hackintosh](https://github.com/user-attachments/assets/d33969bc-3b1b-483d-8508-68e5ca1d169c)
  And remove the hastag # in front of the name of your network card's Device Path where the model is **Comet Lake PCH-LP CNVi WiFi**:
  ![Dell-Latitude-7410-Hackintosh](https://github.com/user-attachments/assets/506477ae-bf93-49fe-a020-113f7bc5b90b)
- Reboot again and your WiFi and Bluetooth will start working.
- If the above steps do not work then follow this guide: https://github.com/randomappleboi/Native-Wifi-for-Hackintoshes-with-Intel-Wireless-cards-on-macOS-sequoia
  


## Additional Notes

Use one-key-hidpi to fix display scaling [https://github.com/xzhih/one-key-hidpi](https://github.com/xzhih/one-key-hidpi) <br>
Don't forget to map your USB ports [https://github.com/corpnewt/USBMap](https://github.com/corpnewt/USBMap) <br>
Don't forget to generate your own SMBIOS before installing macOS [https://github.com/corpnewt/GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)

- ENJOY!
