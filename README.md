# AutoUnattend.xml
Auto Unattend Notes and Examples.

This is predominantly geared towards single computers with a UEFIbios and a single hard drive used in a home or small business settings without being connected a domain server.

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-technical-reference

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-automation-overview#answer-files-named-autounattendxml-are-automatically-discovered-by-windows-setup

### Windows 11 and 10 media creation tool

Web link to the Microsoft Windows 11 download page. Download the media creation tool, install and then run. This will install the latest installation version of windows 11 onto a USB memory stick or download it as an ISO, which is a CD/DVD file that can be easily burnt to CD/DVD.

Copy the AutoUnattend.xml file to the root directory/folder of the USB stick once Windows 11 or 10 has been copied to the USB memory stick.

If everything goes smoothly, Windows will install automatically, and depending on the options and scripts that run, will also configure your version of Windows just how you like it, saving you time making Windows yours.

https://www.microsoft.com/en-us/software-download/windows11

https://www.microsoft.com/en-us/software-download/windows10

### schneegans.de

Autounattend.xml generator, containing most common settings and some of the latest changes to help customise windows.

https://schneegans.de/windows/unattend-generator/



### Microsoft web links for the autounattend.xml file.

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/changed-answer-file-settings-for-windows-11

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/components-b-unattend



### Overview of the AutoUnattend.xml file

Configurations Passes run in order and where applicable if enabled. Components are added to a configuration pass and can be added to more than one Configuration Pass for those instances where another Configuration Pass is not enabled. For a more detailed overview visit the link below.


[How Configuration Passes Work](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/how-configuration-passes-work)

In order of processing, when a pass exists. 

[windowsPE](windowsPE.md) Windows Preinstallation Environment (Windows PE) is a small operating system where settings for the installation and WinPE are set, like display resolutions, disk partitions, installation partition, licence keys and specific commands. P

[offlineServicing](offlineServicing.md)  Apply unattended Setup setting to an offline Microsoft Windows image, like drivers, language packs, update packages and other packages.

[generalize](generalize.md) The generalize pass is used to create a reference or master image that can be used throughout an organisation. Its the master image before department customisations take place in the specialize pass.

[specialize](specialize.md) The specialize pass is where machine specific settings are processed, like domain information, wifi, network, international settings, department webpages. It runs on the next reboot after the generalize pass. 

[auditSystem](auditSystem.md)  IF the optional auditMode is activated, the auditSystem pass runs as System immediately before login and auditUser and is where OEM's can install device drivers, applications and other updates.

[auditUser](auditUser.md)  IF the optional auditMode is activated, the auditUser pass runs after login as User immediately after auditSystem and is used to execute RunSynchronous or RunAsynchronous commands for the default user profile which is used to configure and personalise all subsequent user accounts. This includes HKEY_USERS\DefaultUser\

[oobeSystem](oobeSystem.md)  The oobeSystem pass is where the settings for the users first login are processed. OOBE is the users first boot experience.

[Extensions](Extensions.md) Where files and scripts can be stored in the AutoUnattend.xml before being extracted to specific locations typically on the windows partition.


Components can be added to one or more configuration passes, check the specific component webpage to see what configurations passes it can be added to. For a list of the latest components, visit the link below.

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/components-b-unattend

### How to for AutoUnattend.xml answer file

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/wsim/windows-system-image-manager-how-to-topics

### How to add drivers to offlineServicing in AutoUnattend.xml file 

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/wsim/add-a-device-driver-path-to-an-answer-file


https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/add-and-remove-drivers-to-an-offline-windows-image#add-driver-packages-to-an-offline-windows-image-by-using-an-unattended-answer-file

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-command-line-options

### How to validate an answer file

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/wsim/validate-an-answer-file

