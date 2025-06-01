# windowsPE

## Overview

Windows Preinstallation Environment (Windows PE) is a small operating system where settings for the installed copy of Windows and WinPE are set, like display resolutions, disk partitions, installation partition, licence keys and specific commands, although some settings are only applicable to computers using the older BIOS and not the newer UEFI bios.

To add out-of-box, boot-critical drivers during an unattended installation, you must make sure that the boot-critical driver is available on preinstallation media. Boot-critical drivers will include drivers for Raid controllers, to configure the Raid drives before windows can be installed. In practice, this is typically needed for servers and high end desktops which is beyond the scope of this repo. 

If you need windows to connect to network shares and/or online network shares, boot-critical network drivers will also need to be installed from the windowsPE pass.

This repo is geared for installing windows in an offline situation for your typical desktop or laptop at home or small business.


https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windowspe

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-intro

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/wpeinit-and-startnetcmd-using-winpe-startup-scripts#supported-unattend-settings


[windowsPE AutoUnattend.xml](windowsPE-AutoUnattend.md)

# windowsPE Components

[Microsoft-Windows-International-Core-WinPE](Microsoft-Windows-International-Core-WinPE.md) Configuring the locale settings like timezone, keyboard layout for use during installation and after installation.


[Microsoft-Windows-Setup](Microsoft-Windows-Setup.md) Configure the hard disk partitions, run boot-critical drivers and other programs for the installation process. 


[Microsoft-Windows-PnpCustomizationsWinPE](Microsoft-Windows-PnpCustomizationsWinPE.md) Configuring and installing drivers to be include in the windows driver store for the windows installation and windowsPE boot-critical drivers.



