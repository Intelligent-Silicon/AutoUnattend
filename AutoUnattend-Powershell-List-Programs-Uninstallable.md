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
PS C:\WINDOWS\system32> $RemoveApps = Get-WMIObject Win32_InstalledWin32Program | select Name, Version, ProgramId | Where-Object {$_.Name -match "Lenovo Service Bridge"}
PS C:\WINDOWS\system32> echo $removeapps

Name                  Version  ProgramId
----                  -------  ---------
Lenovo Service Bridge 5.0.2.18 00004ad2fd3710250362be295d664083aaeb0000ffff

PS C:\WINDOWS\system32> $RemoveApps.Uninstall()
Method invocation failed because [Selected.System.Management.ManagementObject] does not contain a method named 'Uninstall'.
At line:1 char:1
+ $RemoveApps.Uninstall()
+ ~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (Uninstall:String) [], RuntimeException
    + FullyQualifiedErrorId : MethodNotFound
```
```
PS C:\WINDOWS\system32> $RemoveApps = Get-WmiObject -Class Win32_Product | Where-Object {$_.Name -match "LibreOffice"}
PS C:\WINDOWS\system32> echo $removeapps


IdentifyingNumber : {E67DBA3B-4C2A-44AC-BC4D-86EA56550BB3}
Name              : LibreOffice 25.2.4.3
Vendor            : The Document Foundation
Version           : 25.2.4.3
Caption           : LibreOffice 25.2.4.3

PS C:\WINDOWS\system32> $RemoveApps.Uninstall()


__GENUS          : 2
__CLASS          : __PARAMETERS
__SUPERCLASS     :
__DYNASTY        : __PARAMETERS
__RELPATH        :
__PROPERTY_COUNT : 1
__DERIVATION     : {}
__SERVER         :
__NAMESPACE      :
__PATH           :
ReturnValue      : 0
PSComputerName   :
```

```
PS C:\WINDOWS\system32> Get-CimInstance Win32_Product | Where-Object {$_.Name -match "Vmware"}

Name             Caption                                          Vendor                                           Version                                         IdentifyingNumber
----             -------                                          ------                                           -------                                         -----------------
Kits Configur... Kits Configuration Installer                     Microsoft                                        10.1.19041.5856                                 {908FE6B0-80EC-7865-67BB-933F99D1E7CE}
Toolkit Docum... Toolkit Documentation                            Microsoft                                        10.1.19041.5856                                 {1C708552-1753-51DB-2F01-BA95CBD0CE78}
Microsoft Vis... Microsoft Visual C++ 2022 X64 Additional Runt... Microsoft Corporation                            14.36.32532                                     {0025DD72-A959-45B5-A0A3-7EFEB15A8050}
Windows Syste... Windows System Image Manager on amd64            Microsoft                                        10.1.19041.5856                                 {E42BEA87-DC2D-E775-B9AB-658F3D0EDC1A}
VMware Workst... VMware Workstation                               VMware, Inc.                                     17.6.3                                          {6E541D29-21D9-4803-A6FF-3F2E6850D284}
Windows Deplo... Windows Deployment Tools                         Microsoft                                        10.1.19041.5856                                 {5BC4FAA9-EE09-D1C1-94E4-3373144048E6}
Microsoft Vis... Microsoft Visual C++ 2022 X86 Additional Runt... Microsoft Corporation                            14.36.32532                                     {C2C59CAB-8766-4ABD-A8EF-1151A36C41E5}
Windows Deplo... Windows Deployment Customizations                Microsoft                                        10.1.19041.5856                                 {6215B93C-95ED-50CF-6F41-129747110760}
Microsoft Vis... Microsoft Visual C++ 2022 X86 Minimum Runtime... Microsoft Corporation                            14.36.32532                                     {73F77E4E-5A17-46E5-A5FC-8A061047725F}
Microsoft Vis... Microsoft Visual C++ 2022 X64 Minimum Runtime... Microsoft Corporation                            14.36.32532                                     {D5D19E2F-7189-42FE-8103-92CD1FA457C2}
```
```
PS C:\WINDOWS\system32> Get-CimInstance Win32_Product | Where-Object {$_.Name -match "Vmware"}

Name             Caption                                          Vendor                                           Version                                         IdentifyingNumber
----             -------                                          ------                                           -------                                         -----------------
VMware Workst... VMware Workstation                               VMware, Inc.                                     17.6.3                                          {6E541D29-21D9-4803-A6FF-3F2E6850D284}
```

```
PS C:\WINDOWS\system32> Get-Package -Provider Programs -IncludeWindowsInstaller

Name                           Version          Source                           ProviderName
----                           -------          ------                           ------------
Mozilla Firefox (x64 en-GB)    140.0.2                                           Programs
Mozilla Maintenance Service    138.0.4                                           Programs
Notepad++ (64-bit x64)         8.8.1                                             Programs
TreeSize Free V4.7.3 (64 bit)  4.7.3                                             Programs
VMware Workstation             17.6.3                                            Programs
GitHub Desktop                 3.5.0                                             Programs
Microsoft OneDrive             25.115.0615.0002                                  Programs
Lenovo Service Bridge          5.0.2.18                                          Programs
Microsoft Edge                 138.0.3351.65                                     Programs
Microsoft Visual C++ 2015-2... 14.36.32532.0                                     Programs
Microsoft Visual C++ 2015-2... 14.36.32532.0                                     Programs
Windows Assessment and Depl... 10.1.19041.5856                                   Programs
```

```
PS C:\WINDOWS\system32> Get-Package -IncludeWindowsInstaller

Name                           Version          Source                           ProviderName
----                           -------          ------                           ------------
Mozilla Firefox (x64 en-GB)    140.0.2                                           Programs
Mozilla Maintenance Service    138.0.4                                           Programs
Notepad++ (64-bit x64)         8.8.1                                             Programs
TreeSize Free V4.7.3 (64 bit)  4.7.3                                             Programs
VMware Workstation             17.6.3                                            Programs
GitHub Desktop                 3.5.0                                             Programs
Microsoft OneDrive             25.115.0615.0002                                  Programs
Lenovo Service Bridge          5.0.2.18                                          Programs
Microsoft Edge                 138.0.3351.65                                     Programs
Microsoft Visual C++ 2015-2... 14.36.32532.0                                     Programs
Microsoft Visual C++ 2015-2... 14.36.32532.0                                     Programs
Windows Assessment and Depl... 10.1.19041.5856                                   Programs
```
https://devblogs.microsoft.com/scripting/use-powershell-to-find-and-uninstall-software/


Get-package | Where-Object {$_.Name -match "Lenovo Service Bridge"} | uninstall-package 

Get-CimInstance