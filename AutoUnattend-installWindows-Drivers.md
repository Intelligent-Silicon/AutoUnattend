Identify the Drivers in the Offline Windows Image

PS C:\WINDOWS\system32> $DiskImageResult = Mount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32.iso"
PS C:\WINDOWS\system32> $DiskImageDriveLetter = ($DiskImageResult | Get-Volume).DriveLetter
PS C:\WINDOWS\system32> md -path "C:\mount"
PS C:\WINDOWS\system32> Copy-Item -Path "$($DiskImageDriveLetter):\*" -Destination "C:\mount" -Recurse
PS C:\WINDOWS\system32> Dismount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32.iso"

PS C:\WINDOWS\system32> md -path "C:\mount_Win10_Pro_x32"
PS C:\WINDOWS\system32> Dism /mount-image /imagefile:"C:\mount\sources\install.esd" /index:6 /mountdir:"C:\mount_Win10_Pro_x32"
PS C:\WINDOWS\system32> Dism /get-mountedwiminfo

PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_Pro_x32" /Get-Packages /Format:Table

This command doesnt take long - a minute or two
PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_Pro_x32" /Cleanup-Image /CheckHealth

This command takes age - like an hour or more
PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_Pro_x32" /Cleanup-Image /ScanHealth


Takes about 2-3mins
PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_Pro_x32" /Add-Package /PackagePath:"C:\mount_kb5060533\SSU-19041.5911-x86.cab"

Error: 2 The system cannot find the file specified.
PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_Pro_x32" /Add-Package /PackagePath:"C:\mount_kb5060533\Cab_2_for_KB5060533.cab"


Switch off Realtime AV scanning in Windows Defender for this, because its scanning the package as its being added and if you cant trust MS, who the fuck can you trust?
PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_Pro_x32" /Add-Package /PackagePath:"C:\mount_kb5060533\Windows10.0-KB5060533-x86-Unnested.cab"

21:20++++21:27-44%++++21:42-44%+++++21:53-44.9%
PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_Pro_x32" /Add-Package /PackagePath:"C:\mount_kb5060533\Windows10.0-KB5060533-x86-Unnested.cab"

Deployment Image Servicing and Management tool
Version: 10.0.26100.1150

Image Version: 10.0.19045.3803

Processing 1 of 1 - Adding package Package_for_RollupFix~31bf3856ad364e35~x86~~19041.5965.1.5
[=========================  44.0%                          ]

*************************************************************************************************
*************************************************************************************************
*************************************************************************************************
https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/deployment-image-servicing-and-management--dism--best-practices?view=windows-11#create-a-temporary-directory-in-which-to-store-update-files

https://www.elevenforum.com/t/how-to-identify-reclaimable-packages-reported-as-count-by-dism-online-cleanup-image-analyzecomponentstore.30344/page-4


You should use the /ScratchDir option with DISM to create a temporary directory on a different drive when you create or service a Windows image. A temporary directory is used for many DISM operations including capturing an image, installing language packs, installing updates, or installing or removing Windows features in a Windows image. Some files are expanded to this temporary directory before they are applied to a Windows image.
https://superuser.com/questions/1629909/how-to-modify-a-winpe-iso-to-also-have-the-ability-to-install-windows-from-the-w

Check C:\WINDOWS\Logs\DISM\dism.log for whats going on in realtime
https://www.reddit.com/r/techsupport/comments/11lf83v/dism_restore_health_stuck_on_623/

https://www.anoopcnair.com/add-windows-update-in-offline-image-using-dism/

LogPath – Specifies the logfile path.
/LogLevel – Specifies the output level shown in the log (1-4).
/Set-ScratchSpace:<size_of_ScratchSpace> Valid values are 32, 64, 128, 256 and 512.
/ScratchDir – Specifies the path to a scratch directory.

https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-7/dd799261(v=ws.10)?redirectedfrom=MSDN

*************************************************************************************************
*************************************************************************************************
*************************************************************************************************

Dism /Image:"C:\mount_Win10_Pro_x32" /Add-Package /PackagePath:"C:\mount_kb5060533\Windows10.0-KB5060533-x86.cab"

C:\mount_kb5060533


PS C:\WINDOWS\system32> Set-ItemProperty "C:\mount\sources\install.esd" -name IsReadOnly -value $false
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount\sources\install.esd" | Out-File -FilePath "C:\mount\sources\install.esd.txt"
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount\sources\install.esd" /index:6 | Out-File -FilePath "C:\mount\sources\install.esd.6.txt"
PS C:\WINDOWS\system32> md -path "C:\mount_Win10_Pro_x32"
PS C:\WINDOWS\system32> Dism /mount-image /imagefile:"C:\mount\sources\install.esd" /index:6 /mountdir:"C:\mount_Win10_Pro_x32"
PS C:\WINDOWS\system32> Dism /get-mountedwiminfo

https://learn.microsoft.com/en-us/windows/release-health/windows11-release-information
24H2 	General Availability Channel 	2024-10-01 	2026-10-13 	2027-10-12 	2025-06 D 	2025-06-26 	26100.4484

https://learn.microsoft.com/en-us/windows/release-health/release-information
22H2 	General Availability Channel 	2022-10-18 	2025-10-14 	2025-10-14 	2025-06 D 	2025-06-24 	19045.6036
2021 (21H2) 	Long-Term Servicing Channel (LTSC) 	2021-11-16 	2027-01-12 	2032-01-13 (IoT Enterprise only)1 	2025-06 B 	2025-06-10 	19044.5965


Version 22H2 (OS build 19045)

To update devices running Windows 10, version 20H2 or 21H2 to version 22H2, you can speed up the update process using an enablement package.

General Availability Channel 	2025-06 D 	2025-06-24 	19045.6036 	KB5061087
General Availability Channel 	2025-06 OOB 	2025-06-16 	19045.5968 	KB5063159
General Availability Channel 	2025-06 B 	2025-06-10 	19045.5965 	KB5060533

Clean drivers 

https://www.tenforums.com/performance-maintenance/149630-old-devices-driver-cleanup-one-command.html

rundll32.exe pnpclean.dll,RunDLL_PnpClean /DRIVERS /MAXCLEAN’


https://www.uwe-sieber.de/misc_tools_e.html


https://www.deploymentresearch.com/inside-the-hack-that-fixes-most-of-the-windows-10-build-10122-upgrade-issues/



https://techcommunity.microsoft.com/discussions/windowsinsiderprogram/how-to-remove-oem-drivers-causing-memory-integrity-problems-/3955127



List installed drivers

PS C:\WINDOWS\system32> pnputil.exe -e | Out-File -FilePath "C:\Users\Admin1\Documents\Drivers\installdrivers.txt"

Delete driver
pnputil.exe -f -d oem<number>.inf (replace <number> with the actual number from the .inf file) and press Enter. 
The -f flag forces deletion, and -d deletes the package



Remove drivers from an offline image.

Mount image
dism /image:<mounted_image_path> /remove-driver /driver:<driver_path>
Unmount image


https://github.com/lostindark/DriverStoreExplorer/releases/tag/v0.11.92

https://about.signpath.io/product/editions

Dism /Image:"C:\mount_Win10_Pro_x32" /Get-Drivers
Image Version: 10.0.19045.3803

PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_Pro_x32" /Get-Drivers | Out-File -FilePath "C:\Users\Admin1\Documents\Drivers\DriverStoreDrivers.txt"

Using Driver Store RAPR to manage offline driver stores.
Mount offline Image and navigate to C:\mount_Win10_Pro_x32\Windows\System32\DriverStore\FileRepository and then select the folder.
If you get an error message run the following command in a new powershell window.

If you see this in the text file, the driver store is currently locked.

Error: 183

The specified image is currently being serviced by another DISM operation.
Wait for the existing DISM operation to complete, and then try the operation again.




%SYSTEMDRIVE%\Windows\System32\DriverStore


https://learn.microsoft.com/en-us/windows-hardware/drivers/install/driver-store
Before a driver package is staged to the Driver Store, the operating system first verifies that the driver package is trusted. In order for the driver package to be considered trusted, the INF file must have a CatalogFile directive in the Version section that provides the file name for a catalog file that is associated with the INF file. The catalog file must contain hashes for the INF file and any files referenced in the INF file. The catalog file must be signed with a trusted digital signature.


https://learn.microsoft.com/en-us/powershell/module/dism/get-windowsdriver?view=windowsserver2025-ps
Get-WindowsDriver -Online -All

Get-WindowsDriver -Path "c:\offline"

This command will output a list of all installed drivers, including their names, providers, and other relevant information. You can further refine the output by specifying properties to display,
Get-WindowsDriver -Online -All | Select-Object ProviderName, OriginalFileName, PublishedName


To get a list of drivers that are not installed but available for installation, you can use the -Offline switch with Get-WindowsDriver and specify the path to the driver store or an offline image.

Get-WindowsDriver -Offline -All | Select-Object ProviderName, OriginalFileName, PublishedName

https://www.reddit.com/r/PowerShell/comments/1eg15gt/using_getwindowsdriver_to_find_multiple_drivers/

https://www.ntlite.com/

https://www.foxdeploy.com/blog/using-powershell-to-find-drivers-for-device-manager.html

https://www.deploymentresearch.com/back-to-basics-finding-lenovo-drivers-and-certify-hardware-control-freak-style/


https://learn.microsoft.com/en-us/windows/win32/cimwin32prov/win32-pnpentity

https://learn.microsoft.com/en-us/windows-hardware/drivers/bringup/device-management-namespace-objects
Win32_PNPEntity
ACPI 	Advanced Configuration and Power Interface
UEFI 	Unified Extensible Firmware Interface
ROOT 	ROOT
SWD 	Root Switched Device
PCI		Peripheral Component Interconnect


Get-WmiObject Win32_PNPEntity | Select Name, DeviceID, DevMgrName, LikelyName | Out-File -FilePath "C:\Users\Admin1\Documents\Drivers\InstalledDrivers.txt"

Get-WmiObject Win32_PNPEntity | Select InstallDate, DeviceID, Name, DevMgrName, LikelyName | Export-CSV "C:\Users\Admin1\Documents\Drivers\InstalledDrivers.csv"

Get-WmiObject Win32_PNPEntity | Select * | Export-CSV "C:\Users\Admin1\Documents\Drivers\InstalledDrivers.csv"

Get-WmiObject Win32_PNPEntity | Select __RELPATH

Get-WmiObject Win32_PNPEntity | Select __PATH

The Driver Store is located at C:\Windows\System32\DriverStore\FileRepository
You can also view the drivers in the driver store by using the pnputil.exe /e command

pnputil.exe /e 
Published name :            oem175.inf

Get-WindowsDriver -Online -All 
Driver           : 1394.inf

Driver           : acpi.inf
OriginalFileName : C:\Windows\System32\DriverStore\FileRepository\acpi.inf_amd64_5859aee69503742c\acpi.inf
Inbox            : True
ClassName        : System
BootCritical     : True
ProviderName     : Microsoft
Date             : 21/06/2006 00:00:00
Version          : 10.0.26100.4202


24H2 	Hudson Valley[citation needed] 	2024 Update 	26100 	Release Date October 1, 2024

"C:\mount_Win10_Pro_x32"
Get-WindowsPackage -Path "C:\mount_Win10_Pro_x32" | Format-List | Export-CSV "C:\Users\Admin1\Documents\Drivers\OfflinePackages.csv"
Get-WindowsPackage -Path "C:\mount_Win10_Pro_x32" | Format-List | Out-File -FilePath "C:\Users\Admin1\Documents\Drivers\OfflinePackages.txt"

Install-Module PSWindowsUpdate
Get-WUList -WindowsUpdate -ComputerName "C:\mount_Win10_Pro_x32" -Verbose

Dism /Image:"C:\mount_Win10_Pro_x32" /Get-Packages /Format:Table

https://forums.mydigitallife.net/threads/windows-10-hotfix-repository.57050/page-729


https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/repair-a-windows-image?view=windows-11

Dism /Image:"C:\mount_Win10_Pro_x32" /Cleanup-Image /CheckHealth
Dism /Image:"C:\mount_Win10_Pro_x32" /Cleanup-Image /ScanHealth

https://learn.microsoft.com/en-us/windows/release-health/release-information
General Availability Channel 	2023-12 B 	2023-12-12 	19045.3803 	KB5033372

#Pull out specific values for VendorID and DeviceID, from the objects in $Unknown_dev
            $vendorID = ($device.DeviceID | Select-String -Pattern 'VEN_....' | select -expand Matches | select -expand Value) -replace 'VEN_',''
            $deviceID = ($device.DeviceID | Select-String -Pattern 'DEV_....' | select -expand Matches | select -expand Value) -replace 'DEV_',''
			
			
Get-WmiObject Win32_PNPEntity | Where-Object{$_.ConfigManagerErrorCode -ne 0} | Select Name, DeviceID

Get-WmiObject Win32_PNPEntity | Where-Object{$_.ConfigManagerErrorCode -ne 0} | Select Name, DeviceID | Export-CSV C:\Drivers.csv

Get-WmiObject Win32_PNPEntity | Where-Object{$_.Name -Match "VGA"} | Select Name, DeviceID 

ACPI\LEN0068
https://www.catalog.update.microsoft.com/Search.aspx?q=ACPI%5CLEN0068

Get-WmiObject Win32_ComputerSystem | Select Model

PS C:\WINDOWS\system32> Get-WmiObject Win32_ComputerSystem | Select Model

Model
-----
82XB

https://github.com/1RedOne/Get-UnknownDevices
VendorID DeviceID DevMgrName LikelyName


https://tekcookie.com/auto-install-drivers-using-powershell/
# Mount the driver iso image
Mount-DiskImage D:\Driver\drivers-windows.iso

# Get the mount point/drive letter, considering that the above one is the only disk mounted
$isoMount = (Get-DiskImage -DevicePath \\.\CDROM0  | Get-Volume).DriveLetter

# Find the inf files and install
Get-ChildItem "$($isoMount):\" -Recurse -Include *.inf | ForEach-Object {
     $_.FullName
     pnputil /add-driver $_.FullName /install 
}



https://pureinfotech.com/restore-registry-backup-windows-10/
