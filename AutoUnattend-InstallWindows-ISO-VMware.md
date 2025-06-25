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

PS C:\WINDOWS\system32> $DiskImageResult = Mount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32_x64.iso"
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