PS C:\WINDOWS\system32> $DiskImageResult = Mount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32.iso"
PS C:\WINDOWS\system32> $DiskImageDriveLetter = ($DiskImageResult | Get-Volume).DriveLetter
PS C:\WINDOWS\system32> Copy-Item -Path "$($DiskImageDriveLetter):\*" -Destination "C:\mount" -Recurse



EDIT C:\mount, ie add AutoUnattend.xml

Open Deployment and Imaging Tools Environment Command Window

C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools> oscdimg -m -o -u2 -udfver102 -bootdata:2#p0,e,bc:\mount\boot\etfsboot.com#pEF,e,bc:\mount\efi\microsoft\boot\efisys_noprompt.bin "c:\mount" "c:\Users\Admin1\Documents\ISO Files\AU_Win10_22H2_x32.iso"

VMware Workstation 17.6.3 build-24583834 Windows 10 22H2_x32.
If the VMware Workstation defaults to a NVMe Hard Disk, edit the Hardware, add a new SATA Hard disk of the same size (default 60GB), and then remove the NVMe Hard Disk. The NVMe Hard Disk can cause BSOD kmode_excepton_not_handled early in the installation process (Getting files ready for installation 0%) and the SATA hard disk (driver) is the work around.





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








