# VMware Windows 10 Pro

## Create Master copy

[1. Download Windows 10 and copy the ISO to a folder.](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#1-download-windows-10-and-copy-the-iso-to-a-folder)

[2. Export the required version of Windows from the ```\sources\install.esd``` to a ```\sources\install.wim``` file.](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#2-export-the-required-version-of-windows-from-the-sourcesinstallesd-to-a-sourcesinstallwim-file)

[3. Download the Deployment Tools from the Windows ADK.](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#3-download-the-deployment-tools-from-the-windows-adk)

[4. Check what Features, Packages, & KB's are installed (Optional)](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#4-check-what-features-packages--kbs-are-installed-optional)

4. Create an Answer File to initially setup windows.
5. Download Windows updates using the update services.
6. Install required End User Apps.
7. Finish setting up Windows and tweaking it to suit.
8. Generalise the Image.
9. Capture the Image.
10. Keep this as a master copy of the Vmware.



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

This 2004 ADK supports Windows 10, version 2004, and later versions of Windows 10

This version of the ADK and ADK WinPE Add-on have been republished in May 2025 to fix a security vulnerability. 

Windows PE, Setup and main installable Windows needs the Servicing Stack Update (SSU) in [KB5026361](https://support.microsoft.com/en-gb/topic/may-9-2023-kb5026361-os-builds-19042-2965-19044-2965-and-19045-2965-3edafffe-c3cc-4010-af43-2097c84c9437).

The steps below in [section 4](AutoUnattend-InstallWindows-VMWare-Workstation-Clone.md#4-check-what-features-packages--kbs-are-installed-optional) will show you how to check what updates are installed so you can see if KB5026361 is installed.

[For offline OS image servicing:](https://support.microsoft.com/topic/may-9-2023-kb5026361-os-builds-19042-2965-19044-2965-and-19045-2965-3edafffe-c3cc-4010-af43-2097c84c9437)

If your image does not have the March 22, 2022 ([KB5011543](https://support.microsoft.com/en-gb/topic/march-22-2022-kb5011543-os-builds-19042-1620-19043-1620-and-19044-1620-preview-4fe2d1c0-720f-47fe-9523-75339bc107a1)) or later Cumulative Update (CU), you must install the 
special standalone May 10, 2022 SSU ([KB5014032](https://support.microsoft.com/en-gb/topic/kb5014032-servicing-stack-update-for-windows-10-version-20h2-21h1-and-21h2-may-10-2022-69a798ad-813d-4d62-bb54-2252bbb434a1)) before installing this update.


### 4. Check what Features, Packages, & KB's are installed (Optional)
 
To check the ```boot.wim``` which contains the ```WindowsPE``` image used in the ```WindowsPE``` configuration pass.
```
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount_Win10_22H2_x32_ISO\sources\boot.wim" | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.ImageInfo.default.txt"
PS C:\WINDOWS\system32> md -path "C:\mount_Win10_22H2_x32_Boot_PE_WIM"
PS C:\WINDOWS\system32> Dism /mount-image /imagefile:"C:\mount_Win10_22H2_x32_ISO\sources\install.wim" /index:1 /mountdir:"C:\mount_Win10_22H2_x32_Boot_PE_WIM" /readonly
```

If you dont want to use the ```/ReadOnly``` attribute but make changes to a ```WIM``` file, use the line below to change the file attribute.

```
PS C:\WINDOWS\system32> Set-ItemProperty "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim" -name IsReadOnly -value $false
```

```
PS C:\WINDOWS\system32> Get-WindowsPackage -Path "C:\mount_Win10_22H2_x32_Boot_PE_WIM" | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.PE.Packages.default.txt"
PS C:\WINDOWS\system32> Get-WindowsPackage -Path "C:\mount_Win10_22H2_x32_Boot_PE_WIM" | Where-Object {$_.PackageName -match "KB"} | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.PE.PackageKB.default.txt"
```

```
PackageName  : Package_for_KB5015684~31bf3856ad364e35~x86~~19041.1799.1.2
PackageState : Installed
ReleaseType  : Update
InstallTime  : 04/12/2023 03:23:00
```

```
[Optional] PS C:\WINDOWS\system32> Dism /get-mountedwiminfo
[Optional] PS C:\WINDOWS\system32> Dism /remount-image /MountDir:"C:\mount_Win10_22H2_x32_Boot_PE_WIM"
PS C:\WINDOWS\system32> Dism /unmount-image /mountdir:"C:\mount_Win10_22H2_x32_Boot_PE_WIM" /discard # 
```

Even though this was loaded with ```/ReadOnly```, ```/unmount-image``` has to have either ```/discard``` or ```/commit```, it cant be missed off so using ```/discard```


We can see KB5015684 is installed in the Windows 10 22H2 x32 Boot PE WIM.
KB5015684 is https://support.microsoft.com/en-gb/topic/kb5015684-featured-update-to-windows-10-version-22h2-by-using-an-enablement-package-09d43632-f438-47b5-985e-d6fd704eee61

So check the boot.wim which contains the Windows Setup.

```
PS C:\WINDOWS\system32> md -path "C:\mount_Win10_22H2_x32_Boot_Setup_WIM"
[Optional] PS C:\WINDOWS\system32> Dism /get-mountedwiminfo
[Optional] PS C:\WINDOWS\system32> Dism /remount-image /MountDir:"C:\mount_Win10_22H2_x32_Boot_Setup_WIM"
PS C:\WINDOWS\system32> Dism /mount-image /imagefile:"C:\mount_Win10_22H2_x32_ISO\sources\boot.wim" /index:2 /mountdir:"C:\mount_Win10_22H2_x32_Boot_Setup_WIM" /readonly
```

If you dont want to use the /ReadOnly attribute but make changes, use the line below.
```
PS C:\WINDOWS\system32> Set-ItemProperty "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim" -name IsReadOnly -value $false
```

```
PS C:\WINDOWS\system32> Get-WindowsPackage -Path "C:\mount_Win10_22H2_x32_Boot_Setup_WIM" | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.Setup.Packages.default.txt"
PS C:\WINDOWS\system32> Get-WindowsPackage -Path "C:\mount_Win10_22H2_x32_Boot_Setup_WIM" | Where-Object {$_.PackageName -match "KB"} | Out-File -FilePath "C:\mount_Win10_22H2_x32_ISO\sources\boot.wim.Setup.PackageKB.default.txt"
```

PackageName  : Package_for_KB5015684~31bf3856ad364e35~x86~~19041.1799.1.2
PackageState : Installed
ReleaseType  : Update
InstallTime  : 04/12/2023 03:30:00

```
PS C:\WINDOWS\system32> dism /remount-image /MountDir:"C:\mount_Win10_22H2_x32_Boot_Setup_WIM"
PS C:\WINDOWS\system32> Dism /get-mountedwiminfo
PS C:\WINDOWS\system32> Dism /unmount-image /mountdir:"C:\mount_Win10_22H2_x32_Boot_Setup_WIM" /discard # Even though this was loaded with /ReadOnly, /unmount-image has to have either /discard or /commit so using /discard
```

So we can see that KB5015684 has been applied to the boot.wim PE & Setup images, along with the install.wim image we have previously made.
The later and greater CU KB5015684 would suggest the SSU has been installed at the time of writing.

4.
This AutoUnattend answer file needs to setup Windows 10. 
Parts of it will be reused in other answer files. 
Settings used in Windows 11 will be ignored.

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



 
