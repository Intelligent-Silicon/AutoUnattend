# VMware Windows 10 Pro

## Create Master copy

[1. Download Windows 10 and copy the ISO to a folder.](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#1-download-windows-10-and-copy-the-iso-to-a-folder)

[2. Export the required version of Windows from the ```\sources\install.esd``` to a ```\sources\install.wim``` file.](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#2-export-the-required-version-of-windows-from-the-sourcesinstallesd-to-a-sourcesinstallwim-file)

[3. Download the Deployment Tools from the Windows ADK.](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#3-download-the-deployment-tools-from-the-windows-adk)

[4. Check what Features, Packages, & KB's are installed (Optional)](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#4-check-what-features-packages--kbs-are-installed-optional)

[4.1 To check the ```boot.wim``` which contains the ```WindowsPE``` image used in the ```WindowsPE``` configuration pass.](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#41-to-check-the-bootwim-which-contains-the-windowspe-image-used-in-the-windowspe-configuration-pass)

[4.2 To check the ```boot.wim``` which contains the ```Windows Setup``` image used in the ```WindowsPE``` configuration pass.](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#42-to-check-the-bootwim-which-contains-the-windows-setup-image-used-in-the-windowspe-configuration-pass)

[4.3 To check the ```install.wim``` which contains the main Windows image used to install Windows onto a computer.](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#43-to-check-the-installwim-which-contains-the-main-windows-image-used-to-install-windows-onto-a-computer)

[4.4 Checking for installed Updates Summary](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#44-checking-for-installed-updates-summary)


[5. Create an AutoUnattend.xml Answer File to initially setup Windows.](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#5-create-an-autounattendxml-answer-file-to-initially-setup-windows)


6. Install Windows Using AutoUnattend.xml File  

Download Windows updates using the update services.
7. Install required End User Apps.
8. Finish setting up Windows and tweaking it to suit.
9. Generalise the Image.
10. Capture the Image.
11. Keep this as a master copy of the Vmware.



### 1 Download Windows 10 and copy the ISO to a folder.

https://www.microsoft.com/en-gb/software-download/windows10

Save as MediaCreationTool_Win10_22H2.exe

Run the MCT and save ISO as Win10_22H2_x32.iso. You'll need 40GB of storage space.

```PS C:\WINDOWS\system32>``` means this is Powershell running elevated as the Administrator.

```PS C:\Users\Admin1>``` or ```PS C:\Users\[UserName]>``` means this is Powershell running unelevated as the user account.

```
PS C:\WINDOWS\system32> $DiskImageResult = Mount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32.iso"
PS C:\WINDOWS\system32> $DiskImageDriveLetter = ($DiskImageResult | Get-Volume).DriveLetter
PS C:\WINDOWS\system32> md -path "C:\mount_Win10_22H2_x32_ISO"
PS C:\WINDOWS\system32> Copy-Item -Path "$($DiskImageDriveLetter):\*" -Destination "C:\mount_Win10_22H2_x32_ISO" -Recurse
PS C:\WINDOWS\system32> Dismount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32.iso"
```

### 2 Export the required version of Windows from the ```\sources\install.esd``` to a ```\sources\install.wim``` file.

We need to export from a ```ESD``` file and create a ```WIM``` file because the ADK Windows System Image Manager (SIM) only uses ```WIM``` files.

Text files are made to document states before and after change, to help track down unforeseen problems.

```
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount_Win10_22H2_x32_ISO\sources\install.esd" | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\install.esd.txt"
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount_Win10_22H2_x32_ISO\sources\install.esd" /index:7 | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\install.esd.7.txt"
```

The N variants stand for "Not with Windows Media Player" and related Media Player apps, to comply with European Union law.

```
PS C:\WINDOWS\system32> dism /export-image /SourceImageFile:"C:\mount_Win10_22H2_x32_ISO\sources\install.esd" /SourceIndex:7 /DestinationImageFile:"C:\mount_Win10_22H2_x32_ISO\sources\install.wim" /Compress:max /CheckIntegrity
```
As this is a new ```WIM``` file, its relative index position will be 1, if you were to export more images to the destination ```WIM``` file its relative index position will increase by 1.

```
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount_Win10_22H2_x32_ISO\sources\install.wim" | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\install.wim.txt"
```


### 3 Download the Deployment Tools from the Windows ADK.

As we are trying to install a 32bit version of Windows, we need the last version of ADK which supports 32bit installations.

[Main ADK download page](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install)

The last version of ADK which supported 32-bit Windows is version 2004. 
Click the link below, scroll down to "```Download the ADK for Windows 10, version 2004 (Republished in May 2025)```" or click on the two Download links below it to download the programs directly.

[Main ADK download Page - Other versions](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install#other-adk-downloads)

[Download Windows ADK for Windows 10, version 2004 ```adksetup.exe```](https://go.microsoft.com/fwlink/?linkid=2120254)

[Download Windows PE add-on for the ADK, version 2004 ```adkwinpesetup.exe```](https://go.microsoft.com/fwlink/?linkid=2120253)

Rename the filename ```adksetup.exe``` to ```adksetup_2004.exe``` or similar to help differentiate different versions.

Rename the filename ```adkwinpesetup.exe``` to ```adkwinpesetup_2004.exe``` or similar to help differentiate different versions.

This 2004 ADK supports Windows 10, version 2004, and later versions of Windows 10.

This version of the ADK and ADK WinPE Add-on have been republished in May 2025 to fix a security vulnerability. 

Windows PE, Setup and main installable Windows needs the Servicing Stack Update (SSU) in [KB5026361](https://support.microsoft.com/en-gb/topic/may-9-2023-kb5026361-os-builds-19042-2965-19044-2965-and-19045-2965-3edafffe-c3cc-4010-af43-2097c84c9437).

The steps below in [section 4](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#4-check-what-features-packages--kbs-are-installed-optional) will show you how to check what updates are installed so you can see if KB5026361 is installed.

[For offline OS image servicing:](https://support.microsoft.com/topic/may-9-2023-kb5026361-os-builds-19042-2965-19044-2965-and-19045-2965-3edafffe-c3cc-4010-af43-2097c84c9437)

If your image does not have the March 22, 2022 ([KB5011543](https://support.microsoft.com/en-gb/topic/march-22-2022-kb5011543-os-builds-19042-1620-19043-1620-and-19044-1620-preview-4fe2d1c0-720f-47fe-9523-75339bc107a1)) or later Cumulative Update (CU), you must install the 
special standalone May 10, 2022 SSU ([KB5014032](https://support.microsoft.com/en-gb/topic/kb5014032-servicing-stack-update-for-windows-10-version-20h2-21h1-and-21h2-may-10-2022-69a798ad-813d-4d62-bb54-2252bbb434a1)) before installing this update.


### 4. Check what Features, Packages, & KB's are installed (Optional)
 
### 4.1 To check the ```boot.wim``` which contains the ```WindowsPE``` image used in the ```WindowsPE``` configuration pass.

```
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount_Win10_22H2_x32_ISO\sources\boot.wim" /index:1 | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.ImageInfo.default.txt"
PS C:\WINDOWS\system32> md -path "C:\mount_Win10_22H2_x32_Boot_PE_WIM"
PS C:\WINDOWS\system32> Dism /mount-image /imagefile:"C:\mount_Win10_22H2_x32_ISO\sources\install.wim" /index:1 /mountdir:"C:\mount_Win10_22H2_x32_Boot_PE_WIM" /readonly
```

If you dont want to use the ```/ReadOnly``` attribute but make changes to a ```WIM``` file, use the line below to change the file attribute.

```
PS C:\WINDOWS\system32> Set-ItemProperty "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim" -name IsReadOnly -value $false
```

```
PS C:\WINDOWS\system32> Get-WindowsPackage -Path "C:\mount_Win10_22H2_x32_Boot_PE_WIM" | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.PE.Get-WindowsPackage.default.txt"
[As Above] PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_22H2_x32_Boot_PE_WIM" /Get-Packages | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.PE.Get-Packages.default.txt"
 

PS C:\WINDOWS\system32> Get-WindowsPackage -Path "C:\mount_Win10_22H2_x32_Boot_PE_WIM" | Where-Object {$_.PackageName -match "KB"} | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.PE.PackageKB.default.txt"
PS C:\WINDOWS\system32> Get-WindowsPackage -Path "C:\mount_Win10_22H2_x32_Boot_PE_WIM" | Where-Object {$_.PackageName -match "ServicingStack"} | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.PE.PackageSSU.default.txt"
[Where-Object does not work directly with DISM, only Cmdlet objects]

PS C:\WINDOWS\system32> Get-WindowsOptionalFeature -Path "C:\mount_Win10_22H2_x32_Boot_PE_WIM" | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.PE.Get-WindowsOptionalFeature.default.txt"
[As Above] PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_22H2_x32_Boot_PE_WIM" /Get-Features | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.PE.Get-Features.default.txt"
```

```
PackageName  : Package_for_KB5015684~31bf3856ad364e35~x86~~19041.1799.1.2
PackageState : Installed
ReleaseType  : Update
InstallTime  : 04/12/2023 03:23:00

PackageName  : Package_for_ServicingStack_1704~31bf3856ad364e35~x86~~19041.1704.1.4
PackageState : Installed
ReleaseType  : SecurityUpdate
InstallTime  : 04/12/2023 02:25:00

PackageName  : Package_for_ServicingStack_3745~31bf3856ad364e35~x86~~19041.3745.1.0
PackageState : Installed
ReleaseType  : Update
InstallTime  : 04/12/2023 02:25:00
```

```
[Optional] PS C:\WINDOWS\system32> Dism /get-mountedwiminfo
[Optional] PS C:\WINDOWS\system32> Dism /remount-image /MountDir:"C:\mount_Win10_22H2_x32_Boot_PE_WIM"
PS C:\WINDOWS\system32> Dism /unmount-image /mountdir:"C:\mount_Win10_22H2_x32_Boot_PE_WIM" /discard 
```

Even though this was loaded with ```/ReadOnly```, ```/unmount-image``` has to have either ```/discard``` or ```/commit```, it cant be missed off so using ```/discard```

### 4.2 To check the ```boot.wim``` which contains the ```Windows Setup``` image used in the ```WindowsPE``` configuration pass.

```
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount_Win10_22H2_x32_ISO\sources\boot.wim" /index:2 | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.ImageInfo.default.txt"
PS C:\WINDOWS\system32> md -path "C:\mount_Win10_22H2_x32_Boot_Setup_WIM"
PS C:\WINDOWS\system32> Dism /mount-image /imagefile:"C:\mount_Win10_22H2_x32_ISO\sources\boot.wim" /index:2 /mountdir:"C:\mount_Win10_22H2_x32_Boot_Setup_WIM" /readonly
```

```
PS C:\WINDOWS\system32> Get-WindowsPackage -Path "C:\mount_Win10_22H2_x32_Boot_Setup_WIM" | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.Setup.Packages.default.txt"
PS C:\WINDOWS\system32> Get-WindowsPackage -Path "C:\mount_Win10_22H2_x32_Boot_Setup_WIM" | Where-Object {$_.PackageName -match "KB"} | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.Setup.PackageKB.default.txt"
PS C:\WINDOWS\system32> Get-WindowsPackage -Path "C:\mount_Win10_22H2_x32_Boot_Setup_WIM" | Where-Object {$_.PackageName -match "ServicingStack"} | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.Setup.PackageSSU.default.txt"
PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_22H2_x32_Boot_Setup_WIM" /Get-Features | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.Setup.Features.default.txt"
```

```
PackageName  : Package_for_KB5015684~31bf3856ad364e35~x86~~19041.1799.1.2
PackageState : Installed
ReleaseType  : Update
InstallTime  : 04/12/2023 03:30:00

PackageName  : Package_for_ServicingStack_1704~31bf3856ad364e35~x86~~19041.1704.1.4
PackageState : Installed
ReleaseType  : SecurityUpdate
InstallTime  : 04/12/2023 03:30:00

PackageName  : Package_for_ServicingStack_3745~31bf3856ad364e35~x86~~19041.3745.1.0
PackageState : Installed
ReleaseType  : Update
InstallTime  : 04/12/2023 03:30:00
```

```
[Optional] PS C:\WINDOWS\system32> Dism /get-mountedwiminfo
[Optional] PS C:\WINDOWS\system32> Dism /remount-image /MountDir:"C:\mount_Win10_22H2_x32_Boot_Setup_WIM"
PS C:\WINDOWS\system32> Dism /unmount-image /mountdir:"C:\mount_Win10_22H2_x32_Boot_Setup_WIM" /discard
```

### 4.3 To check the ```install.wim``` which contains the main Windows image used to install Windows onto a computer.

```
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount_Win10_22H2_x32_ISO\sources\install.wim" /index:1 | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\install.wim.Pro.N.ImageInfo.default.txt"
PS C:\WINDOWS\system32> md -path "C:\mount_Win10_22H2_x32_Install_Pro_N_WIM"
PS C:\WINDOWS\system32> Dism /mount-image /imagefile:"C:\mount_Win10_22H2_x32_ISO\sources\install.wim" /index:1 /mountdir:"C:\mount_Win10_22H2_x32_Install_Pro_N_WIM"
```

```
PS C:\WINDOWS\system32> Get-WindowsPackage -Path "C:\mount_Win10_22H2_x32_Install_Pro_N_WIM" | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\install.wim.Pro.N.Packages.default.txt"
PS C:\WINDOWS\system32> Get-WindowsPackage -Path "C:\mount_Win10_22H2_x32_Install_Pro_N_WIM" | Where-Object {$_.PackageName -match "KB"} | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\install.wim.Pro.N.PackageKB.default.txt"
PS C:\WINDOWS\system32> Get-WindowsPackage -Path "C:\mount_Win10_22H2_x32_Install_Pro_N_WIM" | Where-Object {$_.PackageName -match "ServicingStack"} | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\install.wim.Pro.N.PackageSSU.default.txt"
PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_22H2_x32_Install_Pro_N_WIM" /Get-Features | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\install.wim.Pro.N.Features.default.txt"
```

```
PackageName  : Package_for_KB5015684~31bf3856ad364e35~x86~~19041.1799.1.2
PackageState : Installed
ReleaseType  : Update
InstallTime  : 04/12/2023 02:25:00

PackageName  : Package_for_ServicingStack_1704~31bf3856ad364e35~x86~~19041.1704.1.4
PackageState : Installed
ReleaseType  : SecurityUpdate
InstallTime  : 04/12/2023 02:25:00

PackageName  : Package_for_ServicingStack_3745~31bf3856ad364e35~x86~~19041.3745.1.0
PackageState : Installed
ReleaseType  : Update
InstallTime  : 04/12/2023 02:25:00
```

```
[Optional] PS C:\WINDOWS\system32> Dism /get-mountedwiminfo
[Optional] PS C:\WINDOWS\system32> Dism /remount-image /MountDir:"C:\mount_Win10_22H2_x32_Install_Pro_N_WIM"
PS C:\WINDOWS\system32> Dism /unmount-image /mountdir:"C:\mount_Win10_22H2_x32_Install_Pro_N_WIM" /discard
```
### 4.4 Checking for installed Updates Summary

A [(Latest) Cumulative Update (L(CU))|Servicing Stack Update (SSU)|Feature Update (FU)|Quality Update (QU)|Driver Update (DU)] which is a KB number greater than another KB Update number of the same Type will replace the lower KB numbered Update, unless a note specifically states a previous KB Update of the same Type needs to be installed first.

You have to walk the chain of latest updates backwards until you reach your current version of windows or the required KB's that need to be installed.

Windows can be viewed:

Windows 10 Common Core + Foundation Package (Version Features to make versions like Home, Education, Pro, Pro N and others like Server versions) + Enablement Package (22H2) (updates to features & new/deprecated features) + Various Types of Updates.
 
[KB5015684](https://support.microsoft.com/en-gb/topic/kb5015684-featured-update-to-windows-10-version-22h2-by-using-an-enablement-package-09d43632-f438-47b5-985e-d6fd704eee61) is an Enablement Update, which has been applied to Windows (Windows 10 Common Core + Foundation Package) and then the boot.wim WindowsPE & WindowsSetup images, along with the install.wim Windows 10 Pro N image we have previously made.
The later and greater in number value CU KB5015684 would suggest the SSU has been installed at the time of writing. The SSU that is needed for the ADK is the May 10, 2022 SSU ([KB5014032](https://support.microsoft.com/en-gb/topic/kb5014032-servicing-stack-update-for-windows-10-version-20h2-21h1-and-21h2-may-10-2022-69a798ad-813d-4d62-bb54-2252bbb434a1)). 

The SSU KB5014032 can be downloaded from the Microsoft Update Catalog by clicking [here](https://www.catalog.update.microsoft.com/Search.aspx?q=windows%2010%20x86%20KB5014032).

There are three to choose from. 

[Link 1 2022-05 Servicing Stack Update for Windows 10 Version 21H2 for x86-based Systems (KB5014032) - Windows 10, version 1903 and later, Windows 10 LTSB](https://www.catalog.update.microsoft.com/ScopedViewInline.aspx?updateid=841c7193-96db-43b9-8e89-eebd1d1c3a04)

[Link 2 2022-05 Servicing Stack Update for Windows 10 Version 21H1 for x86-based Systems (KB5014032) - Windows 10, version 1903 and later](https://www.catalog.update.microsoft.com/ScopedViewInline.aspx?updateid=35bd64e9-aa66-43c3-80ea-a00184937ba3)

[Link 3 2022-05 Servicing Stack Update for Windows 10 Version 20H2 for x86-based Systems (KB5014032) - Windows 10, version 1903 and later](https://www.catalog.update.microsoft.com/ScopedViewInline.aspx?updateid=2df37f22-5902-43ec-ae92-d50574470916)

If you click Link 1, then select the Package Details tab, you can see this update file has been replaced by ```2023-10 Servicing Stack Update for Windows 10 Version 21H2 for x86-based Systems (KB5031539)```
Link 2 has N/A, and Link 3 replaces a list of other KB updates.

We can use any of the 3 SSU updates to apply the update if we needed to, but for the purpose of the rest of this section, we will download [Link 2](https://catalog.s.download.windowsupdate.com/c/msdownload/update/software/secu/2022/05/ssu-19041.1704-x86_3cec66c3891a613e6656f141547e573f9d700d35.msu). 

We can see the Link 3 ```MSU``` file is called ```ssu-19041.1704-x86_3cec66c3891a613e6656f141547e573f9d700d35.msu``` and we can see ```ssu-19041.1704``` listed in the ```Package Details Package_for_ServicingStack_1704~31bf3856ad364e35~x86~~19041.1704.1.4``` shown above in each ```WIM``` file. So we can conclude the SSU required is already installed and was included in the Enablement Update which is no longer available for download from the Microsoft Update Catalog. 

We cant download the Enablement Update KB5015684 from the Microsoft Update Catalog, only from the Windows Update service, but if we download the very latest Cumulative Update by searching for [Windows 10 x86 22H2 Cumulative Update 2025-06](https://www.catalog.update.microsoft.com/Search.aspx?q=Windows%2010%20x86%2022h2%20Cumulative%20Update%202025-06) and then selecting Link 2 to [download the LCU](https://catalog.s.download.windowsupdate.com/c/msdownload/update/software/updt/2025/06/windows10.0-kb5063159-x86_7d10fe75ab78f32f2f68ce8f990e23ef6cf8cf35.msu), we can then expand the ```MSU``` to see what exactly is included in the update file.

```
PS C:\WINDOWS\system32> md -path "C:\mount_windows10.0-kb5063159-x86"
PS C:\WINDOWS\system32> Expand -F:* "C:\Users\Admin1\Documents\ISO Files\windows10.0-kb5063159-x86_7d10fe75ab78f32f2f68ce8f990e23ef6cf8cf35.msu" "C:\mount_windows10.0-kb5063159-x86"
Microsoft (R) File Expansion Utility
Copyright (c) Microsoft Corporation. All rights reserved.

Adding C:\mount_windows10.0-kb5063159-x86\Windows10.0-KB5063159-x86-pkgProperties.txt to Extraction Queue
Adding C:\mount_windows10.0-kb5063159-x86\Windows10.0-KB5063159-x86.cab to Extraction Queue
Adding C:\mount_windows10.0-kb5063159-x86\Windows10.0-KB5063159-x86_uup.xml to Extraction Queue
Adding C:\mount_windows10.0-kb5063159-x86\WSUSSCAN.cab to Extraction Queue
Adding C:\mount_windows10.0-kb5063159-x86\SSU-19041.5967-x86.cab to Extraction Queue

Expanding Files ....
Progress: 1 out of 5 files
Expanding Files Complete ...
5 files total.
PS C:\WINDOWS\system32>
```

Inside this expanded ```MSU``` file, ```CAB``` files can also be expanded, we can see ```SSU-19041.5967-x86.cab```. Here we can see a similarly numbered SSU which can be installed. We would need to investigate if the SSU seen in ```SSU-19041.5967-x86.cab``` can replace the one referred to in ```2023-10 Servicing Stack Update for Windows 10 Version 21H2 for x86-based Systems (KB5031539)``` or not.  

Image files (```.ISO```, ```.vhd```, ```boot.wim```, ```install.[wim|esd]```) can contain other files including other image files and ```MSU``` and ```CAB``` files, which they can contain other files like other ```MSU``` and ```CAB``` files, like [Russian Matryoshka Dolls](https://en.wikipedia.org/wiki/Matryoshka_doll). The Archive utility app [7Zip](https://www.7-zip.org/) can open the ```MSU``` and ```CAB``` just like Expand utility seen above can.



 


### 5. Create an AutoUnattend.xml Answer File to initially setup Windows.

This AutoUnattend answer file needs to setup Windows 10 Pro N.
 
The [Configuration Pass Order](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/how-configuration-passes-work?view=windows-11#understanding-configuration-passes).

[Configuration Pass - Windows PE](AutoUnattend-ConfigurationPass-windowsPE.md)

[Component - Microsoft-Windows-International-Core-WinPE (Language, Locale settings)](AutoUnattend-ConfigurationPass-windowsPE-Component-Microsoft-Windows-International-Core-WinPE.md)

[Component - Microsoft-Windows-Setup (Hard Disk configuration, diagnostics, Update settings)](AutoUnattend-ConfigurationPass-windowsPE-Component-Microsoft-Windows-Setup-WindowsPE.md)

[Configuration Pass - Offline Servicing](AutoUnattend-ConfigurationPass-offlineServicing.md)

[Component - Microsoft-Windows-Shell-Setup (Computer Name, Bluetooth Taskbar Icon, OEM Info - SupportProvider)](AutoUnattend-ConfigurationPass-offlineServicing-Component-Microsoft-Windows-Shell-Setup.md)

[Configuration Pass - generalize](AutoUnattend-ConfigurationPass-generalize.md)

[Component - Microsoft-Windows-Shell-Setup (OEM Info - SupportURL)](AutoUnattend-ConfigurationPass-generalize-Component-Microsoft-Windows-Shell-Setup.md)

[Configuration Pass - specialize](AutoUnattend-ConfigurationPass-specialize.md)

[Component - Microsoft-Windows-Shell-Setup (OEM Info - TradeInURL)](AutoUnattend-ConfigurationPass-specialize-Component-Microsoft-Windows-Shell-Setup.md)

[Configuration Pass - auditSystem](AutoUnattend-ConfigurationPass-auditSystem.md)

[Component - Microsoft-Windows-Shell-Setup (OEM Info - RecycleURL)](AutoUnattend-ConfigurationPass-auditSystem-Component-Microsoft-Windows-Shell-Setup.md)

[Configuration Pass - auditUser](AutoUnattend-ConfigurationPass-auditUser.md)

[Component - Microsoft-Windows-Shell-Setup (RegisteredOrganization)](AutoUnattend-ConfigurationPass-auditUser-Component-Microsoft-Windows-Shell-Setup.md)

[Configuration Pass - oobeSystem](AutoUnattend-ConfigurationPass-oobeSystem.md)

[Component - Microsoft-Windows-Shell-Setup (Hide various OOBE screens, Setup Themes, Local Accounts)](AutoUnattend-ConfigurationPass-oobeSystem-Component-Microsoft-Windows-Shell-Setup.md)

[Component - Microsoft-Windows-Sensors-Core (Screen Dimming)](AutoUnattend-ConfigurationPass-oobeSystem-Component-Microsoft-Windows-Sensors-Core.md)


### 6. Install Windows Using AutoUnattend.xml File


Create a VMware Workstation virtual PC.
 


https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/wsim/best-practices-for-authoring-answer-files



[Configuration Pass Order](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/how-configuration-passes-work?view=windows-11#understanding-configuration-passes)

[Windows PE](AutoUnattend-AnswerFile-VMware-WindowsPE.md)
https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windowspe?view=windows-11
Windows PE Settings
Windows Setup Settings

40GB size

[Offline Servicing](AutoUnattend-AnswerFile-VMware-OfflineServicing.md)

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/update-windows-settings-and-scripts-create-your-own-answer-file-sxs?view=windows-11


https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/offlineservicing?view=windows-11
Update Windows faster by using DISM to make your changes without ever booting Windows. 
Mount an image to a temporary location, install apps, drivers, languages, and more, and then commit the changes so they can be applied to new devices. 
DISM requires an elevated command-line or from PowerShell, which makes it easier to automate your changes with scripts.

If you're updating device drivers using an unattended answer file, you must apply the answer file to an offline image and specify the settings in the offlineServicing configuration pass.

If you are updating packages or other settings using an unattended answer file, you can apply the answer file to an offline or online image. 
Specify the settings in the offlineServicing configuration pass.

Dism /image:C:\test\offline /Apply-Unattend:C:\test\answerfiles\myunattend.xml
Dism /online /Apply-Unattend:C:\test\answerfiles\myunattend.xml


[generalize](AutoUnattend-AnswerFile-VMware-generalize.md)

[specialize](AutoUnattend-AnswerFile-VMware-specialize.md)

[auditSystem](AutoUnattend-AnswerFile-VMware-auditSystem.md)

[auditUser](AutoUnattend-AnswerFile-VMware-auditUser.md)

[oobeSystem](AutoUnattend-AnswerFile-VMware-oobeSystem.md)



 
