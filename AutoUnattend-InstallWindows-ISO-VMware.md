# Install Windows 10 x32bit

## ISO file for VMware

Setup for:
VMWare Workstation 17 on Win11 Home
British English
Single Hard Drive
Ethernet
No Bluetooth
No Printer
VMware Tools Installed

Download Windows 10 2022 Update | version 22H2 Media Creation Tool

Image Version: 10.0.19041.1

https://www.microsoft.com/en-gb/software-download/windows10

Download Windows x32 using the Media Creation Tool. VMware cant handle an ISO with both x64 and x32 versions of windows on it.

x86 & x32 = 32bit Windows.
x64 = 64bit Windows.

Download the Windows Assessment and Deployment Kit (ADK) for [Windows 10 2004](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install#other-adk-downloads).

This is the last version of the ADK which can build Windows x32 versions.
https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/download-winpe--windows-pe

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-create-usb-bootable-drive?view=windows-11

When installing the ADK, select only the Deployment Tools option.
Then install adkwinpesetup_2004.exe

This will create a new entry in the Start menu called "Window Kits".
Load the Deployment and Imaging Tools Environment which will open a special DOS window with extra path settings.
You can see these by typing ```echo %path%```, the extra path settings are:
C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools\AMD64\DISM;
C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools\AMD64\Imaging;
C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools\AMD64\BCDBoot;
C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools\AMD64\Oscdimg;
C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools\AMD64\Wdsmcast;
C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools\HelpIndexer;
C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools\WSIM;
C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment;
C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Imaging and Configuration Designer\x86;

Next type ```copype x86 C:\mount_Win10x32_WinPE```. 
This will create a folder called: ```C:\mount_Win10x32_WinPE``` and will then create & copy, a number of subfolders and files to the ```C:\mount_Win10x32_WinPE``` folder.

Once completed, close the window.



The first stage is to prepare the WindowsPE boot.wim image file.
Mount the ```boot.wim``` image file to the folder ```C:\mount_Win10x32_WinPE\mount```

```
PS C:\WINDOWS\system32> Dism /Mount-Image /ImageFile:"C:\mount_Win10x32_WinPE\media\sources\boot.wim" /Index:1 /MountDir:"C:\mount_Win10x32_WinPE\mount"
```

Check that the ```boot.wim``` has mounted properly with 
```
Dism /Get-MountedImageInfo
```

In Notepad or Notepad++ running as Administrator, edit the file: ```C:\WinPE_amd64\mount\windows\system32\startnet.cmd```, adding the command ```powercfg /s 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c``` to set the WindowsPE power scheme to High Performance. 

```
wpeinit
powercfg /s 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c
```

When saving the ```startnet.cmd``` file, if you get an error message saying it cant be saved, then the text editor is not running with elevated permissions as the Administrator. Reload the text editor so its running as the Administrator.

Next apply windows updates to the Boot.wim.

Note the version of windows found in Boot.wim is the Image Version: 10.0.19041.1 which is 2004. At the time of writing 20250613:YYYYMMDD there is no offline package file to download and use to up date Boot.wim.



https://www.catalog.update.microsoft.com/Search.aspx?q=windows%2010%202004%20x86
Sort by "Last Updated" so the most recent update "windows10.0-kb5008212-x86" is at the top of the list.

For a description of the standard terminology that is used to describe most of the Microsoft software updates.
https://learn.microsoft.com/en-gb/troubleshoot/windows-client/installing-updates-features-roles/standard-terminology-software-updates

Adding packages can take a very long time on slow or under powered computers, so to speed up the process, switching off RealTime protection in Windows Defender can seriously speed up the process. Its not ideal doing this, but if you get inpatient or are short on time, this is an option. 
Its also advisable to disconnect from the internet at this stage.

```
PS C:\WINDOWS\system32>  Dism /Add-Package /Image:"C:\mount_Win10x32_WinPE\mount" /PackagePath:"C:\Users\Admin1\Documents\ISO Files\windows10.0-kb5008212-x86_ae7611462ceaac2536f35f1b1fa34a2add032a63.msu"
```

Here is where you add any special drivers needed for the computer, but as this is an ISO for VMware, we dont need to add any special drivers.

For more help on customising the windowsPE boot.wim file, use the link below.
https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-mount-and-customize?view=windows-11#set-the-power-scheme-to-high-performance

Next cleanup the image, remove old updates and leave the most current update kb5008212 in place.

```
md C:\temp
PS C:\WINDOWS\system32> Dism /Cleanup-Image /Image:"C:\mount_Win10x32_WinPE\mount" /Startcomponentcleanup /Resetbase /ScratchDir:"C:\temp"
```

Unmount the boot.wim image

```
PS C:\WINDOWS\system32> dism /unmount-image /mountdir:"C:\mount_Win10x32_WinPE\mount" /commit
```

If you switched off RealTime protection, now is the time to switch it back on, and you can reconnect to the internet now.


Now reopen the Deployment and Imaging Tools Environment command window again and run this command

Makewinpemedia /iso C:\mount_Win10x32_WinPE C:\mount_Win10x32_WinPE\WinPEx32.iso
 
https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-mount-and-customize?view=windows-11#add-updates-to-winpe-if-needed



Create the following Folder: c:\mount_WinPEx32
Open Command Window and run the following command:

cd "C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment"

copype x86 C:\WinPEx32

Makewinpemedia /iso C:\mount_Win10x32_WinPE C:\mount_Win10x32_WinPE\WinPEx32.iso

Load Powershell, check ExecutionPolicy is set to Bypass, Mount ISO as Cd/DVD, Get ISO Drive Letter, Copy ISO contents to C:\Mount, Dismount ISO. 
```
PS C:\WINDOWS\system32> Get-ExecutionPolicy -list

PS C:\WINDOWS\system32> $DiskImageResult = Mount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32.iso"
PS C:\WINDOWS\system32> $DiskImageDriveLetter = ($DiskImageResult | Get-Volume).DriveLetter
PS C:\WINDOWS\system32> Copy-Item -Path "$($DiskImageDriveLetter):\*" -Destination "C:\mount" -Recurse
PS C:\WINDOWS\system32> Dismount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32.iso"
```
Check what versions & variants of Windows 10 exist in the install.esd file.
```
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount\sources\install.esd" | Out-File -FilePath "C:\mount\sources\install.esd.txt"
```

Create AutoUnattend.xml file and save it to C:\mount.

Save the [AutoUnattend-InstallWindows-ISO-VMware-XML.md](AutoUnattend-InstallWindows-ISO-VMware-XML.md} and save it to C:\mount as AutoUnattend.xml

```
PS C:\WINDOWS\system32> import-Module "C:\Users\Admin1\Documents\ISO Files\New-ISOFile.psm1" 
PS C:\WINDOWS\system32> New-ISOFile "C:\mount" "C:\Users\Admin1\Documents\ISO Files\AU_Win10_22H2_x32.iso" -verbose
```



https://www.reddit.com/r/vmware/comments/171pf4m/comment/kfvtrtf/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button

 



Create a new virtual machine and load the Microsoft Windows 10 22H2 x86 (32bit) (AU_Win10_22H2_x32.iso) ISO to detect recommended settings.

Recommended Settings for Win 10 when it auto detects the Win10_22H2_x32.iso
Memory 1GB
Processors 2
Hard Disk (NVMe) 60GB, Single file and let it expand with use to avoid taking up all your disk space straight away.
CD/DVD using ISO Image C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32.iso
Network Adapter NAT
USB Controller Present
Sound Card Auto Detect
Display Auto Detect
Firmware Type Bios

Create a new Virtual Machine with the above settings, select a single file for the Hard Disk for performance.
Add the operating later on.
Save.
Locate the ```.VMX``` file for the newly created Virtual Machine. It will be something like 
C:\Users\Admin1\Documents\Virtual Machines\Win10x32\Win10x32.vmx where Win10x32 was the name specified for the virtual machine.

Open the .VMX file in Notepad++ or MS Notepad, search using Find "ulm.disableMitigations".
If a line entry exists, make sure its set to "TRUE", if it doesnt exist add the following line to the bottom of the list of line entrys, the order is not important:
ulm.disableMitigations = "TRUE"

This option switchs off Side Channel mitigation code to help mitigate against Side Channel attacks. 
If this option is switched on, which is the default option, then the virtual machine will run incredibly slow on slow machines.
Switching Side Channel mitigation code off will speed up the virtual machine so it can run smoothly. 

Spooky hackers still need to get onto the virtual machine before they can exploit this attack vector, so your firewall and other security measures should prevent them from getting onto your machine, especially if it doesnt need to go online. If you go online, you then become dependant on others working properly to ensure you dont end up on a website or have adverts delivered to your machine which doesnt attack your machine.



