# Config.plist setup
## [Here](ConfigExplain.md)
PS : ***This not cover all the topic you should go read on dortania guide to understand it more***


- **More Details on dortania guide : https://dortania.github.io/OpenCore-Install-Guide**

- **Dortania guide coffee-lake(8th gen) : https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/coffee-lake.html**

# Post-Install
**Details Info on : https://dortania.github.io/OpenCore-Post-Install/**

**Necessary Apps**
1. MountEFI https://github.com/corpnewt/MountEFI
2. GenSMBIOS https://github.com/corpnewt/GenSMBIOS
3. ProperTree https://github.com/corpnewt/ProperTree

**What need to be done after install macos**
1. generate you own SMBIOS (This config use `MacbookPro15,2` SMBIOS ) 

// PS.If you change to something else you need to change in USBMap.kext too
2. install Combojack Switcher https://github.com/hackintosh-stuff/ComboJack

**How-to Install Combojack switcher**
- Put Combojack folder in user/youraccountname first or cd combojack folder

- Open Terminal Type : ```bash install.sh ```

- then follow the prompt (VerbStub.kext already include in EFI)
- restart
- Try plug-in your headphone If you get prompt , Then patch is working

**Nice tools that should try**
- OCAuxiliaryTools https://github.com/ic005k/OCAuxiliaryTools

**Misc Fixes**
- Fix Dualboot timezone: https://www.tonymacx86.com/threads/fix-incorrect-time-in-windows-osx-dual-boot.133719/
