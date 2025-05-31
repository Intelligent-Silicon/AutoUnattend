# windowsPE Component
# Microsoft-Windows-Setup

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup

This is the section where you can choose the version of windows you want to install, configure the partition drives on the hard disk and configure the windowsPE operating system further.

### ```<ImageInstall>```

```<OSImage>``` specifies the path and the destination of a Windows image (.wim) file that contains the image to install.

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage


```<Compact>``` Compact OS allows you to run the operating system from compressed files. This will result in a slower experience because files have to be decompressed before they can be used, but enables devices with small storage drives to run windows from. Works best with a fast cpu and plenty of fast ram.



https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/compact-os

```<InstallTo>``` specifies the disk and partition where you install the Windows operating system image, requires ```<DiskID>``` and ```<PartitionID>```. See next sections below.

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installto

```<DiskID>``` is the hard drive found on the device. Drive numbers start at 0 and increment by 1. Typically this will be drive 0 for most computers unless you have added additional hard drives and want to install windows on a specific drive. To find out what your computer has, open up a dos command window, diskpart, list disk, exit.

```
DISKPART> list disk

  Disk ###  Status         Size     Free     Dyn  Gpt
  --------  -------------  -------  -------  ---  ---
  Disk 0    Online          119 GB  1024 KB        *

DISKPART> exit
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

DISKPART> exit
```

```
<ImageInstall>
	<OSImage>
		<Compact>false</Compact> 
		<InstallTo>
			<DiskID>0</DiskID>
			<PartitionID>3</PartitionID>
		</InstallTo>
	</OSImage>
</ImageInstall>
```

### ```<UserData>```

This is the section where you specify the user settings to install the version of windows. 

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-userdata

```<ProductKey>``` is the section where you choose what edition of windows to install based on the 25 characters generic key supplied, or you can pull the licence key from the UEFI bios using powershell. Use Powershell running elevated as administrator and type at the powershell prompt:
```
PS C:\Users\SysOps> (Get-WmiObject -query 'select * from SoftwareLicensingService').OA3xOriginalProductKey
ABCDE-FGHIJ-KLMNO-PQRST-UVWXY
PS C:\Users\SysOps>
```

[Generic Windows product keys](https://www.elevenforum.com/t/generic-product-keys-to-install-or-upgrade-windows-11-editions.3713/)

You can use Windows without activation for up to 30 days, but then you'll encounter some restrictions. This should give you plenty of time to evaluate different versions of windows using the generic windows product keys in the link above.

```<WillShowUI>``` specifies when the Windows Installation User Interface (UI) is displayed. Options include Always, OnError, Never. 

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-willshowui

```<AcceptEula>``` specifies whether to automatically accept the Microsoft Software License Terms aka the End User Licence Agreement (EULA). Options are true and false.

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

### ```<UseConfigurationSet>```

```<UseConfigurationSet>``` sets whether to use a configuration set or not. Options are true or false, the latter being the default option.
A configuration set is a variable called ```%configsetroot%``` that enables you to refer to the root drive/folder of the USB memory stick, for such things like additional drivers, packages and/or software. This is useful and required for installing in offline circumstances.

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-useconfigurationset

```
<UseConfigurationSet>true</UseConfigurationSet>
```

### ```<RunSynchronousCommand>```

```<RunSynchronousCommand>``` is where you can run additional commands or scripts to perform additional functions. These run one after the after unlike ```<RunAsynchronousCommand>``` which run all at the same time and cant be relied upon by commands in later orders. These run as User in the auditUser pass and as System in the specialise pass.

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-deployment-runsynchronous-runsynchronouscommand


These examples create hard drive partitions. When copying commands that use symbols like >> encoding them as ```&gt;``` reduces errors with commands when stored in a file.

```
&lt; stands for the less-than sign: <
&gt; stands for the greater-than sign: >
&le; stands for the less-than or equals sign: ≤
&ge; stands for the greater-than or equals sign: ≥
&amp; stands for the ampersand sign: &
```


To quick wipe the hard drive keep ```QUICK``` in the partition commands eg ```FORMAT QUICK FS=FAT32```.
To wipe the hard drive and wipe every sector which will take longer, anything from 10-15minutes to hours depending on disk size and if its a slow spin disk or not, remove ```QUICK``` from the partition commands eg ```FORMAT FS=FAT32``` 

If you have been hacked, removing the ```QUICK``` option is generally the best as it will wipe malware stored on the drive that is not removed by simply removing the index of disk files which is what the ```QUICK``` does.

This example creates the following:
```
Order 1
Partition 1    System             300 MB
Partition 2    Reserved            16 MB
Order 2
Partition 3    Primary            118 GB
Order 3
diskpart /s X:\diskpart.txt > X:\diskpart.log
```
The Windows RE (Recovery Environment) partition is installed in C:\Recovery and no recovery partition will be created.

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

This example creates the following:
```
Order 1
Partition 1    System             300 MB FAT32
Partition 2    Reserved            16 MB FAT32
Order 2
Partition 3    Primary            118 GB NTFS	Windows
Order 3
Partition 4    Recovery           651 MB FAT32
Order 4
diskpart /s X:\diskpart.txt > X:\diskpart.log
```

The Windows RE (Recovery Environment) partition is installed in a seperate partition categorized as Recovery. 

For more information see https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/configure-uefigpt-based-hard-drive-partitions

Also note this example is not using any encoding for symbols, but would need to be encoded if saved to an autounattend.xml file.

```
<RunSynchronousCommand wcm:action="add">
	<Order>1</Order>
	<Path>cmd.exe /c ">>"X:\diskpart.txt" (echo SELECT DISK=0&echo CLEAN&echo CONVERT GPT&echo CREATE PARTITION EFI SIZE=300&echo FORMAT QUICK FS=FAT32 LABEL="System"&echo CREATE PARTITION MSR SIZE=16)"
	</Path>
</RunSynchronousCommand>
<RunSynchronousCommand wcm:action="add">
	<Order>2</Order>
	<Path>cmd.exe /c ">>"X:\diskpart.txt" (echo CREATE PARTITION PRIMARY&echo SHRINK MINIMUM=1000&echo FORMAT QUICK FS=NTFS LABEL="Windows"&echo CREATE PARTITION PRIMARY&echo FORMAT QUICK FS=NTFS LABEL="Recovery")"
	</Path>
</RunSynchronousCommand>
<RunSynchronousCommand wcm:action="add">
	<Order>3</Order>
	<Path>cmd.exe /c ">>"X:\diskpart.txt" (echo SET ID="de94bba4-06d1-4d40-a16a-bfd50179d6ac"&echo GPT ATTRIBUTES=0x8000000000000001)"
	</Path>
</RunSynchronousCommand>
<RunSynchronousCommand wcm:action="add">
	<Order>4</Order>
	<Path>cmd.exe /c "diskpart.exe /s "X:\diskpart.txt" >>"X:\diskpart.log" || ( type "X:\diskpart.log" & echo diskpart encountered an error. & pause & exit /b 1 )"
	</Path>
</RunSynchronousCommand>
```

This example uses ```<DiskConfiguration>``` to create the partitions and assumes a UEFI bios exists and not the older bios type:

Create partitions first, then modify the required partitions. Its unclear from the Microsoft docs if this is a sector level data wipe or index only wipe, so the examples above using a script command is safer to use for a sector level wipe by removing the ```QUICK``` attribute.

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-createpartitions-createpartition-type

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-modifypartitions-modifypartition-typeid

```
Order 1
Partition 1    System             300 MB FAT32
Order 2
Partition 2    Reserved            16 MB FAT32
Order 3
Partition 3    Primary            118 GB NTFS	Windows
Order 3
Partition 4    Recovery           651 MB FAT32
Order 4
diskpart /s X:\diskpart.txt > X:\diskpart.log
```

```
<Disk wcm:action="add">
	<DiskID>0</DiskID>
	<!-- Its unclear if this is a sector level wipe or just an index wipe. -->
    <WillWipeDisk>true</WillWipeDisk>
	
	<CreatePartitions>
	
		<!-- System -->
		<CreatePartition wcm:action="add">
			<Order>1</Order> 
			<Type>EFI</Type> 
			<Size>300</Size> 
		</CreatePartition>

		<!-- Reserved - Docs suggest minimum size of 32MB -->
		<CreatePartition wcm:action="add">
			<Order>2</Order> 
			<Type>MSR</Type>
			<Size>16</Size>		 
		</CreatePartition>
		
		<!-- Primary In MB. 1GB = 1000MB -->
		<CreatePartition wcm:action="add">
			<Order>3</Order> 
			<Type>Primary</Type>
			<Size>118000</Size>		 
		</CreatePartition>
		
		<!-- Recovery using <ModifyPartitions> below -->
		<CreatePartition wcm:action="add">
			<Order>4</Order> 
			<Type>Primary</Type>
			<Size>651</Size>		 
		</CreatePartition>

    </CreatePartitions>
	
    <ModifyPartitions>

		<!-- EFI -->
		<ModifyPartition wcm:action="add">
			<Order>1</Order> 
			<PartitionID>1</PartitionID> 
			<Format>FAT32</Format> 
		</ModifyPartition>
		
		<!-- MSR -->
		<ModifyPartition wcm:action="add">
			<Order>2</Order> 
			<PartitionID>2</PartitionID> 
			<Format>FAT32</Format> 
		</ModifyPartition>

		<!-- Windows partition -->
		<ModifyPartition wcm:action="add">
			<Order>3</Order> 
			<PartitionID>3</PartitionID> 
			<Label>Windows</Label> 
			<Letter>C</Letter> 
			<Format>NTFS</Format> 
		</ModifyPartition>
		
		<!-- Recovery -->
		<ModifyPartition wcm:action="add">
			<Order>4</Order> 
			<PartitionID>4</PartitionID> 
			<TypeID>de94bba4-06d1-4d40-a16a-bfd50179d6ac</TypeID>
			<Format>FAT32</Format>		
		</ModifyPartition>
	</ModifyPartitions>
</Disk>
```