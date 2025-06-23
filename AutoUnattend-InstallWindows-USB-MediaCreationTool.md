# Install Windows

## Windows Media Creation Tool 

Setup for a British English, single hard drive, wifi, computer 

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/how-configuration-passes-work

Lists Window's 10 variants to txt. Both x86 (32bit) and x64 (64bit) versions on the USB stick. 
```
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"D:\x64\sources\install.esd" | Out-File -FilePath "D:\x64\sources\install.esd.txt"
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"D:\x86\sources\install.esd" | Out-File -FilePath "D:\x86\sources\install.esd.txt"
```

Lists Window's 11 variants to txt. Windows 11 only comes in 64bit versions.
```
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"D:\sources\install.esd" | Out-File -FilePath "D:\sources\install.esd.txt"
```



## WindowsPE - WindowsPE Settings

[Component - Microsoft-Windows-International-Core-WinPE](AutoUnattend-WindowPE-Microsoft-Windows-International-Core-WinPE.md)

[InputLocale](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-inputlocale)
[SetupUILanguage](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-setupuilanguage)
[SystemLocale](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-systemlocale)
[UILanguage](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-uilanguage)
[UserLocale](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-userlocale)

```
<component name="Microsoft-Windows-International-Core-WinPE">
	<InputLocale>0809:00000809</InputLocale>
	<SetupUILanguage>
		<UILanguage>en-GB</UILanguage>
	</SetupUILanguage>
	<SystemLocale>en-GB</SystemLocale>
	<UILanguage>en-GB</UILanguage>
	<UserLocale>en-GB</UserLocale>
</component>
```

## WindowsPE - Windows Setup Settings

[Component - Microsoft-Windows-Setup](AutoUnattend-WindowsPE-Microsoft-Windows-Setup.md)






```
<component name="Microsoft-Windows-Setup">
```

[Diagnostics](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diagnostics)
```
	<Diagnostics>
		<OptIn>false</OptIn>
	</Diagnostics>
```

[DiskConfiguration](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration)
[Disk](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk)
[WillWipeDisk](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-willwipedisk)
```
	<DiskConfiguration>
		<WillShowUI>OnError</WillShowUI>
		<Disk wcm:action="add">
			<DiskID>0</DiskID>
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
	</DiskConfiguration> 
```

[ImageInstall](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall)
[OSImage](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage)
[InstallFrom](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installfrom)
[Path](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installfrom-path)
[MetaData](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installfrom-metadata)
[InstallToAvailablePartition](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installtoavailablepartition?source=recommendations)

```	
	<ImageInstall>
		<OSImage>
			<InstallFrom>
				<Path>\\networkshare\share\install.wim</Path>
				<MetaData wcm:action="add">
					<Key>/IMAGE/INDEX</Key>
					<Value>2</Value>
				</MetaData>
			</InstallFrom>
			<InstallTo>
				<DiskID>0</DiskID> 
				<PartitionID>3</PartitionID> 
			</InstallTo>
			<WillShowUI>OnError</WillShowUI>
			<InstallToAvailablePartition>false</InstallToAvailablePartition>
		</OSImage>
	</ImageInstall>  
</component>
```


[Component - Microsoft-Windows-PnpCustomizationsWinPE](AutoUnattend-WindowsPE-Microsoft-Windows-PnpCustomizationsWinPE.md)

## offlineServicing

Not Used

## generailize

Not Used

## specialize

Not Used

## auditSystem


## auditUser






