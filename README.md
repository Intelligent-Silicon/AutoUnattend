# AutoUnattend.xml

AutoUnattend.xml is an answer file which can control and configure the installation of windows 10 (version 1607 and onwards) or Windows Server 2016, 2019, 2022 and windows 11 or Windows Server 2025, along with user software in an expedient, consistent and hands free way on a computer, to make the computer yours.


The AutoUnattend.xml file can be used with the Windows 10 and Windows 11 Media Creation Tool (MCT) by adding the file to the USB memory stick or the ISO aka DVD (image) file created by the MCT, or by adding it to the Windows Server ISO files and VHD/VHDX (Virtual Hard Disk) and FFU (Full FlashUpdate) files.

Device Drivers pertinent for the computer and/or network device(s) like printer's, scanner's or camera's can be installed and configured using the answer file or added to the [Windows Driver Store](https://learn.microsoft.com/en-us/windows-hardware/drivers/install/driver-store) in the ```install.wim```, ```install.esd``` image files using [DISM](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/what-is-dism).

3rd Party User software like [Softvelocity's Clarion](https://www.softvelocity.com/), [Visual Studio](https://visualstudio.microsoft.com/), [Notepad++](https://notepad-plus-plus.org/) and more, can be installed and configured after windows has been installed, provided the 3rd party user installation software allows command line switches (flags).

Windows can then be configured to work just the way you like it after windows has been installed, using a variety of methods like ```.BAT``` (Batch), ```.CMD``` (Command), ```.PS1``` ([Powershell](https://learn.microsoft.com/en-us/powershell/)) script files, to alter the [Windows Registry](https://learn.microsoft.com/en-us/windows/win32/sysinfo/registry) or [Windows Group Policy](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-policy/group-policy-overview) settings (where applicable).



# TLDR

# Less is More 

Less or essential only programs & packages installed to a Windows Image ```install.wim``` or ```install.esd``` file and/or a Windows installation means a reduced [Attack Vector](https://en.wikipedia.org/wiki/Attack_vector) which helps improve [Computer Security](https://en.wikipedia.org/wiki/Computer_security) and helps keep the computer responsive.

# Basic Layout of the AutoUnattend.xml file

Configuration Pass sections contain zero, one or more Component sections.

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

As at 20250613:YYYYMMDD, also note ```DPI``` is not listed as a child element, but is shown in the 2nd XML example on the Microsoft webpage.

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

"nonSxS" is a switch to force dll's or .net assemblies to be loaded in an exclusive manner similar to those described in the [remarks section concerning the Search Path](https://learn.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-loadlibraryexa#searching-for-dlls-and-dependencies) used with the [LoadLibraryEx Windows API (Application Programmer Interface)](https://learn.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-loadlibraryexa) in an attempt to mitigate the hijacking of dll's and assemblies as described [here](https://learn.microsoft.com/en-us/windows/win32/dlls/dynamic-link-library-security). After Windows installation, SXS dll's can be found in the ```%systemroot%\WinSxS``` folder. An application's Manifest can control the use of SXS dll's and assemblies. More information [here](https://learn.microsoft.com/en-us/windows/win32/sbscs/application-manifests#assemblyIdentity)


# Creating the AutoUnAttend.xml file

There are a few ways to create/modify the AutoUnAttend.xml file, [an online generator can be found at schneegans.de](https://schneegans.de/windows/unattend-generator/), Microsoft provides the [Windows System Image Manager (Windows SIM)](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/wsim/windows-system-image-manager-technical-reference) found in the [Windows Assessment and Deployment Kit (Windows ADK)](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install), or modifying [various AutoUnAttend.xml files on Github](https://github.com/search?q=autounattend.xml&type=repositories) and elsewhere which can be modified using a [text editor](https://notepad-plus-plus.org/) or [XML editor](https://microsoft.github.io/XmlNotepad/).  

Explanations and examples of the Configuration Passes and Components are explored in greater detail below and on other pages in this repo.


# Creating a Windows installer

There are a few different ways to install Windows. 

The finished AutoUnattend.xml file is added to the root folder of the Media Creation Tool (MCT) created USB memory stick or ISO image file, or Windows Server ISO, before burning to DVD where applicable.

Most members of the public and businesses will use the [Windows 10 (22H2)](https://www.microsoft.com/en-gb/software-download/windows10) or [Windows 11](https://www.microsoft.com/en-gb/software-download/windows11) MCT to download Windows onto a USB memory stick to boot from and install Windows, or use MCT to create an ISO image file which can be used with [virtualisation software](https://en.wikipedia.org/wiki/Virtual_machine) like [VMware Workstation](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion) or to burn a DVD capable of installing Windows. [Windows Server 2016 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016), [Windows Server 2019 ISO/VHD](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019), [Windows Server 2022 ISO/VHD](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022) or [Windows Server 2025 ISO/VHD](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025) only exist in ISO & VHD forms and are not included as an option in the MCT. 

Other methods exist for installing Windows 10/11/Server which are more technical and/or have specialist reasons for existing.

[The Windows Image (.WIM/.ESD) vs Virtual Hard Disk (.VHD/.VHDX) vs Full Flash Update (.FFU) pro's and con's can be seen here.](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/wim-vs-ffu-image-file-formats)


# ```boot.wim```, ```install.wim```& ```install.esd```


The ```boot.wim```, ```install.wim```& ```install.esd``` are contained in ```[Mem stick|ISO]Drive Letter:\sources```, or ```[Mem stick|ISO]Drive Letter:\x86\sources``` & ```[Mem stick|ISO]Drive Letter:\x64\sources``` when both 32bit (x86) and 64bit (x64) are selected for optional installation using the MCT.

```boot.wim``` contains the Windows Installer software that is the Windows PE and Windows Setup software used to install Windows. Packages & drivers can be added to activate specialist hardware like RAID controllers or additional networking functionality in order to install a Windows edition onto the computer.

```install.wim``` & ```install.esd``` contains the different editions (Home, Education, Pro, Data Centre, Core) of Windows which is used to install on the computer or server. Packages and drivers to activate specialist hardware like RAID controllers or additional networking functionality added to ```boot.wim``` are typically added to ```install.wim``` & ```install.esd``` as well, along with Packages and drivers for the computer to use on a day to day basis after installation which is not required during the installation process.

The ```install.esd``` file is a more recently introduced, more highly compressed version of the ```install.wim``` file to install Windows used with the MCT. 

As at 20250613:YYYYMMDD, the ```install.esd``` file can not be seen or selected as a Windows Image in the Windows System Image Manager [10.0.26100.2454] program, but you can rename the ```install.esd``` file to ```install.wim``` and then select the subsequent ```install.wim``` file to work in the Windows System Image Manager.
  
  
```
Windows Edition Order (in ascending order starting from 1):
MCT Windows 10 (22H2)/11 install.esd : Home, Home N, Home Single Language, Education, Education N, Pro, Pro N
Server 2016/2019/2022/2025 ISO/VHD install.wim : Server Standard Core, Server Standard, Server Data Centre Core, Server Data Centre
```
The N versions stand for "Not with Windows Media Player" and related Media Player apps, to comply with European Union law.



# DISM, mount, unmount, commit, discard

MCT and Server ISO ```install.wim``` and ```install.esd``` files can be mounted using [DISM](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/what-is-dism) from [Powershell](https://learn.microsoft.com/en-us/powershell/), where the ```install.wim``` or ```install.esd``` image file can be altered. Drivers and Packages can be added or removed. VHD files should only use ```/index:1```. 

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

dism /remount-image /MountDir:"C:\mount"

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
[For more information on Modifying a Windows image using DISM] (https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/mount-and-modify-a-windows-image-using-dism)

# DISM Driver Packages

The ```install.wim``` and ```install.esd``` starts off with an empty driver store.

[DISM Packages in general](https://learn.microsoft.com/en-us/windows-hardware/drivers/install/driver-packages)

[Components of a driver package, INF file, catalog file, driver files & other files](https://learn.microsoft.com/en-us/windows-hardware/drivers/install/components-of-a-driver-package)


[Add and Remove packages to a mounted offline (not running) boot.wim, install.wim or install.esd file](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/add-and-remove-drivers-to-an-offline-windows-image)


The ```.INF``` file contains a list of the files the driver package needs to function properly and is added to the [Windows Driver Store](https://learn.microsoft.com/en-us/windows-hardware/drivers/install/driver-store), ready for automatic installation when Windows is installed. 

This method can exclude additional software which would have been installed using the User's "manually operated" installation program. 

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

The ```install.wim``` and ```install.esd``` starts off with a selection of App & Window Update packages. Adding required packages and removing unnecessary packages is a good way to reduce the attack vector by reducing the number of unnecessary services and apps which are installed in a default installation of Windows.

You can search for the latest Window's Updates on the [Microsoft Catalogue](https://www.catalog.update.microsoft.com/Search.aspx?q=windows%2011%2024h2%20%202025-06%20x64-based%20) and add them to the ```install.wim``` and ```install.esd```. You can also remove packages you dont want installed at this stage, instead of writing a Powershell script to remove them after Windows has been installed using the AutoUnattend.xml file.

When adding Windows Update packages from the [Microsoft Catalogue](https://www.catalog.update.microsoft.com/Search.aspx?q=windows%2011%2024h2%20%202025-06%20x64-based%20), the latest package will replace earlier monthly Windows Update packages, but the Window Update package may also list other prerequisite dependency packages that need to be installed before the latest Windows Update package. 

For example, [Windows 11 2025-06 Windows Update package KB5063060 for x64 based systems](https://www.catalog.update.microsoft.com/Search.aspx?q=2025-06%20Cumulative%20Update%20for%20Windows%2011%20Version%2024H2%20for%20x64-based%20Systems%20(KB5063060)%20) requires KB5043080 to be added as a prerequiste dependency. To see KB5043080 and its download link, click the Download button in the above link. 

In this example the prerequiste dependancy file KB5043080 .MSU file is added first, and then the latest as at 2025-06 Windows Update for x64bit Windows 11 KB5063060 .MSU package is added second. This is because DISM will check the prequisite files exist in the packages first unless the command flag/switch ```/IgnoreCheck``` is used.


```
Dism /mount-image /imagefile:"C:\Users\Admin1\Documents\WIM files\installW11.wim" /mountdir:"C:\mount" /index:7
Dism /Image:"C:\mount" /Get-Packages /Format:Table
Dism /Image:"C:\mount" /Get-PackageInfo /PackageName:"Microsoft-Windows-Foundation-Package~31bf3856ad364e35~amd64~~10.0.26100.1"
Dism /Image:"C:\mount" /Get-Features /Format:Table
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


https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options?view=windows-11


Dism /mount-image /imagefile:"C:\Users\Admin1\Documents\WIM files\installW11.wim" /mountdir:"C:\mount" /index:7
Dism /Image:"C:\mount" /Get-Drivers
Dism /Image:"C:\mount" /Add-Driver /Driver:"C:\drivers\Netwtw08.INF"
Dism /Image:"C:\mount" /Add-Driver /Driver:"C:\AllSignedDrivers" /Recurse



Windows 10 (22H2) contains
Windows 10 Home
Windows 10 Home N
Windows 10 Home Single language
Windows 10 Education 
Windows 10 Education N
Windows 10 Pro
Windows 10 Pro N

Windows 11 contains
Windows 11 Home
Windows 11 Home N
Windows 11 Home Single language
Windows 11 Education 
Windows 11 Education N
Windows 11 Pro
Windows 11 Pro N



Servicing Stack Updates (SSU) are updates for the Windows updates component. 


```boot.wim``` contains Windows PE and Windows Setup which are cut down version of Windows for installing Windows. They contain a limited number of Components which are most likely to be required for installing Windows, things like Networking & BlueTooth packages and a few others. 

.WIM files can contain one or more versions of Windows, like Home, both 32bit and 64bit and contain packages pertinent 



creates a ```rootfolder\sources\install.wim``` file, later versions of MCT create a ```rootfolder\sources\install.esd``` file.
Early versions of ISO image files contain ```rootfolder\sources\install.wim``` file, later versions of ISO image files contain ```rootfolder\sources\install.esd```

The .ESD file is not selectable in the Windows System Image Manager so change the file extension from .ESD to .WIM, and then you can select the .WIM file before selecting the version of Windows you want to create an AutoUnAttend.xml file for. The .ESD file is a highly compressed version of .WIM files.

Windows System Image Manager creates 


https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/wim-vs-ffu-image-file-formats?view=windows-11



Download the [Windows 10](https://www.microsoft.com/en-gb/software-download/windows10) or [Windows 11](https://www.microsoft.com/en-gb/software-download/windows11) Media Creation Tool, or [Windows Server 2016 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016), [Windows Server 2019 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019), [Windows Server 2022 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022) or [Windows Server 2025 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025).

Copy the AutoUnAttend.xml to the root folder of the USB Memory Stick or ISO image file, before optionally burning the latter to a DVD, and you are good to go.

Mount an ISO image file in order to copy the AutoUnAttend.xml to the root of the ISO image file.
```
Using File Explorer.
Navigate to the folder containing the ISO image file in File Explorer. Right mouse click on the ISO image file, from the popup menu choose "Mount" if Win11, or "Mount as Virtual Drive" if Win10. 
Navigate to the newly mounted ISO drive in File Explorer, and copy the AutoUnAttend.xml file to the root folder of the mounted ISO drive.

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-automation-overview?view=windows-11#replace-the-answer-file-in-an-offline-image

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/mount-and-modify-a-windows-image-using-dism?view=windows-11


Dism /Mount-Image /ImageFile:"C:\images\CustomImage.wim" /Index:1 /MountDir:C:\mount
Copy CustomAnswerFile.xml C:\mount\Windows\Panther\unattend.xml
Dism /Unmount-Image /MountDir:C:\mount /Commit




 
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

Configurations Passes run in order and where applicable if enabled. Components are added to a configuration pass and can be added to more than one Configuration Pass for those instances where another Configuration Pass is not enabled or required. For a more detailed overview visit the link below.


[How Configuration Passes Work](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/how-configuration-passes-work)

In order of processing, when a pass exists. AuditMode determines whether the auditSystem and auditUser configuration pass runs or the oobeSystem configuration pass runs.  

[windowsPE](windowsPE.md) Windows Preinstallation Environment (Windows PE) is a small operating system where settings for the installation and WinPE are set, like display resolutions, disk partitions, installation partition, licence keys and specific commands.

	1. Windows PE Settings component

	2. Windows Setup Settings component

[offlineServicing](offlineServicing.md)  Apply unattended Setup setting to an offline Microsoft Windows image, like drivers, language packs, update packages and other packages.

[generalize](generalize.md) The generalize pass is used to create a reference or master image that can be used throughout an organisation. Its the master image before department customisations take place in the specialize pass.

[specialize](specialize.md) The specialize pass is where machine specific settings are processed, like domain information, wifi, network, international settings, department webpages. It runs on the next reboot after the generalize pass. 

[auditSystem](auditSystem.md)  IF the optional auditMode is activated, the auditSystem pass runs as System immediately before login and auditUser and is where OEM's can install device drivers, applications and other updates.

[auditUser](auditUser.md)  IF the optional auditMode is activated, the auditUser pass runs after login as User immediately after auditSystem and is used to execute RunSynchronous or RunAsynchronous commands for the default user profile which is used to configure and personalise all subsequent user accounts. This includes HKEY_USERS\DefaultUser\

[oobeSystem](oobeSystem.md)  The oobeSystem (Out-Of-Box-Experience) pass is where the settings for the users first login are processed. OOBE is the users first boot experience.

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

