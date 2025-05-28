# windowsPE Component
# Microsoft-Windows-Setup

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup

This is the section where you can choose the version of windows you want to install, configure the partition drives on the hard disk and configure the windowsPE operating system further.

### ```<ImageInstall>```

```<OSImage>``` specifies the path and the destination of a Windows image (.wim) file that contains the image to install.

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage


```<Compact>``` Compact OS allows you to run the operating system from compressed files. This will result in a slower experience because files have to be decompressed before they can be used, but enables devices with small storage drives to run windows from. Works best with a fast cpu and plenty of fast ram.



https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/compact-os

```<InstallTo>``` specifies the disk and partition where you install the Windows operating system image.

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installto

```<DiskID>``` is the hard drive found on the device. Drive numbers start at 0 and increment by 1. Typically this will be drive 0 for most computers unless you have added additional hard drives and want to install windows on a specific drive. To find out what your computer has, open up a dos command window, diskpart, list disk, exit.

```
DISKPART> list disk

  Disk ###  Status         Size     Free     Dyn  Gpt
  --------  -------------  -------  -------  ---  ---
  Disk 0    Online          119 GB  1024 KB        *

DISKPART>
```

```<PartitionID>``` specifies the identification number of the partition to modify. The first partition on a disk has the value of 1, the second, 2, and so on. If you have a typical single hard drive, it will typically look like the partition list below. Primary is where you want to install windows onto.

```
DISKPART> list disk

  Disk ###  Status         Size     Free     Dyn  Gpt
  --------  -------------  -------  -------  ---  ---
  Disk 0    Online          119 GB  1024 KB        *

DISKPART> select disk 0

Disk 0 is now the selected disk.

DISKPART> list partition

  Partition ###  Type              Size     Offset
  -------------  ----------------  -------  -------
  Partition 1    System             300 MB  1024 KB
  Partition 2    Reserved            16 MB   301 MB
  Partition 3    Primary            118 GB   317 MB
  Partition 4    Recovery           651 MB   118 GB

DISKPART>exit
```

```
<ImageInstall>
	<OSImage>
		<Compact>false</Compact> /// Use CompactOS, Options = true or false. If <Compact></Compact>, windowsPE decides.
		<InstallTo>
			<DiskID>0</DiskID> /// Disk 0 is the first drive, and partition 3 is the 2nd partition on the hard drive. Partition 1 during installation is the installation partition.
			<PartitionID>3</PartitionID>
		</InstallTo>
	</OSImage>
</ImageInstall>
```

### <UserData>

This is the section where you specify the user settings to install the version of windows. 

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-userdata

<ProductKey> is the section where you choose what edition of windows to install based on the 25 characters generic key supplied, or you can pull the licence key from the UEFI bios using powershell. Use Powershell running elevated as administrator and type at the powershell prompt:
```
PS C:\Users\SysOps> (Get-WmiObject -query 'select * from SoftwareLicensingService').OA3xOriginalProductKey
ABCDE-FGHIJ-KLMNO-PQRST-UVWXY
PS C:\Users\SysOps>
```

[Generic Windows product keys](https://www.elevenforum.com/t/generic-product-keys-to-install-or-upgrade-windows-11-editions.3713/)

You can use Windows without activation for up to 30 days, but then you'll encounter some restrictions. This should give you plenty of time to evaluate different versions of windows using the generic windows product keys in the link above.

<WillShowUI> specifies when the Windows Installation User Interface (UI) is displayed. Options include Always, OnError, Never. 

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-willshowui

<AcceptEula> specifies whether to automatically accept the Microsoft Software License Terms aka the End User Licence Agreement (EULA). Options are true and false.

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-userdata-accepteula


```
<UserData>
	<ProductKey>
		<Key>ABCDE-FGHIJ-KLMNO-PQRST-UVWXY</Key>
		<WillShowUI>OnError</WillShowUI>
	</ProductKey>
	<AcceptEula>true</AcceptEula>
</UserData>
```

To quick wipe the hard drive keep QUICK in the partition commands eg ```FORMAT QUICK FS=FAT32```
```
<RunSynchronousCommand wcm:action="add">
	<Order>1</Order>
	<Path>cmd.exe /c "&gt;&gt;"X:\diskpart.txt" (echo SELECT DISK=0&amp;echo CLEAN&amp;echo CONVERT GPT&amp;echo CREATE PARTITION EFI SIZE=300&amp;echo FORMAT QUICK FS=FAT32 LABEL="System"&amp;echo CREATE PARTITION MSR SIZE=16)"</Path>
</RunSynchronousCommand>
<RunSynchronousCommand wcm:action="add">
	<Order>2</Order>
	<Path>cmd.exe /c "&gt;&gt;"X:\diskpart.txt" (echo CREATE PARTITION PRIMARY&amp;echo FORMAT QUICK FS=NTFS LABEL="Windows")"</Path>
</RunSynchronousCommand>
<RunSynchronousCommand wcm:action="add">
	<Order>3</Order>
	<Path>cmd.exe /c "diskpart.exe /s "X:\diskpart.txt" &gt;&gt;"X:\diskpart.log" || ( type "X:\diskpart.log" &amp; echo diskpart encountered an error. &amp; pause &amp; exit /b 1 )"</Path>
</RunSynchronousCommand>
```

To wipe the hard drive and wipe every sector which will take longer, anything from 10-15minutes to hours depending on size and if its a slow spin disk or not, remove QUICK from the partition commands eg ```FORMAT FS=FAT32``` 

If you have been hacked, this option is generally the best as it will wipe malware stored on the drive that is not removed by simply removing the index of disk files which is what the ```QUICK``` does.

```
<RunSynchronousCommand wcm:action="add">
	<Order>1</Order>
	<Path>cmd.exe /c "&gt;&gt;"X:\diskpart.txt" (echo SELECT DISK=0&amp;echo CLEAN&amp;echo CONVERT GPT&amp;echo CREATE PARTITION EFI SIZE=300&amp;echo FORMAT FS=FAT32 LABEL="System"&amp;echo CREATE PARTITION MSR SIZE=16)"</Path>
</RunSynchronousCommand>
<RunSynchronousCommand wcm:action="add">
	<Order>2</Order>
	<Path>cmd.exe /c "&gt;&gt;"X:\diskpart.txt" (echo CREATE PARTITION PRIMARY&amp;echo FORMAT FS=NTFS LABEL="Windows")"</Path>
</RunSynchronousCommand>
<RunSynchronousCommand wcm:action="add">
	<Order>3</Order>
	<Path>cmd.exe /c "diskpart.exe /s "X:\diskpart.txt" &gt;&gt;"X:\diskpart.log" || ( type "X:\diskpart.log" &amp; echo diskpart encountered an error. &amp; pause &amp; exit /b 1 )"</Path>
</RunSynchronousCommand>
```



```
<component name="Microsoft-Windows-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
	<ImageInstall>
		<OSImage>
			<Compact>false</Compact> /// Use CompactOS, Options = true or false. If <Compact></Compact>, windowsPE decides.
			<InstallTo>
			<DiskID>0</DiskID> /// Disk 0 is the first drive, and partition 3 is the 2nd partition on the hard drive. Partition 1 during installation is the installation partition.
			<PartitionID>3</PartitionID>
			</InstallTo>
		</OSImage>
	</ImageInstall>
	<UserData>
		<ProductKey>
		<Key>VK7JG-NPHTM-C97JM-9MPGT-3V66T</Key> /// To get the Windows Key from UEFI Bios use Powershell as administrator. PS>(Get-WmiObject -query 'select * from SoftwareLicensingService').OA3xOriginalProductKey
		<WillShowUI>OnError</WillShowUI> /// Always/OnError/Never https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-willshowui
		</ProductKey>
		<AcceptEula>true</AcceptEula> /// true/false https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-userdata-accepteula
	</UserData>
	<UseConfigurationSet>true</UseConfigurationSet> /// true/false https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-useconfigurationset
	<RunSynchronous> /// Run one after the other, can rely on previous steps being completed first https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-runsynchronous
		<RunSynchronousCommand wcm:action="add">
			<Order>1</Order>
			<Path>cmd.exe /c "&gt;&gt;"X:\diskpart.txt" (echo SELECT DISK=0&amp;echo CLEAN&amp;echo CONVERT GPT&amp;echo CREATE PARTITION EFI SIZE=300&amp;echo FORMAT QUICK FS=FAT32 LABEL="System"&amp;echo CREATE PARTITION MSR SIZE=16)"</Path>
		</RunSynchronousCommand>
		<RunSynchronousCommand wcm:action="add">
			<Order>2</Order>
			<Path>cmd.exe /c "&gt;&gt;"X:\diskpart.txt" (echo CREATE PARTITION PRIMARY&amp;echo FORMAT QUICK FS=NTFS LABEL="Windows")"</Path>
		</RunSynchronousCommand>
		<RunSynchronousCommand wcm:action="add">
			<Order>3</Order>
			<Path>cmd.exe /c "diskpart.exe /s "X:\diskpart.txt" &gt;&gt;"X:\diskpart.log" || ( type "X:\diskpart.log" &amp; echo diskpart encountered an error. &amp; pause &amp; exit /b 1 )"</Path>
		</RunSynchronousCommand>
	</RunSynchronous>
	<RunAsynchronous> /// Run all at the same time aka a free for all, cant not rely on previous steps being completed. https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-runasynchronous
	</RunAynchronous>
</component>
```