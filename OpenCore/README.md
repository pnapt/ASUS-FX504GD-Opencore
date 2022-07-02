# Guide&Info
Information will be here
## How to create/setup config.plist
### **[Here](ConfigExplain.md)**

- PS : ***This not cover all the topic you should go read on dortania guide to understand it more***
 - **More Details on dortania guide : https://dortania.github.io/OpenCore-Install-Guide**

 - **Dortania guide coffee-lake(8th gen) : https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/coffee-lake.html**

### SSDT
- [SSDTTime](https://github.com/corpnewt/SSDTTime)
### USBMap Kext
- [USBMap](https://github.com/corpnewt/USBMap)
- This laptop config [COMING SOON](USBMapExplain.md)

# Necessary Apps
1. [MountEFI](https://github.com/corpnewt/MountEFI)
2. [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
3. [ProperTree](https://github.com/corpnewt/ProperTree)

# Installation
## USB Install
- [Here](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/)

## BIOS Settings
1. Secure Boot: Disabled
2. Fastboot : Disable
2. SATA mode: AHCI
3. DVMT-Preallocated: 64MB

# Post-Install
**Details Info on : https://dortania.github.io/OpenCore-Post-Install/**

## What need to be done after install macos
### 1. Generate you own SMBIOS (This config use `MacbookPro15,2` SMBIOS ) 
- Notes :If you change to something else you need to change in USBMap.kext too

### 2. Install Combojack Switcher https://github.com/hackintosh-stuff/ComboJack
**How-to Install Combojack switcher**
- Follow Full instruction on [Here](https://github.com/hackintosh-stuff/ComboJack)

  - This is short-cut version

  - Download This [Repo](https://github.com/hackintosh-stuff/ComboJack)

  - 1. Open Terminal Type : ```cd /ComboJack/ComboJack_Installer/```

  - 2. Type : ```bash install.sh ```

  - 3. follow the prompt (VerbStub.kext already include in EFI)
  
  - 4. restart

  - 5. Try plug-in your headphone If you get prompt , Then patch is working enjoy

## Enable Secureboot
<p align="left">
    <a href="https://github.com/pnapt/ASUS-FX504GD-Opencore/blob/main/OpenCore/Doc/Sign-Command.md">
        <img src="https://img.shields.io/badge/How%20to-SecureBoot-red" /></a>
# Misc

 **Nice tools that you should try**
- OCAuxiliaryTools-CrossPlatform Plist edit tools: https://github.com/ic005k/OCAuxiliaryTools

**Fixes**
- Fix Dualboot timezone: https://www.tonymacx86.com/threads/fix-incorrect-time-in-windows-osx-dual-boot.133719/
