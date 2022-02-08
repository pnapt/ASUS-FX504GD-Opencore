# ***Notes***
## ConfigExplain.md is Based on 
   - https://github.com/PoomSmart/ASUS-FX504GE-Hackintosh/blob/master/OpenCore/README.md

   - https://github.com/RobyRew/ASUS-FX504GE-Hackintosh_OpenCore/blob/master/Docs/config.plist.md
## ***always lookout on dortania guide first (If They have an update)***
https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/coffee-lake.html#starting-point


Updated to 0.7.8 Release  by Using OC Auxiliary Tool 
Updated Kext
 Lilu 1.5.9 to 1.6.0
 WhateverGreen 1.5.6 to 1.5.7
 AppleALC 1.6.8 to 1.6.9 
IntelBluetooth Firmware 2.0.1 to 2.1.0
UpdateSMBIOSMode set to Create

# OC Config.plist
## This currently cover on OpenCore 0.7.7
## ACPI
### Add
1. `SSDT-ALS0.aml` Ambient light(Maybe not needed)
2. `SSDT-AWAC.aml` 300 Series RTC
3. `SSDT-dGPU-Off.aml` dGPU disable
4. `SSDT-EC-USBX-LAPTOP.aml` Embedded controller and USB power
5. `SSDT-HPET.aml` Generate from ssdt time
6. `SSDT-PLUG-DRTNIA.aml` CPU power management 
7. `SSDT-PMC` Shutdown Fix
8. `SSDT-PNLF.aml` Backlight Fix
9. `SSDT-SBUS.aml`SMBus support
10. `SSDT-XOSI.aml` Trackpad Fix

### Delete
-

### Patch
1. `Rename _OSI to XOSI`

### Quirks
1. `ResetLogoStatus` Enable

## Booter
### Quirks
**Enabled:**
1. `AvoidRuntimeDefrag`
2. `EnableSafeModeSlide`
3. `ProvideCustomSlide`
4. `SetupVirtualMap`
5. `RebuildAppleMemoryMap`
6. `SyncRuntimePermissions`


## DeviceProperties
### Add
| PciRoot(0x0)/Pci(0x2,0x0) | Dictionary | Keys / Values | explaination |
|:--- |:---:|:--- |:--- |
| AAPL,ig-platform-id  | DATA | 0900A53E | iGPU id |
| disable-external-gpu | DATA | 01000000 | or use -wegnoegpu (disable eGPU)|
| enable-backlight-registers-fix | DATA | 01000000 | or use -igfxblr (Blacklight Fix)|
| enable-backlight-smoother | DATA | 01000000 | or use -igfxbls (make brightness adjust smoother)|
| framebuffer-patch-enable | DATA | 01000000 | framebuffer-patch-enable |
| framebuffer-con0-enable | DATA | 01000000 | internal display |
| framebuffer-con0-type | DATA | 02000000 | internal display |
| framebuffer-con1-alldata | DATA | 0101090000080000C7010000 | HDMI fix |
| framebuffer-con1-enable | DATA | 01000000 | HDMI fix |
| framebuffer-con1-type | DATA | 00080000 | HDMI fix |

- or you can add
   - `framebuffer-stolenmem`

   - `framebuffer-fbmem`

   - `framebuffer-unifiedmem (not recommend)`

- more info on https://github.com/acidanthera/WhateverGreen

### Delete
-

## Kernel
### Add
OC Snapshot > OC Clean Snapshot
Make sure
- VoodooPS2Controller.kext/Contents/PlugIns/VoodooInput.kext is Enable

- VoodooI2C.kext/Contents/PlugIns/VoodooInput.kext is Disable

### Block
-

### Emulate
-

### Force
force `IO80211Family.kext` from `System/Library/Extensions` to have complete support for WiFi

### Patch
-

### Emulate
-

### Quirks
**Enabled:**
1. `AppleXcpmCfgLock` (We don't have options to unlock de CFG-Lock on the BIOS)
2. `DisableIoMapper` (If you have VT-d enabled on the BIOS (Prefered))
3. `DisableLinkeditJettison`
4. `PanicNoKextDump`
5. `PowerTimeoutKernelPanic`
**Disable:**
1. `XhciPortLimit` (We don't need it on monterey and we also usb mapped)
2. `CustomSMBIOSGuid` (This config set to true)

### Scheme
**Enabled:**
1. `FuzzyMatch`
2. `KernelArch x86_64`

## Misc
### Boot
1. `PickerAttributes 17`
more info in dortania guide.

### Debug
**Enabled:**
1. `ApplePainc`
2. `DisableWatchDog`

**Disable**
1. `AppleDebug` We dont need debug.

### Entries
-

### Security
**Enabled:**
1. `AllowNvramReset` (For RESET the NVRAM on picker selector)
2. `AllowSetDefault` (Default disk for multiboot)
3. `BlacklistAppleUpdate` (Stop reciving updates for Macs BIOS)
- `ScanPolicy` 0
- `SecureBootModel` Default
- `Vault` Optional

### Tools
Remove from `EFI/OC/Tools` everything


## NVRAM
### Add
| 7C436110-AB2A-4BBB-A880-FE41995C9F82 | Dictionary | Keys / Values |
|:--- |:---:|:--- |
| boot-args  | String | -v keepsyms=1 -wegnoegpu debug=0x100 agdpmod=vit9696 alcdelay=500 alcid=31 |
| run-efi-updater | String | No |
| csr-active-config | DATA | 00000000 |
| prev-lang:kbd | String | en-US:0 |   

**What each thing does:**
- `boot-args` (Boot Arguments)
   - `-v keepsyms=1 debug=0x100` (Debuging)
   - `alcid=31` (Sets de audio to port 31)
   - `-wegnoegpu` (Disable dGPU )
   - `-igfxblr` (Fix Backlight issue on Coffe Lake laptops//we don't need it. Beacuse already have enable-backlight-registers-fix in device prop)
   - `agdpmod=vit9696` (Disable board-id checker **ESSENTIAL FOR HDMI OUTPUT**)
- `run-efi-updater` (Disable macOS updates to EFI)
- `csr-active-config` (SIP configuration (Enabled), For more: [Disabling SIP](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/extended/post-issues.html#disabling-sip))
- `prev-lang:kbd` (Sets custom language, For more: [AppleKeyboardLayouts.txt(opens new window)](https://github.com/acidanthera/OpenCorePkg/blob/master/Utilities/AppleKeyboardLayouts/AppleKeyboardLayouts.txt)

### Delete
Ignore

### LegacySchema
Ignore, we have native NVRAM

### WriteFlash `Enable`

## PlatformInfo
### Automatic `enabled`

### Generic
Download [GenSMBIOS (opens new window)](https://github.com/corpnewt/GenSMBIOS), and open the *GenSMBIOS.command* with *Right-Click > Open*, follow the intructions on the Terminal Window.

| Generic | Dictionary | Keys / Values |
|:--- |:---:|:--- |
| AdviseWindows  | Boolean | False |
| SystemMemoryStatus | String | Auto |
| MLB | String | *Generate your own with [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)* |
| ProcessorType | Number | 0 |
| ROM | DATA | *[Your own MAC Address](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html#derive-the-corresponding-rom-value)* |
| SpoofVendor | Boolean | True |
| SystemProductName | String | MacBookPro15,3 |
| SystemSerialNumber | String | *Generate your own with [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)* |
| SystemUUID | String | *Generate your own with [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)* |

**These values are masked from the provided config file, make sure you enter your own before testing!**

**UpdateDataHub `Boolean` `Enable`**

**UpdateNVRAM `Boolean` `Enable`**

**UpdateSMBIOS `Boolean` `Enable`**

**UpdateSMBIOSMode `String` `Create`** 

**Notes This config set it to `Custom`**

*This will breaks Bootcamp compatibility But can boot in to windows from OC GUI without SMBIOS be Changed in windows*

**UseRawUuidEncoding** `Boolean` `False`**

## UEFI
### ConnectDrivers `Boolean` `enabled`

### APFS
Leave everything default

### Audio
#### If you want bootchime //If you don't want just Ignore.
1. `AudioDevice PciRoot(0x0)/Pci(0x1F,0x3)` For bootchime
2. `PlayChime Enable` For bootchime
3. `AudioSupport Enable` For bootchime
### Drivers
#### Must have // Order is not fixed
1. `AudioDxe.efi`
2. `HFsPlus.efi`
3. `OpenCanopy.efi`
4. `OpenRuntime.efi`

### Input
Ignore

### Output
Ignore

### ProtocolsOverride
Ignore

### Quirks
**Enabled:**
1. `DeduplicateBootOrder`
2. `ReleaseUsbOwnership` (Mainly for USB fixes)
3. `RequestBootVarRouting` (Redirects some Variables for macOS)
