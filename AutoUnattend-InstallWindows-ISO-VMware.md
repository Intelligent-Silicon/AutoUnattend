# Install Windows 10 x32bit

## ISO file for VMware

Setup for:
British English
Single Hard Drive
Ethernet
No Bluetooth
No Printer
VMware Tools Installed

Download Windows 10 2022 Update | version 22H2 Media Creation Tool

https://www.microsoft.com/en-gb/software-download/windows10

Mount ISO file: C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32_x64.iso

Load Powershell, check ExecutionPolicy is set to Bypass, Mount ISO as Cd/DVD, Get ISO Drive Letter, Copy ISO contents to C:\Mount, Dismount ISO. 
```
PS C:\WINDOWS\system32> Get-ExecutionPolicy -list

PS C:\WINDOWS\system32> $DiskImageResult = Mount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32.iso"
PS C:\WINDOWS\system32> $DiskImageDriveLetter = ($DiskImageResult | Get-Volume).DriveLetter
PS C:\WINDOWS\system32> Copy-Item -Path "$($DiskImageDriveLetter):\*" -Destination "C:\mount" -Recurse
PS C:\WINDOWS\system32> Dismount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32_x64.iso"
```
Check what versions & variants of Windows 10 exist in the install.esd file.
```
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount\x64\sources\install.esd" | Out-File -FilePath "C:\mount\x64\sources\install.esd.txt"
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount\x86\sources\install.esd" | Out-File -FilePath "C:\mount\x86\sources\install.esd.txt"
```

Create AutoUnattend.xml file and save it to C:\mount.

Save the [AutoUnattend-InstallWindows-ISO-VMware-XML.md](AutoUnattend-InstallWindows-ISO-VMware-XML.md} and save it to C:\mount as AutoUnattend.xml

```
PS C:\WINDOWS\system32> import-Module "C:\Users\Admin1\Documents\ISO Files\New-ISOFile.psm1" 
```



https://www.reddit.com/r/vmware/comments/171pf4m/comment/kfvtrtf/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button

VMWare Workstation 17 on Win11 Home 

Download Windows x32 using the Media Creation Tool. VMware cant handle an ISO with both x64 and x32 versions of windows on it.

Create a new virtual machine and load the Microsoft Windows 10 22H2 x86 (32bit) ISO to detect recommended settings.

Recommended Settings for Win 10 when it auto detects the Win10_22H2_x32.iso
Memory 1GB
Processors 2
Hard Disk (NVMe) 60GB
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
If a line entry exists, make sure its set to "TRUE", if it doesnt exist add the following line to the bottom of the line entrys:
ulm.disableMitigations = "TRUE"

This option switchs off Side Channel mitigation code to help mitigate against Side Channel attacks. 
If this option is switched on, which is the default, then the virtual machine will run incredibly slow on slow machines.
Switching Side Channel mitigation code off will speed up the virtual machine so it can run smoothly. 

Spooky hackers still need to get onto the virtual machine before they can exploit this attack vector, so your firewall and other security measures should prevent them from getting onto your machine, especially if it doesnt need to go online. If you go online, you then become dependant on others working properly to ensure you dont end up on a website or have adverts delivered to your machine which doesnt attack your machine.

