# List Packages

```
PS C:\WINDOWS\system32> Get-Module -ListAvailable | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\install.wim.Get-Module.ListAvailable.default.txt"
PS C:\WINDOWS\system32> Get-Module -ListAvailable | Out-File -FilePath "C:\Users\Admin1\Documents\ISO Files\Get-Module.ListAvailable.default.txt"


```

```
Get-Package -AllVersions
```

Lists all apps that can be uninstalled using Settings.
```
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* , HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate

Sample Output

DisplayName                                                        DisplayVersion  Publisher               InstallDate
-----------                                                        --------------  ---------               -----------

Mozilla Firefox (x64 en-GB)                                        140.0.2         Mozilla
Mozilla Maintenance Service                                        138.0.4         Mozilla
Notepad++ (64-bit x64)                                             8.8.1           Notepad++ Team
TreeSize Free V4.7.3 (64 bit)                                      4.7.3           JAM Software            20250625
```

```
PS C:\WINDOWS\system32> Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* , HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Where-Object {$_.DisplayName -match "Mozilla"}

DisplayName                 DisplayVersion Publisher InstallDate
-----------                 -------------- --------- -----------
Mozilla Firefox (x64 en-GB) 140.0.2        Mozilla
Mozilla Maintenance Service 138.0.4        Mozilla
```

```
PS C:\WINDOWS\system32> Get-WmiObject -class win32_product
PS C:\WINDOWS\system32> get-wmiobject win32_product

Sample Output

IdentifyingNumber : {908FE6B0-80EC-7865-67BB-933F99D1E7CE}
Name              : Kits Configuration Installer
Vendor            : Microsoft
Version           : 10.1.19041.5856
Caption           : Kits Configuration Installer
```




```
PS C:\WINDOWS\system32> Get-WMIObject Win32_InstalledWin32Program

Sample Output

__GENUS          : 2
__CLASS          : Win32_InstalledWin32Program
__SUPERCLASS     :
__DYNASTY        : Win32_InstalledWin32Program
__RELPATH        : Win32_InstalledWin32Program.ProgramId="0000a7449e3da7df0faff79ef89a595109360000ffff"
__PROPERTY_COUNT : 7
__DERIVATION     : {}
__SERVER         : DESKTOP-A2DP6K0
__NAMESPACE      : root\cimv2
__PATH           : \\DESKTOP-A2DP6K0\root\cimv2:Win32_InstalledWin32Program.ProgramId="0000a7449e3da7df0faff79ef89a595109360000ffff"
Language         : 65535
MsiPackageCode   :
MsiProductCode   :
Name             : Mozilla Firefox (x64 en-GB)
ProgramId        : 0000a7449e3da7df0faff79ef89a595109360000ffff
Vendor           : Mozilla
Version          : 140.0.2
PSComputerName   : DESKTOP-A2DP6K0
```

```
PS C:\WINDOWS\system32> Get-WMIObject Win32_InstalledWin32Program | Select-Object Vendor,Name 

Sample Output

Vendor                  Name
------                  ----
Mozilla                 Mozilla Firefox (x64 en-GB)
Mozilla                 Mozilla Maintenance Service
Notepad++ Team          Notepad++ (64-bit x64)
JAM Software            TreeSize Free V4.7.3 (64 bit)
VMware, Inc.            VMware Workstation
```

```
PS C:\WINDOWS\system32> Get-WMIObject Win32_InstalledWin32Program | select Name, Version, ProgramId 

Name                                                               Version          ProgramId
----                                                               -------          ---------
Mozilla Firefox (x64 en-GB)                                        140.0.2          0000a7449e3da7df0faff79ef89a595109360000ffff
Mozilla Maintenance Service                                        138.0.4          0000433e21f69f01b2f8bcc660c8322c849b0000ffff
Notepad++ (64-bit x64)                                             8.8.1            0000dde5ceefc037ce195dfbcedfdcb18c640000ffff
TreeSize Free V4.7.3 (64 bit)                                      4.7.3            0000c415542d9af6563222f414fc92802c4b0000ffff
VMware Workstation                                                 17.6.3           0000bd3442a29e9bf8075a46413a29b2351e00000904
LibreOffice 25.2.4.3                                               25.2.4.3         0000d87deb4e05ade668d8059c6c75e83ad200000908
Microsoft Edge                                                     138.0.3351.65    00003f8f2e46ec7f8d07a0089f30c2f6a6c50000ffff
Microsoft Visual C++ 2015-2022 Redistributable (x86) - 14.36.32532 14.36.32532.0    00006f06dc414ef1a3840b0944ea6ec23f850000ffff
Microsoft Visual C++ 2015-2022 Redistributable (x64) - 14.36.32532 14.36.32532.0    0000dbb51ad0f5a7ff2deb6a7b44be1d2a4b0000ffff
Windows Assessment and Deployment Kit - Windows 10                 10.1.19041.5856  0000b6ab8ea46ef14490c9c3d5b4dd6b87650000ffff
GitHub Desktop                                                     3.5.0            0000a73b5a93ece206b2f68287a33470514300000904
Microsoft OneDrive                                                 25.115.0615.0002 0000966d700dc7065c895a1b736a7e00975d0000ffff
Lenovo Service Bridge                                              5.0.2.18         00004ad2fd3710250362be295d664083aaeb0000ffff
```

```
PS C:\WINDOWS\system32> $RemoveApps = Get-WmiObject -Class Win32_Product | Where-Object {$_.Name -match "LibreOffice"}
PS C:\WINDOWS\system32> echo $removeapps


IdentifyingNumber : {E67DBA3B-4C2A-44AC-BC4D-86EA56550BB3}
Name              : LibreOffice 25.2.4.3
Vendor            : The Document Foundation
Version           : 25.2.4.3
Caption           : LibreOffice 25.2.4.3

$RemoveApps.Uninstall()
```

Get-CimInstance