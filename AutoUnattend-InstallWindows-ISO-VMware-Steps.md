PS C:\WINDOWS\system32> $DiskImageResult = Mount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32.iso"
PS C:\WINDOWS\system32> $DiskImageDriveLetter = ($DiskImageResult | Get-Volume).DriveLetter
PS C:\WINDOWS\system32> md -path "C:\mount"
PS C:\WINDOWS\system32> Copy-Item -Path "$($DiskImageDriveLetter):\*" -Destination "C:\mount" -Recurse
PS C:\WINDOWS\system32> Dismount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32.iso"

Remove the ReadOnly attribute from all the files from the ISO file.

PS C:\WINDOWS\system32> Get-ChildItem "C:\mount\*" -Recurse -File -Force | foreach { Set-ItemProperty -Path $_.FullName -Name IsReadOnly -Value $false } 



EDIT C:\mount, ie add AutoUnattend.xml, mount install.esd image, establish index number of the Windows variant you want to install, add drivers, add windows update packages. 

PS C:\WINDOWS\system32> Copy "C:\Users\Admin1\Documents\ISO Files\AutoUnAttend.xml" "C:\mount\AutoUnAttend.xml"

Load Windows 10 Pro x32 and add packages.
PS C:\WINDOWS\system32> Set-ItemProperty "C:\mount\sources\install.esd" -name IsReadOnly -value $false
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount\sources\install.esd" | Out-File -FilePath "C:\mount\sources\install.esd.txt"
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount\sources\install.esd" /index:6 | Out-File -FilePath "C:\mount\sources\install.esd.6.txt"
PS C:\WINDOWS\system32> md -path "C:\mount_Win10_Pro_x32"
PS C:\WINDOWS\system32> Dism /mount-image /imagefile:"C:\mount\sources\install.esd" /index:6 /mountdir:"C:\mount_Win10_Pro_x32"
PS C:\WINDOWS\system32> Dism /get-mountedwiminfo
PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_Pro_x32" /Add-Driver /Driver:"C:\Users\Admin1\Documents\ISO Files\VMware Tools 12.4.5" /Recurse /ForceUnsigned

Optionally perform the next two steps and bypass the Windows Package Update.

PS C:\WINDOWS\system32> Dism /unmount-image /mountdir:"C:\mount_Win10_Pro_x32" /commit

Open Deployment and Imaging Tools Environment Command Window

C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools> oscdimg -m -o -u2 -udfver102 -bootdata:2#p0,e,bc:\mount\boot\etfsboot.com#pEF,e,bc:\mount\efi\microsoft\boot\efisys_noprompt.bin "c:\mount" "c:\Users\Admin1\Documents\ISO Files\AU_Win10_22H2_x32.iso"
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount\sources\install.esd"
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount\sources\install.esd" /index:6
PS C:\WINDOWS\system32> Dism /mount-image /imagefile:"C:\mount\sources\install.esd" /index:6 /mountdir:"C:\mount_Win10_Pro_x32"
PS C:\WINDOWS\system32> Dism /get-mountedwiminfo

Optional incase of problems
PS C:\WINDOWS\system32> dism /remount-image /MountDir:"C:\mount_Win10_Pro_x32"


Servicing Stacks Updates (SSU) need to be updated before Latest Cumulative Update (LCU). 
SSU's are not released every month so trawl backwards from the current year & month until you can find a SSU, then see what the documentation says.
Also check what version the Image file is. You can get this by using the following command:
Dism /Get-ImageInfo /ImageFile:"C:\mount\sources\install.esd" /index:6


PS C:\WINDOWS\system32> md -path "C:\mount_kb5060533"
  
Download the 2025-06 Cumulative Update for Windows 10 Version 22H2 for x86-based Systems (KB5060533) classified as a Security Update.

Security Updates install into offline (not running) windows image files.
https://www.catalog.update.microsoft.com/Search.aspx?q=windows%2010%20pro%2022h2%20x86%20security%20updates%202025-06

Standard Updates dont tend to install properly.
https://www.catalog.update.microsoft.com/Search.aspx?q=windows%2010%20pro%2022h2%20x86%202025-06


PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_Pro_x32" /Add-Package /PackagePath:"C:\Users\Admin1\Documents\ISO Files\ssu-19041.3562-x86_5757db67f982216ee2f5973f4b3cfddbcae916b7.msu"

PS C:\WINDOWS\system32> New-Item -ItemType Directory -Force -Path "C:\temp"
PS C:\WINDOWS\system32> Dism /Cleanup-Image /Image:"C:\mount_Win10_Pro_x32" /StartComponentCleanup /Resetbase /ScratchDir:"C:\temp"

PS C:\WINDOWS\system32> Dism /unmount-image /mountdir:"C:\mount_Win10_Pro_x32" /commit


Direct Link to download the KB5060533 MSU.
https://catalog.s.download.windowsupdate.com/d/msdownload/update/software/secu/2025/06/windows10.0-kb5060533-x86_de4a47dde17d91023f93eb9a37c6c96faebf768c.msu


PS C:\WINDOWS\system32> md -path "C:\mount_kb5060533"
PS C:\WINDOWS\system32> expand -F:* "C:\Users\Admin1\Documents\ISO Files\windows10.0-kb5060533-x86_de4a47dde17d91023f93eb9a37c6c96faebf768c.msu" "C:\mount_kb5060533"

PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_Pro_x32" /Add-Package /PackagePath:"C:\mount_kb5060533\Windows10.0-KB5060533-x86.cab"

HRESULT = 0x800f0823 - CBS_E_NEW_SERVICING_STACK_REQUIRED

Invoke-WebRequest -Uri "http://www.contoso.com" -OutFile "C:\path\file" 

Alter the search date (YYYY-MM) and click the link for the "YYYY-MM Cumulative Update for Windows 10 Version 22H2 for x86-based Systems (KBnnnnnnn)"
Make a note of the KB number and add the number to the web address https://support.microsoft.com/help/nnnnnnn, scroll down and look for Update Catalog, click on it and check to make sure it can be it can be added as a stand alone package. If it can, down the update and install it using the command below. For CAB files, use the 2nd command below.
Dism /Image:"C:\Path\To\Mounted\Install.esd" /Add-Package /PackagePath:"C:\Path\To\WindowsUpdatePackagename.msu"
Dism /Image:"C:\Path\To\Mounted\Install.esd" /Add-Package /PackagePath:"C:\Path\To\WindowsUpdatePackagename.cab"


PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount\sources\install.esd" /index:6
PS C:\WINDOWS\system32> Dism /mount-image /imagefile:"C:\mount\sources\install.esd" /index:6 /mountdir:"C:\mount_Win10_Pro_x32"
PS C:\WINDOWS\system32> Dism /get-mountedwiminfo

PS C:\WINDOWS\system32> Dism /Image:"C:\mount_Win10_Pro_x32" /Add-Package /PackagePath:"C:\Users\Admin1\Documents\ISO Files\windows10.0-kb5060533-x86_de4a47dde17d91023f93eb9a37c6c96faebf768c.msu"


PS C:\WINDOWS\system32> New-Item -ItemType Directory -Force -Path "C:\temp"
PS C:\WINDOWS\system32> Dism /Cleanup-Image /Image:"C:\mount_Win10_Pro_x32" /StartComponentCleanup /Resetbase /ScratchDir:"C:\temp"

PS C:\WINDOWS\system32> Dism /unmount-image /mountdir:"C:\mount_Win10_Pro_x32" /commit


https://knowledge.broadcom.com/external/article?articleNumber=315363
https://techdocs.broadcom.com/us/en/vmware-cis/vsphere/tools/12-4-0/vmware-tools-administration-12-4-0.html



Open Deployment and Imaging Tools Environment Command Window

C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools> oscdimg -m -o -u2 -udfver102 -bootdata:2#p0,e,bc:\mount\boot\etfsboot.com#pEF,e,bc:\mount\efi\microsoft\boot\efisys_noprompt.bin "c:\mount" "c:\Users\Admin1\Documents\ISO Files\AU_Win10_22H2_x32.iso"

VMware Workstation 17.6.3 build-24583834 Windows 10 22H2_x32.
If the VMware Workstation defaults to a NVMe Hard Disk, edit the Hardware, add a new SATA Hard disk of the same size (default 60GB), and then remove the NVMe Hard Disk. The NVMe Hard Disk can cause BSOD kmode_excepton_not_handled early in the installation process (Getting files ready for installation 0%) and the SATA hard disk (driver) is the work around.

Install VMware Tools 12.4.5.23787635 driver packages 

To extract the VMware Tools to a folder in order to add to the Windows 10 Image
setup.exe /A C:\VMwareTools

Dism /Mount-Image /ImageFile:C:\test\images\install.wim /Index:<image_index>  /MountDir:C:\mount

Dism /Image:C:\test\offline /Add-Driver /Driver:C:\drivers\mydriver.inf

Dism /Image:C:\test\offline /Add-Driver /Driver:C:\VMwareTools /Recurse

Dism /Image:C:\test\offline /Add-Driver /Driver:C:\drivers\unsigned_driver.inf /ForceUnsigned

Dism /Unmount-Image /MountDir:C:\test\offline /Commit





win10 kmode_excepton_not_handled during installation vmware workstation


https://www.reddit.com/r/vmware/comments/1ht0t4e/windows_10_x32_in_vmware_17/

VMware Workstation 

C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools\amd64\Oscdimg


https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/bcdboot-command-line-options-techref-di?view=windows-11

https://learn.microsoft.com/en-us/troubleshoot/windows-server/setup-upgrade-and-drivers/create-iso-image-for-uefi-platforms#oscdimg-command-arguments

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/oscdimg-command-line-options?view=windows-11

oscdimg -m -o -u2 -udfver102 -bootdata:2#p0,e,bc:\winpe_x64\etfsboot.com#pEF,e,bc:\winpe_x64\efisys.bin c:\winpe_x64\ISO c:\winpe_x64\winpeuefi.iso

-m 			Ignores the maximum size limit of the image.
-o 			Optimizes storage by encoding duplicate files only one time.
-u1			Produces an image that has both the UDF file system and the ISO 9660 file system. 
			The ISO 9660 file system is written by using DOS-compatible 8.3 file names. 
			The UDF file system is written by using Unicode file names.
-u2 		Produces an ISO image that has only the Universal Disk Format (UDF) file system on it.
-udfver102 	Specifies the UDF version 1.02 format.
-bootdata 	Specifies a multiboot image. This image uses an x86-based boot sector as the default image. 
			This sector starts the Etfsboot.com boot code. 
			A secondary EFI boot image starts an EFI boot application.
Single Boot Entries
-p			Specifies the value to use for the platform ID in the El Torito catalog. 
			The default ID is 0xEF to represent a Unified Extensible Firmware Interface (UEFI) system. 
			0x00 represents a BIOS system.
-b 			Specifies the El Torito boot sector file that will be written in the boot sector or sectors of the disk. Do not use spaces. For example:
			On UEFI: -bC:\winpe_x86\Efisys.bin
			On BIOS: -bC:\winpe_x86\Etfsboot.com
Multi Boot Entries
-bootdata:<number>		Specifies a multi-boot image, followed by the number of boot entries. Do not use spaces. For example:
						-bootdata:<3>#<defaultBootEntry>#<bootEntry1>#<bootEntryN>
						where <3> is the number of boot entries that follow.
p						Specifies the value to use for the platform ID in the El Torito catalog. 
						The default ID is 0xEF to represent a UEFI system. 
						0x00 represents a BIOS system.
e						Disables floppy disk emulation in the El Torito catalog.
b<bootSectorFile>		Specifies the El Torito boot sector file that will be written in the boot sector or sectors of the disk. 
						Do not use spaces. For example:
						On UEFI: bEfisys.bin
						On BIOS: bEtfsboot.com
t						Specifies the El Torito load segment. If not specified, this option defaults to 0x7C0.
			
<sourceLocation>	Required. Specifies the location of the files that you intend to build into an .iso image.
<targetFile>		Specifies the name of the .iso image file. This probably should be required.

c:\winpe_x64\ISO			Represents the path of the files for the image.
c:\winpe_x64\winpeuefi.iso	Represents the output image file.


			
-bootdata:2 	Two Boot Partitions
#			
p0,				PlatformX86 = 0 	https://learn.microsoft.com/en-us/windows/win32/api/imapi2fs/ne-imapi2fs-platformid
e,									https://learn.microsoft.com/en-us/windows/win32/api/imapi2fs/ne-imapi2fs-emulationtype
bc:\winpe_x64\etfsboot.com
bc:\mount\boot\etfsboot.com
#
pEF,			PlatformEFI = 0xef 	https://learn.microsoft.com/en-us/windows/win32/api/imapi2fs/ne-imapi2fs-platformid
e,									https://learn.microsoft.com/en-us/windows/win32/api/imapi2fs/ne-imapi2fs-emulationtype
bc:\winpe_x64\efisys.bin
bc:\mount\efi\microsoft\boot\efisys_noprompt.bin

			

PS C:\WINDOWS\system32> Dismount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32.iso"

PS C:\WINDOWS\system32> import-Module "C:\Users\Admin1\Documents\ISO Files\New-ISOFile.psm1"

VMware - UEFI boot - No Prompt to press any key to boot
$boot.PlatformId 	= 239
$boot.Emulation		= 2
PS C:\WINDOWS\system32> New-ISOFile -source "C:\mount" -destinationISO "C:\Users\Admin1\Documents\ISO Files\AU_Win10_22H2_x32.iso" -bootfile "C:\mount\efi\microsoft\boot\efisys_noprompt.bin" -title "AU_Win10_22H2_x32" -verbose

VMware - UEFI boot - Shows Prompt to press any key to boot
PS C:\WINDOWS\system32> New-ISOFile -source "C:\mount" -destinationISO "C:\Users\Admin1\Documents\ISO Files\AU_Win10_22H2_x32.iso" -bootfile "C:\mount\efi\microsoft\boot\efisys.bin" -title "AU_Win10_22H2_x32" -verbose

VMware - Bios boot 
PS C:\WINDOWS\system32> New-ISOFile -source "C:\mount" -destinationISO "C:\Users\Admin1\Documents\ISO Files\AU_Win10_22H2_x32.iso" -bootfile "C:\mount\efi\microsoft\boot\cdboot_noprompt.efi" -title "AU_Win10_22H2_x32" -verbose




New-ISOFile source destinationISO <bootfile> <media> <title> <force>

-source = "C:\mount"
-destinationISO = "C:\Users\Admin1\Documents\ISO Files\AU_Win10_22H2_x32.iso"
-bootfile = "C:\mount\efi\microsoft\boot\efisys_noprompt.bin"
-title = "AU_Win10_22H2_x32"


https://learn.microsoft.com/en-us/troubleshoot/windows-server/setup-upgrade-and-drivers/create-iso-image-for-uefi-platforms

https://github.com/druuu/files/tree/master/dual_boot.bkp/boot/EFI/Microsoft/Boot

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/bcd-system-store-settings-for-uefi?view=windows-11


.PARAMETER source
        The source folder to add to the ISO.

    .PARAMETER destinationIso
        The ISO file to create.

    .PARAMETER bootFile
        Optional. Boot file to add to the ISO.

    .PARAMETER media
        Optional. The media type of the resulting ISO (BDR, CDR etc). Defaults to DVDPLUSRW_DUALLAYER.

    .PARAMETER title
        Optional. Title of the ISO file. Defaults to "untitled".

    .PARAMETER force
        Optional. Force overwrite of an existing ISO file.



Open Deployment and Imaging Tools Environment

```
Makewinpemedia /iso C:\mount C:\Users\Admin1\Documents\ISO Files\AU_Win10_22H2_x32.iso
```








