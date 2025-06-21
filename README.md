# Automatic Windows Installation 
# AutoUnattend.xml Answer File

[AutoUnattend.xml aka Unattend.xml](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/update-windows-settings-and-scripts-create-your-own-answer-file-sxs?view=windows-11) is an answer file that can control and configure the installation of [Windows 10 (22H2)](https://www.microsoft.com/en-gb/software-download/windows10) ([version 1607](https://en.wikipedia.org/wiki/Windows_10,_version_1607) and onwards) or Windows Server [2016](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016), [2019](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019), [2022](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022) and [Windows 11](https://www.microsoft.com/en-gb/software-download/windows11) or Windows Server [2025](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025), along with user software in an expedient, consistent and hands free way on a computer, to make the computer yours.


The AutoUnattend.xml file can be used with the Windows 10 and Windows 11 Media Creation Tool (MCT) by adding the file to the USB memory stick or the ISO aka DVD (image) file created by the MCT, or by adding it to the Windows Server ISO files and VHD/VHDX (Virtual Hard Disk) and FFU (Full FlashUpdate) files.

Device Drivers pertinent for the computer and/or device(s) like [PowerEdge RAID Controllers](https://www.dell.com/support/kbdoc/en-uk/000131648/list-of-poweredge-raid-controller-perc-types-for-dell-emc-systems), [network printer's, network scanner's](https://www.hp.com/gb-en/shop/list.aspx?fc_conn_ethernet=1&sel=prn) or [camera's](https://www.axis.com/en-gb) can be scripted to install and configure using the answer file or added to the [Windows Driver Store](https://learn.microsoft.com/en-us/windows-hardware/drivers/install/driver-store) of [DISM](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/what-is-dism) mounted Windows installation [image files](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/wsim/windows-image-files-and-catalog-files-overview),  ```install.wim``` or ```install.esd```, and/or the cut down version of Windows ```boot.wim``` used to setup & install versions of Windows.

3rd Party User software like [Softvelocity's Clarion](https://www.softvelocity.com/), [Visual Studio](https://visualstudio.microsoft.com/), [Notepad++](https://notepad-plus-plus.org/) and more, can be installed and configured after windows has been installed, provided the 3rd party user installation software allows command line switches (flags).

Windows can then be configured to work just the way you like, using a variety of methods like ```.BAT``` (Batch), ```.CMD``` (Command), ```.PS1``` ([Powershell](https://learn.microsoft.com/en-us/powershell/)) script files, to alter the [Windows Registry](https://learn.microsoft.com/en-us/windows/win32/sysinfo/registry) or [Windows Group Policy](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-policy/group-policy-overview) settings (where applicable).



# TLDR

# Less is More 

Less or essential only programs & packages installed to a Windows Image ```install.wim``` or ```install.esd``` file and/or a Windows installation means a reduced [Attack Vector](https://en.wikipedia.org/wiki/Attack_vector) which helps improve [Computer Security](https://en.wikipedia.org/wiki/Computer_security) and helps keep the computer responsive. 

Likewise a [ laptop \| desktop ] computer hard disk configured for forensic data recovery also helps for those times when spooky hackers strike, and you dont have adequate backups or a hard disk [RAID setup](https://en.wikipedia.org/wiki/RAID#Overview).

# Basic Layout of the AutoUnattend.xml file

[Configuration Pass](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-configuration-passes?view=windows-11) sections contain zero, one or more [Component](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/components-b-unattend) sections.

[Configuration Passes run in a predefined order, some are mandatory, some are optional.](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/how-configuration-passes-work#understanding-configuration-passes)

The Configuration Passes, in order, are:
[windowsPE](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windowspe)
[offlineServicing](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/offlineservicing)
[generalize](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/generalize)
[specialize](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/specialize)
[auditSystem](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/auditsystem)
[auditUser](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/audituser)
[oobeSystem](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/oobesystem)


[Components can be added to one or more Configuration Passes to control and configure the installation process.](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/components-b-unattend)

The Microsoft webpage detailing the Component will list what Configuration Passes it can be added to and what version of Windows it works with. Look for [Valid Configuration Passes](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-display#valid-configuration-passes) or if using the [Windows System Image Manager](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/wsim/windows-system-image-manager-technical-reference), click on a component in the bottom left Windows Image pane and see the ApplicableConfigurationPasses in the top right pane that are valid.

A Component could contain multiple [Child Elements](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-display#child-elements) and when required, because some could be optional child elements, would be listed in the XML file as seen [here](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-display#xml-example) or seen below, demonstrating the use of 

```<Settings pass="[Pass Name]"></Settings>``` and ```<Component name="[Component Name]"></Component>```.

As at 20250613:YYYYMMDD, also note ```DPI``` is not listed as a child element, but is shown in the 2nd XML example on the Microsoft [webpage](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-display#xml-example). Not the only child element which isnt always linkedin either.

```
<Settings pass="windowsPE">
	<Component name="Display">
		<ColorDepth>32</ColorDepth> !
		<DPI>120</DPI>
		<HorizontalResolution>1024</HorizontalResolution>
		<RefreshRate>72</RefreshRate>
		<VerticalResolution>768</VerticalResolution>
	</Component>
</Settings>
```

Multiple Components of the same name can exist within a Configuration Pass because it can contain additional information to further restrict the use of the Component to specific situations which can be detected and/or [specified at the time the AutoUnAttend.xml](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-automation-overview) is used.

These options include: processorArchitecture, publicKeyToken, language, and versionScope.
```
processorArchitecture="x86" represents 32bit CPU's made by Intel and AMD.
processorArchitecture="amd64" represents 64bit CPU's made by Intel and AMD.
processorArchitecture="arm" represents 32bit Arm CPU's made under licence from Arm.
processorArchitecture="arm64" represents 64bit Arm CPU's made under licence from Arm.
publicKeyToken="31bf3856ad364e35" represents the "token" for a public key used to sign a dll or .net assembly by Microsoft to help mitigate dll/assembly hijacking during the Windows installation process.
language="neutral" indicates that the UI (User Interface) language should be determined by the system's default language settings specified in the Component "Microsoft-Windows-International-Core-WinPE"
versionScope="nonSxS" refers to "Non-SxS" aka Non Side-by-Side dll's or assemblies, which is another attempt to mitigate the effects of dll/assembly hijacking during the Windows installation process.
```
	
"31bf3856ad364e35" is a 16-character hexadecimal from the last 8 bytes of the SHA-1 hash of the PublicKeyToken string used by Microsoft to sign dll's and assemblies in the .NET Framework. It's similiar to the dwflag ```LOAD_LIBRARY_REQUIRE_SIGNED_TARGET``` used with the [LoadLibraryEx Windows API (Application Programmer Interface)](https://learn.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-loadlibraryexa) in an attempt to mitigate the hijacking of dll's and assemblies as described [here](https://learn.microsoft.com/en-us/windows/win32/dlls/dynamic-link-library-security). More information on publicKeyToken can be found [here](https://learn.microsoft.com/en-us/windows/win32/sbscs/application-manifests#assemblyIdentity).

"nonSxS" aka Non Side-by-Side dll's or assemblies, is a switch to force dll's or .Net (DotNet) assemblies to be loaded in an exclusive manner similar to those described in the [remarks section concerning the Search Path](https://learn.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-loadlibraryexa#searching-for-dlls-and-dependencies) used with the [LoadLibraryEx Windows API (Application Programmer Interface)](https://learn.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-loadlibraryexa) in an attempt to mitigate the hijacking of dll's and assemblies as described [here](https://learn.microsoft.com/en-us/windows/win32/dlls/dynamic-link-library-security). After Windows installation, SXS dll's can be found in the ```%systemroot%\WinSxS``` folder. An application's Manifest can control the use of SXS dll's and assemblies. More information [here](https://learn.microsoft.com/en-us/windows/win32/sbscs/application-manifests#assemblyIdentity)


# Creating the AutoUnAttend.xml file

There are a few ways to create/modify the AutoUnAttend.xml file, [an online generator can be found at schneegans.de](https://schneegans.de/windows/unattend-generator/), Microsoft provides the [Windows System Image Manager (Windows SIM)](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/wsim/windows-system-image-manager-technical-reference) found in the [Windows Assessment and Deployment Kit (Windows ADK)](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install), or modifying [various AutoUnAttend.xml files on Github](https://github.com/search?q=autounattend.xml&type=repositories) and elsewhere which can be modified using a [text editor](https://notepad-plus-plus.org/) or [XML editor](https://microsoft.github.io/XmlNotepad/).  

Explanations and examples of the Configuration Passes and Components are explored in greater detail below and on other pages in this repo.




# Creating a Windows installer

There are a few different ways to install Windows. 

The finished AutoUnattend.xml file is added to the root folder of the [Media Creation Tool (MCT)](https://support.microsoft.com/en-gb/windows/create-installation-media-for-windows-99a58364-8c02-206f-aa6f-40c3b507420d) created USB memory stick or ISO image file, or Windows Server ISO, before burning to DVD where applicable.

Most members of the public and organisations will use the [Windows 10 (22H2)](https://www.microsoft.com/en-gb/software-download/windows10) or [Windows 11](https://www.microsoft.com/en-gb/software-download/windows11) MCT to download Windows (Home, Education, & Pro) variants onto a USB memory stick to boot from and install Windows, or use MCT to create an ISO image file which can be used with [virtualisation software](https://en.wikipedia.org/wiki/Virtual_machine) like [VMware Workstation](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion) or to burn a DVD capable of installing Windows. [Windows Server 2016 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016), [Windows Server 2019 ISO/VHD](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019), [Windows Server 2022 ISO/VHD](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022) or [Windows Server 2025 ISO/VHD](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025) only exist in ISO & VHD forms and are not included as an option in the MCT. The Windows Server versions installs Data Centre, Core & NanoSever variants.

Other methods exist for installing Windows 10/11/Server which are more technical and/or have specialist reasons for existing.

[The Windows Image (.WIM/.ESD) vs Virtual Hard Disk (.VHD/.VHDX) vs Full Flash Update (.FFU) pro's and con's can be seen here.](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/wim-vs-ffu-image-file-formats)

Image files (```.ISO```, ```.vhd```, ```boot.wim```, ```install.[wim|esd]```) can contain other image files, like [Russian Matryoshka Dolls](https://en.wikipedia.org/wiki/Matryoshka_doll).


# ```boot.wim```, ```install.wim``` and ```install.esd```


The ```boot.wim```, ```install.esd``` or ```install.wim``` are contained in ```[USB stick|mounted ISO image file] Drive Letter:\sources```, or ```[USB stick|mounted ISO image file] Drive Letter:\x86\sources``` & ```[USB stick|mounted ISO image file] Drive Letter:\x64\sources``` when both 32bit (x86) and 64bit (x64) are selected for optional installation using the MCT.


```.WIM``` files only [capture/contain a single partition, typically the Windows partition](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/capture-and-apply-windows-using-a-single-wim), whereby other partitions are then configured and setup with data, like the Recovery partition containing the [Windows RE (Recovery Environment)](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference?view=windows-11). Full Flash Update aka ```FFU``` [captures the entire hard disk and all partitions on the hard disk](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/capture-and-apply-windows-system-and-recovery-partitions). 

```boot.wim``` is a special cut down version of Windows called [Windows PE](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-intro) for installing/repairing a version of Windows onto a computer. [App|Driver] Packages can be added to activate specialist hardware like RAID controllers or additional networking functionality in order to install a Windows edition onto the computer.

```install.wim``` & ```install.esd``` are essentially the same ```.WIM``` image file. 

```.ESD``` image files are a more highly compressed version of the ```.WIM``` image file. 

The ```install.[wim|esd]``` image file contains the different editions (Home, Education, Pro, Data Centre, Core & NanoSever) and their variants of Windows which is used to install the version and variant of Window's on the computer. [App|Driver] Packages added to ```boot.wim``` to activate specialist hardware like RAID controllers or other hardware are typically added to ```install.[wim|esd]```, along with [App|Driver] Packages for the computer & user to use on a daily basis after Window's is installed, which is not required by the WindowsPE Setup process. 

```install.esd``` is the image file typically found in the USB memory stick when using the Windows Media Creation Tool.

```install.wim``` is typically found in the Window's Server [ ISO | VHD ] image file.

User App installation software can also be added to ```install.[wim|esd]``` for installation after Windows is installed, which can help with building an offline USB mem stick or ISO image file installation, to minimise downtime for such scenerios like working remotely, work-from-home or in locations without internet access, but where you still need ALL your software to be installed, configured and running in order to be able to carry on working.

As at 20250613:YYYYMMDD, the ```install.esd``` file can not be seen or selected as a Windows Image file in the WindowsSIM (System Image Manager) [10.0.26100.2454] program, but you can rename the ```install.esd``` file to ```install.wim``` and then work with the subsequent ```install.wim``` image file in WindowsSIM.
  
```
Windows Edition Order (in ascending order starting from 1):
MCT Windows 10 (22H2)/11 install.esd : Home, Home N, Home Single Language, Education, Education N, Pro, Pro N
Server 2016/2019/2022/2025 ISO install.wim : Server Standard Core, Server Standard, Server Data Centre Core, Server Data Centre
```
The N variants stand for "Not with Windows Media Player" and related Media Player apps, to comply with European Union law.

# Powershell Mount, Unmount ```.ISO``` and ```.VHD``` files

[Windows Server](https://en.wikipedia.org/wiki/Windows_Server) installation's come in ```.ISO``` and ```.VHD``` image files, and there is an option in the Windows MCT to save the desktop version of Window's to a ```.ISO``` file.

Once you have downloaded the image file, it needs to be [mounted](https://learn.microsoft.com/en-us/powershell/module/storage/mount-diskimage) (loaded to a drive letter) using [Powershell](https://learn.microsoft.com/en-us/powershell/scripting/overview) before you can modify it. 

```.ISO``` image files are mounted as Fixed Sized Read-only drives where nothing can be added or changed to the mounted drive.

```.VHD``` image files can be mounted as Fixed Sized, Resizable Read-Write drives, where files can be added or removed from the mounted drive.

With this in mind, the best way to modify an ```.ISO``` or ```.VHD``` image file is to mount it, copy the ```.ISO``` contents to another empty folder on the hard drive, work on that folder and its subfolders & files, and then save it all to a new ```.ISO``` or ```.VHD``` image file.


### Powershell [Get|Set]-ExecutionPolicy

The default installation of Powershell prevents scripts and modules from running. In order to run a script ```,ps1``` ormodule ```.psm1``` that's not a built in module, you need to change the Execution Policy.

To check the Execution Policy status and change it if need be, [load Powershell as Administrator](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/starting-windows-powershell#run-from-the-start-menu) and then type:

```
Get-ExecutionPolicy -list
```
Example output of [Get-ExecutionPolicy -List on a default installation](Powershell_Get-ExecutionPolicy_example.md).


If LocalMachine is set to ```Undefined```, or anything else other than ```RemoteSigned``` or ```ByPass``` run the following command to change the Execution Policy.

```
Set-ExecutionPolicy Bypass
```

Afterwards, remember to set the Execution Policy back using the commands below, to help "lock down" the machine.
```
Set-ExecutionPolicy Undefined
Get-ExecutionPolicy -list
```

### Powershell Mount & Copy ISO

[Load Powershell as Administrator](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/starting-windows-powershell#run-from-the-start-menu)  then [mount](https://learn.microsoft.com/en-us/powershell/module/storage/mount-diskimage) the ```ISO``` or ```VHD``` file. In the example below, the ```.ISO``` is mounted, with the result being saved in a new object called ```$DiskImageResult```. The object ```$DiskImageResult``` is piped to the ```Get-Volume``` command. The Drive letter of the ```$DiskImageResult``` is saved to a new object called ```$DiskImageDriveLetter```. Finally the mounted drive path is copied to a blank folder, in this example ```c:\mount```.

```
$DiskImageResult = Mount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\WS_2016_en-us.ISO"
$DiskImageDriveLetter = ($DiskImageResult | Get-Volume).DriveLetter
Copy-Item -Path "$($DiskImageDriveLetter):\" -Destination "C:\mount" -Recurse
```
Example output of the above [Mount & Copy Powershell commands](Powershell_Mount-DiskImage_Copy-Item_example.md).


### Powershell Dismount ISO

[Load Powershell as Administrator](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/starting-windows-powershell#run-from-the-start-menu)  then [dismount](https://learn.microsoft.com/en-us/powershell/module/storage/dismount-diskimage) the ```ISO``` or ```VHD``` file.

```
Dismount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\WS_2016_en-us.ISO"
```
Example output of the [dismount](Powershell_Dismount-DiskImage_example.md) powershell command.


Edit the ```.ISO``` or ```.VHD``` image file in ```C:\mount```. Treat this as if you were editing the USB mem stick created by the [Windows Media Creation Tool](https://support.microsoft.com/en-gb/windows/create-installation-media-for-windows-99a58364-8c02-206f-aa6f-40c3b507420d), eg load the ```boot.wim``` and ```install.[wim|esd]``` files using DISM to add driver packages, add the ```AutoUnattend.xml``` file and optionally add drivers or software before then saving ```C:\mount``` to a new ```.ISO``` or ```.VHD``` file.


### Powershell Save to ISO

To save the finished ```C:\mount``` folder along with its files and subfolders, to make it a Windows installation ```.ISO``` or ```.VHD``` image file, download and run the [New-ISOFile powershell module script](New-ISOFile.psm1) to create the resultant ```.ISO``` file. 

In the example below, the powershell module is imported for use before being executed in order to make the ```ISO``` image file. The module is only available for the lifetime of the session (powershell window) it was imported into and is not available in new powershell windows afterwards, unless imported again.

The command line format is ```New-ISOFile "Path\To\Source\Folder" "Path\To\Destination\Imagefilename.iso"
```-Verbose``` switches on extra messages in order to monitor the progress of the process. This is useful for older and/or slower computers.

Additional command like switches can be found by reading the source code of the ```New-ISOFile.psm1``` file. 

```
import-Module "C:\Users\Admin1\Documents\ISO Files\New-ISOFile.psm1"
New-ISOFile "C:\mount" "C:\Users\Admin1\Documents\ISO Files\WS2016test.iso" -verbose
```

, before optionally burning it to DVD. The Powershell Module ```.PSM1``` needs to be downloaded from [here](New-ISOFile.psm1). Its a modified version of this [one](https://thedotsource.com/2021/03/16/building-iso-files-with-powershell-7/), I've added a couple of extra ```-verbose``` messages for extra feedback for slow computers. 
The import-Module installs the ```New-ISOFile.psm1``` module for the duration of the powershell session only. Its unloaded when the Powershell window is closed. If you start additional Powershell windows whilst the Powershell window which ran the ```import-module``` command is still running, it wont be available to use, you need to run the ```import-module``` command for the newly opened Powershell window.

```
import-Module "C:\Users\Admin1\Documents\ISO Files\New-ISOFile.psm1"
New-ISOFile "C:\mount" "C:\Users\Admin1\Documents\ISO Files\WS2016test.iso" -verbose
```

```
PS C:\WINDOWS\system32> import-Module "C:\Users\Admin1\Documents\ISO Files\New-ISOFile.psm1"
PS C:\WINDOWS\system32> New-ISOFile "C:\mount" "C:\Users\Admin1\Documents\ISO Files\WS2016test.iso" -verbose
VERBOSE: Function start.
VERBOSE: Processing nested system
VERBOSE: Adding ISOFile type.
VERBOSE: Adding type for PowerShell 5.
VERBOSE: Selected media type is DVDPLUSRW_DUALLAYER with value 13
VERBOSE: Initialising image object.
VERBOSE: initialised.
VERBOSE: Performing the operation "New-ISOFile" on target "C:\Users\Admin1\Documents\ISO Files\WS2016test.iso".
VERBOSE: Fetching items from source directory.
VERBOSE: Got source items.
VERBOSE: Adding items to image. Wait at least 1 min, then if new lines below dont appear press Enter - It sometimes hangs.
VERBOSE: Adding boot
VERBOSE: Adding efi
VERBOSE: Adding NanoServer
VERBOSE: Adding sources
VERBOSE: Adding support
VERBOSE: Adding test
VERBOSE: Adding autorun.inf
VERBOSE: Adding bootmgr
VERBOSE: Adding bootmgr.efi
VERBOSE: Adding setup.exe
VERBOSE: Writing out ISO file to C:\Users\Admin1\Documents\ISO Files\WS2016test.iso
VERBOSE: Target File Created:C:\Users\Admin1\Documents\ISO Files\WS2016test.iso Starting to write contents, this can take several minutes... Press Enter to update in case it hangs.
VERBOSE: Writing out to ISO file:C:\Users\Admin1\Documents\ISO Files\WS2016test.iso completed.
VERBOSE: File complete.


    Directory: C:\Users\Admin1\Documents\ISO Files


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        19/06/2025     14:40     6971064320 WS2016test.iso
VERBOSE: Function complete.


PS C:\WINDOWS\system32>
```

To check to make sure the New-ISOFile.psm1 has not installed, you can check the physical folder ```C:\Program Files\WindowsPowerShell\Modules``` or use the following self explanatory commands.

```
Get-Module -ListAvailable -Name New-ISOFile
Remove-Module "C:\Users\Admin1\Documents\ISO Files\New-ISOFile.psm1"
Uninstall-Module New-ISOFile
```
# OSCDImg

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/oscdimg-command-line-options


# DISM, mount, unmount, commit, discard ```boot.wim```, ```install.wim```, ```install.esd``` files

MCT and Server ISO ```boot.wim```, ```install.wim``` or ```install.esd``` files can be mounted using [DISM](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/what-is-dism) from [Powershell](https://learn.microsoft.com/en-us/powershell/), where the ```install.wim``` or ```install.esd``` image file can be altered. 

[App|Driver] Packages can be added or removed from the ```boot.wim```, ```install.wim``` or ```install.esd```. 

VHD files should only use ```/index:1```. 

[DISM Image related commands](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/dism-image-management-command-line-options-s14).

```
dism /mount-image /imagefile:"<path_to_WIM_or_ESD_image_file>" /mountdir:"<folder_that_exists>" /index:<Windows_Edition_Order>
dism /mount-image /imagefile:"D:\Sources\install.esd" /mountdir:"C:\Win10_Home_N_64bit" /index:2
dism /mount-image /imagefile:"E:\x64\sources\install.esd" /mountdir:"C:\Win11_Education_64bit" /index:4
dism /mount-image /imagefile:"E:\x86\sources\install.esd" /mountdir:"C:\Win10_Home_32bit" /index:1
dism /mount-image /imagefile:"F:\sources\install.wim" /mountdir:"C:\Server2016_Standard" /index:2
dism /mount-image /imagefile:"F:\sources\install.wim" /mountdir:"C:\Server2025_Data_Centre" /index:4
dism /mount-image /imagefile:"G:\sources\install.vhd" /mountdir:"C:\Server2025_VHD_Only_Uses_Index1" /index:1

dism /get-mountedwiminfo # lists mounted images and shows their status

dism /remount-image /MountDir:"C:\mount" # remount the image file

DISM /Image:"C:\mount" /Cleanup-image /Restorehealth # cleanup and repair using online Windows Update Servers
DISM /Image:"C:\mount" /Online /Cleanup-Image /RestoreHealth /Source:\\<computername>\c$\winsxs /LimitAccess # Use a running version of windows to clean up & repair. /Online refers to a Running instance of windows. 



PS C:\WINDOWS\system32> dism /mount-image /imagefile:"E:\x64\sources\install.esd" /mountdir:"C:\Win10_Pro_N_64bit" /index:7

Deployment Image Servicing and Management tool
Version: 10.0.26100.1150

Mounting image
[==========================100.0%==========================]
The operation completed successfully.
PS C:\WINDOWS\system32>



dism /unmount-image /mountdir:"<folder_that_exists>" /commit
dism /unmount-image /mountdir:"C:\Win11_Pro_N_64bit" /commit

dism /unmount-image /mountdir:"<folder_that_exists>" /discard
dism /unmount-image /mountdir:"C:\Win10_Education_32bit" /discard

PS C:\WINDOWS\system32> dism /unmount-image /mountdir:"C:\Win11_Pro_N_64bit" /commit

Deployment Image Servicing and Management tool
Version: 10.0.26100.1150

Unmounting image
[==========================100.0%==========================]
The operation completed successfully.
PS C:\WINDOWS\system32>
``` 
[More information on Modifying a Windows image using DISM](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/mount-and-modify-a-windows-image-using-dism)

If you are having problems getting DISM to unmount an image file, check to make sure the folder used to mount the image file to is empty, and then check the registry ```Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WIMMount\Mounted Images```. 
This assumes you only have one image file loaded, but if you dont, make sure the GUID in the registry key that needs to be removed is the correct one.


# DISM Driver Packages

The ```install.wim``` and ```install.esd``` starts off with an empty driver store.

[DISM Packages in general](https://learn.microsoft.com/en-us/windows-hardware/drivers/install/driver-packages)

[Components of a driver package, INF file, catalog file, driver files & other files](https://learn.microsoft.com/en-us/windows-hardware/drivers/install/components-of-a-driver-package)


[Add and Remove packages to a mounted offline (not running) boot.wim, install.wim or install.esd file](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/add-and-remove-drivers-to-an-offline-windows-image)


The ```.INF``` file contains a list of the files the driver package needs to function properly and is added to the [Windows Driver Store](https://learn.microsoft.com/en-us/windows-hardware/drivers/install/driver-store), ready for automatic installation when Windows is installed. 

This method can exclude additional software which would have been installed using the user's "manually operated" driver installation program. 

Drivers are installed into the Windows Driver Store in the ```install.wim``` or ```install.esd``` image file and named ```OEMnnn.inf``` where nnn starts at 0 and increments by 1, eg ```MyWlan.inf``` becomes ```OEM0.inf```, ```myUnsignedBluetoothDriver.inf``` becomes ```OEM1.inf```, ```MyRaidController.inf``` becomes ```OEM2.inf``` and so on. 

The below example, in order, mounts, adds a single signed driver, all signed drivers, a signed or unsigned driver, all signed or unsigned drivers, removes OEM0.inf and OEM8.inf drivers, commits & unmounts.
```
Dism /mount-image /imagefile:"D:\sources\install.esd" /mountdir:"C:\Win11_Pro_64bit" /index:7
Dism /Image:"C:\Win11_Pro_64bit" /Add-Driver /Driver:"C:\drivers\mySignedWlanDriver.inf" # OEM0.inf - add a single signed driver to the driver store
Dism /Image:"C:\Win11_Pro_64bit" /Add-Driver /Driver:"C:\AllSignedDrivers" /Recurse # OME1.inf to OEM7.inf - adds all the driver packages (.INF files) including subfolders where necessary.
Dism /Image:"C:\Win11_Pro_64bit" /Add-Driver /Driver:"C:\drivers\myUnsignedBluetoothDriver.inf" /ForceUnsigned # OEM8.inf - adds a single driver regardless of if its signed or unsigned
Dism /Image:"C:\Win11_Pro_64bit" /Add-Driver /Driver:"C:\AllSignedAndUnsignedDrivers" /Recurse /ForceUnsigned # OEM9.inf to OEM15.inf - adds all driver packages including subfolders, regardless of if the driver is signed or unsigned
Dism /Image:"C:\Win11_Pro_64bit" /Remove-Driver /Driver:OEM0.inf /Driver:OEM8.inf # remove mySignedWlanDriver.inf and myUnsignedBluetoothDriver.inf
Dism /unmount-image /mountdir:"C:\Win11_Pro_64bit" /commit
```

Below is the command to check the drivers have been installed properly, ```OEMnnn.inf``` shows the order the ```.INF``` driver files were added to the Windows Driver Store and the original ```.INF``` filename can be seen in the resulting output, an example can be seen [here](DismDriverPackagesExample.md).
```
Dism /Image:"C:\Win11_Pro_64bit" /Get-Drivers
```

# DISM App Packages & Windows Update (```.CAB``` & ```.MSU```) Packages

The ```install.wim``` and ```install.esd``` image file starts off with a selection of App & Window Update packages to bring them upto a certain release. Adding required packages and removing unnecessary packages is a good way to reduce the attack vector by reducing the number of unnecessary apps & services which are installed in a default installation of Windows.

You can search for the latest Window's Updates on the [Microsoft Catalogue](https://www.catalog.update.microsoft.com) website but you need to check they can be installed from the Catalogue using DISM.

As at 20250613:YYYYMMDD, using the Windows Media Creation Tool for Windows 11, the current release for ```install.esd``` is ```Windows 11 2024 Update l Version 24H2```.

The [Microsoft Catalogue](https://www.catalog.update.microsoft.com) using ```windows 11```, ```24h2```, ```x64``` and ISO Date format ```YYYY-MM``` ```2025-06``` as the search term for the latest Windows Update files returns 2 items as seen [here](https://www.catalog.update.microsoft.com/Search.aspx?q=windows%2011%2024h2%20x64%202025-06%20%20) 

At the time of writing, 2 updates exist:
```
2025-06 Cumulative Update for Windows 11 Version 24H2 for x64-based Systems (KB5063060) Classification: Updates
2025-06 Cumulative Update for Windows 11 Version 24H2 for x64-based Systems (KB5060842) Classification: Security Updates
```  
Before downloading the update(s), you need to check that they can be installed using DISM.

Click on the KB Update's weblink to get a popup window which shows the Support URL. 

The Support URL format is ```https://support.microsoft.com/help/[Number of the KB]``` eg ```https://support.microsoft.com/help/5063060```. 

Scroll down to the [Install this update](https://support.microsoft.com/help/5063060#:~:text=To%20install%20this%20update,%20use%20one%20of%20the%20following%20Windows%20and%20Microsoft%20release%20channels.&text=Catalog) section, click the ```Catalog``` tab and see if it can be installed using DISM. 

The KB5063060 update at the time of writing (20250613:YYYYMMDD) can not be added to the Windows ```install.esd``` image file using DISM.

The Security Update [KB5060842](https://support.microsoft.com/help/5060842#:~:text=To%20install%20this%20update,%20use%20one%20of%20the%20following%20Windows%20and%20Microsoft%20release%20channels.&text=Catalog) can be installed to the Windows ```install.esd``` image file using DISM because it displays the instructions to do so. 

Below, the example mounts the Window 24H2 image file, renamed from ```install.esd``` to ```installWin11.wim```, adds the KB5060842 Security Update package and then unmounts the image file.
```
Dism /mount-image /imagefile:"C:\Users\Admin1\Documents\WIM files\installW11.wim" /mountdir:"C:\mount" /index:7   # If not already Mounted
Dism /Image:"C:\mount" /Add-Package /PackagePath:"C:\Users\Admin1\Downloads\windows11.0-kb5060842-x64_07871bda98c444c14691e0a90560306703b739cf.msu"
Dism /unmount-image /mountdir:"C:\mount" /commit # Commit (Save) and Unmount
```

Below is a list of the main commands for package and feature maintainence. 
```powershell
Dism /mount-image /imagefile:"C:\Users\Admin1\Documents\WIM files\installW11.wim" /mountdir:"C:\mount" /index:7 # If not already Mounted
Dism /Image:"C:\mount" /Get-Packages /Format:Table # list packages installed in the install.wim or install.esd image file.
Dism /Image:"C:\mount" /Get-PackageInfo /PackageName:"Microsoft-Windows-Foundation-Package~31bf3856ad364e35~amd64~~10.0.26100.1" # get slighty more info than what is displayed using the /Get-Packages /Format:Table command.
Dism /Image:"C:\mount" /Get-Features /Format:Table # list features of windows 
Dism /Image:"C:\mount" /Get-FeatureInfo /FeatureName:"Windows-Defender-Default-Definitions"
Dism /Image:"C:\mount" /Get-PackageInfo /PackagePath:"C:\Users\Admin1\Documents\WIM files\windows11.0-kb5063060-x64_96be31e3e3e1cbc216229abb83e5be9da4e08496.msu"
Dism /Image:"C:\mount" /Add-Package /PackagePath:"C:\Users\Admin1\Documents\WIM files\windows11.0-kb5063060-x64_96be31e3e3e1cbc216229abb83e5be9da4e08496.msu"
Dism /Image:"C:\mount" /Get-Packages /Format:Table
Dism /Image:"C:\mount" /Add-Package /PackagePath:"C:\Users\Admin1\Documents\WIM files\windows11.0-kb5043080-x64_953449672073f8fb99badb4cc6d5d7849b9c83e8.msu"
Dism /Image:"C:\mount" /Add-Package /PackagePath:"C:\Users\Admin1\Documents\WIM files\windows11.0-kb5063060-x64_96be31e3e3e1cbc216229abb83e5be9da4e08496.msu"
Dism /Image:"C:\mount" /Get-Packages /Format:Table
Dism /Image:"C:\mount" /Get-PackageInfo /PackageName:"Microsoft-Windows-Foundation-Package~31bf3856ad364e35~amd64~~10.0.26100.1"
Dism /Image:"C:\mount" /Remove-Package /PackagePath:"C:\Users\Admin1\Documents\WIM files\windows11.0-kb5063060-x64_96be31e3e3e1cbc216229abb83e5be9da4e08496.msu"
Dism /Image:"C:\mount" /Remove-Package /PackagePath:"C:\Users\Admin1\Documents\WIM files\windows11.0-kb5043080-x64_953449672073f8fb99badb4cc6d5d7849b9c83e8.msu"
 
Dism /unmount-image /mountdir:"C:\mount" /commit
```

[Windows DISM Package States](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/dism/dismpackagefeaturestate-enumeration)

Servicing Stack Updates (SSU) are updates for the Windows Updates part of Windows Server. SSU's are only available for Window's Server and are built into Window Desktop Updates.  


# AutoUnAttend Configuration Passes

[How Configuration Passes Work](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/how-configuration-passes-work)

[Unattended Windows Setup Reference](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/)

Below are the Configuration Passes in order of processing when it exists or is enabled. 

[Audit Mode and ForceShutdownNow](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/boot-windows-to-audit-mode-or-oobe) determines whether the ```auditSystem``` and ```auditUser``` configuration passes run or the ```oobeSystem``` configuration pass runs, or, all of them run. 

For a table detailing the different Configuration Passes ```Mode``` and ```ForceShutdownNow``` combination click [here](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-deployment-reseal-mode).

[windowsPE](windowsPE.md) Windows Preinstallation Environment (Windows PE) is a small operating system where [Components](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/components-b-unattend) for the Windows installation and WinPE are set, like [Display resolutions](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-display), [Disk partitions](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-createpartitions-createpartition), [Product Keys](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-productkey) & [generic product keys](https://www.elevenforum.com/t/generic-product-keys-to-install-or-upgrade-windows-11-editions.3713/) and specific commands or scripts.

[offlineServicing](offlineServicing.md)  Apply unattended Setup setting to an offline Microsoft Windows image, like drivers, language packs, update packages and other packages.

[generalize](generalize.md) The generalize pass is used to create a reference or master image that can be used throughout an organisation. Its the master image before department customisations take place in the specialize pass.

[specialize](specialize.md) The specialize pass is where machine specific settings are processed, like domain information, wifi, network, international settings, department webpages. It runs on the next reboot after the generalize pass. 

[auditSystem](auditSystem.md)  IF the optional auditMode is activated, the auditSystem pass runs as System immediately before login and auditUser and is where OEM's can install device drivers, applications and other updates.

[auditUser](auditUser.md)  IF the optional auditMode is activated, the auditUser pass runs after login as User immediately after auditSystem and is used to execute RunSynchronous or RunAsynchronous commands for the default user profile which is used to configure and personalise all subsequent user accounts. This includes HKEY_USERS\DefaultUser\

[oobeSystem](oobeSystem.md)  The oobeSystem (Out-Of-Box-Experience) pass is where the settings for the users first login are processed. OOBE is the users first boot experience.

[Extensions](Extensions.md) Where files and scripts can be stored in the AutoUnattend.xml before being extracted to specific locations typically on the windows partition.


https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/components-b-unattend



# Components

Components can be added to one or more specified configuration passes, check the specific component webpage to see what configurations passes it can be added to. For a list of the latest components, visit the link below.

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/components-b-unattend

The minimum configuration for ```AutoUnattend.xml``` to install a single hard drive Windows Desktop using components, without user input using a USB mem stick aka a fully automated installation of windows. 



| Configuration Pass | Component | Setting Name | Notes |
| --- | --- | --- | --- |
| [windowsPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/windowsPE.md) | [Microsoft-Windows-Setup](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-Setup.md) | [ImageInstall](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-Setup.md#imageinstall) | ImageInstall specifies the Windows image or secondary data image to install and the location to which the image is to be installed. |
| [windowsPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/windowsPE.md) | [Microsoft-Windows-Setup](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-Setup.md) | [UserData](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-Setup.md#userdata) | Specify the version of windows to install. |
| [windowsPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/windowsPE.md) | [Microsoft-Windows-Setup](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-Setup.md) | [UseConfigurationSet](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-Setup.md#useconfigurationset) | The variable ```%configsetroot%``` refers to the root drive/folder of the USB memory stick, for things like installing additional drivers, packages and/or software, useful for offline installations. |
| [windowsPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/windowsPE.md) | [Microsoft-Windows-Setup](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-Setup.md) | [DiskConfiguration](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration) | The settings [WillWipeDisk](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-willwipedisk), [Disk](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk) & more, that Windows uses to partition and configure one or more physical hard disks. |
| [windowsPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/windowsPE.md) | [Microsoft-Windows-Setup](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-Setup.md) | [RunSynchronous](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-runsynchronous) | Run Commands & Scripts to perform tasks or changes not carried out by a Component Setting. |
| [windowsPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/windowsPE.md) | [Microsoft-Windows-International-Core-WinPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-International-Core-WinPE.md) | [SetupUILanguage](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-setupuilanguage) | The language to use in Windows Setup and Windows Deployment Services. |
| [windowsPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/windowsPE.md) | [Microsoft-Windows-International-Core-WinPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-International-Core-WinPE.md) | [UILanguage](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-uilanguage) | Child Element of SetupUILanguage. The language that is used to display menu items in Windows Setup. |
| [windowsPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/windowsPE.md) | [Microsoft-Windows-International-Core-WinPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-International-Core-WinPE.md) | [InputLocale](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-inputlocale) |  The input device (eg. keyboard) language and method. |
| [windowsPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/windowsPE.md) | [Microsoft-Windows-International-Core-WinPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-International-Core-WinPE.md) | [SystemLocale](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-systemlocale) | The default language for non-Unicode programs. |
| [windowsPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/windowsPE.md) | [Microsoft-Windows-International-Core-WinPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-International-Core-WinPE.md) | [UILanguage](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-uilanguage) | Standalone Setting. Default user interface (UI) language of the Windows installation. |
| [windowsPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/windowsPE.md) | [Microsoft-Windows-International-Core-WinPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-International-Core-WinPE.md) | [UserLocale](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-userlocale) | The default user setting for formatting dates, times, currency, and numbers in a Windows installation and the Windows Setup process. |
| [windowsPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/windowsPE.md) | [Microsoft-Windows-PnpCustomizationsWinPE](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-PnpCustomizationsWinPE.md) | [DriverPaths](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/Microsoft-Windows-PnpCustomizationsWinPE.md#path) | DriverPaths specifies one or more paths that contain out-of-box drivers. These out-of-box drivers are copied to the Windows image [file\|box] during the windowsPE configuration pass. ```$WinpeDriver$``` can be used. |
| [auditSystem](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/auditSystem.md) | [Microsoft-Windows-Deployment](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/auditSystem.md#microsoft-windows-deployment-component) | [Mode](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-deployment-reseal-mode) | Mode [Audit\|OOBE] setting with the ```ForceShutdownNow``` setting, controls whether the computer starts in audit mode or Out-of-Box-Experience (OOBE). |
| [auditSystem](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/auditSystem.md) | [Microsoft-Windows-Deployment](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/auditSystem.md#microsoft-windows-deployment-component) | [ForceShutdownNow](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-deployment-reseal-forceshutdownnow) | ForceShutdownNow [true\|false] setting with the ```Mode``` setting, controls whether the computer starts in audit mode or Out-of-Box-Experience (OOBE). |
| [auditSystem](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/auditSystem.md) | [Microsoft-Windows-Deployment](https://github.com/Intelligent-Silicon/AutoUnattend/blob/main/auditSystem.md#microsoft-windows-deployment-component) | [Reseal](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-deployment-reseal) | Reseal indicates whether the computer runs in audit mode or Windows Out-of-Box Experience (OOBE) when the computer is next started. |

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-deployment

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-folderlocations-profilesdirectory

https://learn.microsoft.com/en-us/troubleshoot/windows-server/user-profiles-and-logon/relocation-of-users-and-programdata-directories

https://superuser.com/questions/1577924/can-i-move-the-c-users-folder-to-d-drive

# Install User software

## SetupComplete.cmd

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/add-a-custom-script-to-windows-setup?view=windows-11


# ErrorHandler.cmd

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/add-a-custom-script-to-windows-setup#run-a-script-if-windowssetup-encounters-a-fatal-error-errorhandlercmd

# FirstLogonCommands

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-firstlogoncommands

### How to for AutoUnattend.xml answer file

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/wsim/windows-system-image-manager-how-to-topics

### How to add drivers to offlineServicing in AutoUnattend.xml file 

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/wsim/add-a-device-driver-path-to-an-answer-file


https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/add-and-remove-drivers-to-an-offline-windows-image#add-driver-packages-to-an-offline-windows-image-by-using-an-unattended-answer-file

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-command-line-options

### How to validate an answer file

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/wsim/validate-an-answer-file

