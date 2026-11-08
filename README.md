<div align="center">

<!-- Replace with your banner: 1280x640 PNG, saved in /assets -->
<img src="https://github.com/user-attachments/assets/08571777-a263-48d4-bf66-990fc4eb10d2" alt="Dell Latitude 7410 running macOS Tahoe" width="100%">

# Dell Latitude 7410 · macOS Hackintosh

**A daily-driver OpenCore EFI for the Dell Latitude 7410.**
Sleep, audio, Wi-Fi, Bluetooth, iServices and Thunderbolt all working.

<br>

![macOS Tahoe](https://img.shields.io/badge/macOS-Tahoe%2026.7.1-000000?style=for-the-badge&logo=apple&logoColor=white)
![OpenCore](https://img.shields.io/badge/OpenCore-1.0.8-4a5568?style=for-the-badge)
![SMBIOS](https://img.shields.io/badge/SMBIOS-MacBookPro16,3-2d3748?style=for-the-badge)
![Stars](https://img.shields.io/github/stars/abbasabidii/Dell-Latitude-7410-Hackintosh-OC?style=for-the-badge&logo=github&color=1e2327)

[**Download EFI**](https://github.com/abbasabidii/Dell-Latitude-7410-Hackintosh-OC/releases) ·
[**HiDPI**](#-hidpi) 

</div>

---

## What's working

| Graphics & display | Audio | 
| :-- | :-- |
| ✅ iGPU acceleration | ✅ Internal speakers |
| ✅ HDMI + HDMI audio | ✅ Internal microphone |
| ✅ USB-C DP Alt Mode | ✅ Headphone jack ([ComboJack](https://github.com/macos86/ComboJack)) |
| ✅ Webcam | ✅ Airpods & Apple Music (Downloaded Songs) |

| Connectivity | Power |
| :-- | :-- |
| ✅ Wi-Fi 2.4 + 5 GHz | ✅ CPU SpeedStep |
| ✅ Bluetooth LE + AirPods audio | ✅ Battery management |
| ✅ AirPlay, screen mirroring, Handoff | ✅ Sleep / wake | 
| ✅ Hibernation S4 | ✅ iServices (iMessage, FaceTime, Phone) |

| Input | Ports |
| :-- | :-- |
| ✅ Touchpad, gestures + buttons | ✅ USB 3.0 |
| ✅ Fn hotkeys (volume `F2/F3`, brightness `F6/F7`) | ✅ USB-C charging + data |
| ✅ Bluetooth Keyboard & Mouse | ✅ microSD reader |
| ✅ Continuity Camera (Via USB only) | ✅ Thunderbolt (plug in before boot) |

> [!NOTE]
> Not working: nothing found so far. iPhone Mirroring is not supported.

---

## 💡 Quick start

1. Download the EFI for your macOS version from [**Releases**](https://github.com/abbasabidii/Dell-Latitude-7410-Hackintosh-OC/releases).
2. Generate **your own SMBIOS** with [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) and map your ports with [USBMap](https://github.com/corpnewt/USBMap).
3. Remove any `Device Properties` entries your laptop doesn't have.
4. Apply the [BIOS settings](#-bios-settings) below, then install.

---

## 🧩 Compatibility

| macOS | Status | SecureBootModel | Notes |
| :-- | :-- | :-- | :-- |
| Tahoe 26 | ✅ Compatible | Default | Laobamac [Airportitlwm](https://github.com/laobamac/itlwm/releases) and [SimpleHDA](https://github.com/laobamac/SimpleHDA/releases) for Wifi and Audio |
| Sequoia 15 | ✅ Daily driver (15.8.2) | `j215` or `j223` | Working iServices setup OOB |
| Sonoma 14 | ✅ Compatible | Disabled | See Releases | Disabled due to installer boot and OCLP Wi-Fi patching issues | 
| Ventura 13 | ✅ Compatible | `j215` or `j223` | Working iServices setup OOB |
---

## 💻 Specs

| Hardware | Details |
| :-- | :-- |
| CPU | Intel Core i7-10610U vPro |
| iGPU | Intel UHD Graphics 620 (Comet Lake) |
| RAM | 16 GB DDR4 2667 MHz |
| Storage | 512 GB KIOXIA KBG40ZNS512G NVMe |
| Wi-Fi | Intel Wi-Fi 6 AX201 CNVi |
| Display | 14" 1920×1080 Full HD IPS |
| Audio | Realtek ALC295 (layout ID `33`) |
| BIOS | 1.30.0 |

---

## ⚙️ BIOS settings

| Setting | Value |
| :-- | :-- |
| Boot mode | UEFI |
| Fast Boot | Thorough |
| Secure Boot | Disabled |
| SATA mode | AHCI |
| Thunderbolt | No Security |
| Enable Thunderbolt Boot Support | Enabled |
| Wake on AC | Disabled |

---

## 🔧 UEFI Settings Mod

This is not required to boot macOS at all and can be skipped, but is done to enable features that are otherwise would not be available.

We can use [ModGrubShell](https://github.com/datasone/grub-mod-setup_var/releases) to modify settings (Can be used as a tool in OC)

For Dell BIOS IFR data extraction to find VarOffset, please refer to Dreamwhite's [guide](https://github.com/dreamwhite/bios-extraction-guide).

BIOS Version: 1.40.0

UEFI IFR settings
| UEFI Setting Name | Varstore | VarOffset | Value |
| :-- | :-- | :-- | :-- |
| CFG Lock | CpuSetup | 0x3E | 0x1(Enable) 0x0(Disable) |
| TB Auto Switch | Setup | 0xA08 | 0x1(Enable) 0x0(Disable) |
| TB BIOS Enumeration Mode | Setup | 0xA09 | 0x1(Native) 0x0(BIOS Assist) | 

- Disable CFG Lock -> (VarStore: CpuSetup, VarOffset: 0x3E)

       setup_var_cv CpuSetup 0x3E 0x1 0x0     (This will disable CFG Lock)
       setup_var_cv CpuSetup 0x3E 0x1 0x1     (Back to default)
  
         (setup_var_cv,VarStore,VarOffset,Size,Value)

> [!NOTE]
> [Why disable CFG Lock?](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html#what-is-cfg-lock)

---

## 🛠 Tahoe audio & Wi-Fi patching

Do this after a successful Tahoe install.

**Preparation**

- Install macOS Tahoe using the EFI from this repo.

**Audio**

- Go to Laobamac's [SimpleHDA](https://github.com/laobamac/SimpleHDA/releases)
- Download and extract the RELEASE package from Releases, and place SimpleHDA.kext in EFI/OC/Kexts.
- Add the driver in Kernel → Add, in the order of Lilu → AppleALC → SimpleHDA. An example of OpenCore-Kernel-Add.plist entries is included in the package.
- Confirm that Kernel → Quirks → DisableLinkeditJettison is true, preserving the existing AppleALC, layout, and ACPI configurations for this machine.
- Optional: Install SimpleHDA-HAL-1.0.1.pkg.

Reboot.

**Wi-Fi**

- Install Laobamac's [Airportitlwm](https://github.com/laobamac/itlwm/releases)

---

## 🖥 HiDPI

HiDPI at **1600×900** works using [one-key-hidpi](https://github.com/xzhih/one-key-hidpi).

1. Run one-key-hidpi and choose **Manual input resolution**.
2. Enter your display's supported resolutions plus their 2× versions. For this 1920×1080 panel:

   ```
   3840x2160 1920x1080 3200x1800 1600x900 2732x1536 1366x768 2560x1440 1280x720
   ```
3. Reboot. HiDPI is now available at 1600×900.

<!-- Add your screenshot: assets/hidpi.png -->
<img width="1600" height="900" alt="HiDPI Display Settings" src="https://github.com/user-attachments/assets/cc58887f-d402-4fcf-9676-03294d574f7e"/>

> [!WARNING]
> On Tahoe, **1600×900** HiDPI causes sluggishness because Tahoe is heavy on the GPU. Use **1366×768** for a snappier experience, or stay on Sequoia for maximum smoothness because after [recent macOS Sequoia 15.8 update Apple Updated Graphics drivers](https://www.reddit.com/r/hackintosh/s/qp8lkdRXIy) and now iGPU works super smooth in Sequoia.

---

## 🔋 Power management

| Setting | Value |
| :-- | :-- |
| Thunderbolt BIOS Enumeration mode | BIOS Assist |
| BIOS Enumeration Auto Switch | Disabled |
| Realtek card reader | Disabled in BIOS |
| CPUFriend | Enabled |

<!-- Add your screenshot: assets/power.png -->
<img width="373" height="621" alt="Screenshot 2026-07-21 at 7 47 18 PM" src="https://github.com/user-attachments/assets/4c2f09c2-dacd-4e57-8dd5-e18d7becd733"/>

> [!NOTE]
> The trade-off for good power management is that Thunderbolt **hotplug is disabled**. Plug devices in before booting.

---

## ⚠️ Known Issues

<details>
<summary><b>High GPU usage in the Tahoe Apps menu and Adobe apps</b></summary>

<br>

Opening the new Launchpad-style Apps menu can spike WindowServer GPU usage above 90%. Photoshop and Illustrator show the same behaviour. Running these apps in **fullscreen** keeps them smoother and avoids the spike.

</details>

<details>
<summary><b>Uploads over 1 MB fail on Google Drive or Google Photos</b></summary>

<br>

On some Wi-Fi networks, make sure Wi-Fi is at the top of the service order in **System Settings → Network**. If that doesn't help, try Google Chrome.

</details>

<details>
<summary><b>Apple Music skips tracks over Bluetooth</b></summary>

<br>

See this [workaround on r/hackintosh](https://www.reddit.com/r/hackintosh/comments/uip41i/workaround_apple_music_skipping_tracks_when_using/).

</details>

---

## 🙌 Credits & disclaimer

Credit goes to Apple for macOS, and to the developers of Clover, OpenCore, the kexts and the tools that make Hackintoshing possible. I gathered information from across the community and wrote this guide after a lot of trial and error, hacking since 2018 with my first working Hackintosh in 2019.

This project is for educational purposes only, if this EFI helped you, a ⭐ is much appreciated.
