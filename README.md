# OpenCore_HP-EliteDesk-800-G5-DM
Here a the files and guides I used for the installation of macOS Catalina on HP EliteDesk 800 G5 Desktop Mini

## Description
Product Name : HP EliteDesk 800 G5 Mini / HP EliteDesk 800 G5 Desktop Mini

Product N° 7YX38PA

- CPU : Intel Core i5-9500T 2.2 GHz (boost to 3.7 GHz)
- GPU : Intel Graphics UHD630 (Dual DisplayPort, additional DisplayPort)
- Memory : 2 Slots DDR4-2400
- Storage : PCIe NVMe 256 GB, PCI-E Gen3 x 4, Turbo Drive G2 Toshiba KXG50ZNV256G 
- Ethernet : Intel I219LM Gigabit Network Connection LOM
- Wireless LAN : Intel 7265 802.11ac 2x2 Wi-Fi with Bluetooth M.2 Combo Card 
- Audio : Conexant CX20632 (Internal Speaker, 2 audio out front ports)

## Known Issues
- Intel WiFi does not work, but Bluetooth can be made to work with a kext

## Installation
This guide isn't a Vanilla install guide since I couldn't get the BCM94352Z to work with kexts installed on the EFI partition
For a Vanilla install, simply skip the Wireless LAN part and use a Wifi-Bluetooth USB Key instead

### BIOS Settings
- Disable Fast Boot
- Enable USB boot
- UEFI Boot order, make sure your USB device is first to boot
- Secure Boot : Legacy Support Disable and Secure Boot Disable
- Disable : Virtualization Technology for Directed I/O (VTd)
- Enable : M.2 SSD (if you use a NVMe SSD)
- Uncheck M.2 WLAN/BT (if you don't have a compatible M.2 Combo Card)
- Disable Wake on lan
- Set Video Memory to 64 MB

### Installation 
Create a USB Key with Catalina following the Dortaina OpenCore guide

## Post Installation
### Add OpenCore to Boot disk
The Dortania Post-Install guide demonstrates how to install OpenCore to the boot drive

### Add kexts and ssdt patches
Copy the EFI folder on your Installation key on the EFI of your 
Add the following kext in **/EFI/OC/Kexts**
- Lilu.kext (https://github.com/acidanthera/Lilu)
- AppleALC.kext (https://github.com/acidanthera/AppleALC)
- IntelMausi.kext (https://github.com/acidanthera/IntelMausi)
- WhateverGreen.kext (https://github.com/acidanthera/WhateverGreen)
- VirtualSMC.kext (https://github.com/acidanthera/VirtualSMC)
- USBInjectAll.kext (https://github.com/RehabMan/OS-X-USB-Inject-All)
- IntelBluetoothFirmware.kext (https://github.com/OpenIntelWireless/IntelBluetoothFirmware)
- IntelBluetoothInjector.kext (https://github.com/OpenIntelWireless/IntelBluetoothFirmware - optional, adds BT to the system settings)
- NVMeFix.kext (https://github.com/acidanthera/NVMeFix)
- SMCProcessor.kext (https://github.com/acidanthera/VirtualSMC)
- SMCSuperIO.kext (https://github.com/acidanthera/VirtualSMC)

Post-install USBMap can be used to map the actual USB ports on the system, one is included in this repo

The Dortania install guide details how to create SSDT patches.
The following patches were needed on the EliteDesk 800 G5
- SSDT-AWAC
- SSDT-EC
- SSDT-PLUG
- SSDT-PMC
- SSDT-USBX

#### config.plist
**ACPI Section**

Add the following DSDT Patches

- RTC Fix to stop POST error
  - Find : <47017000 70000108>
  - Replace with : <47017000 70000102>

**Graphics Section**
- Inject Intel
- ig-platform-id : 07009B3E

**SMBIOS**
use iMac19,1 (2019)

**If you use the attached config.plist generate a new Serial Number and a new SmUUID**

