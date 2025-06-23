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

[UseConfigurationSet](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-useconfigurationset)

```
<UseConfigurationSet>true</UseConfigurationSet>  /// %configsetroot%
```

[ImageInstall](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall)
[OSImage](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage)
[InstallFrom](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installfrom)
[Path](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installfrom-path)
[MetaData](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installfrom-metadata)
[Key](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-dataimage-installfrom-metadata-key)
[InstallToAvailablePartition](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installtoavailablepartition?source=recommendations)
[InstallTo](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installto)



```	
	<ImageInstall>
		<OSImage>
			<InstallFrom>
				<Path>%configsetroot%\sources\install.esd</Path>
				<MetaData wcm:action="add">
					<Key>/IMAGE/INDEX</Key>
					<Value>7</Value>
				</MetaData>
			</InstallFrom>
			<InstallToAvailablePartition>false</InstallToAvailablePartition>
			<InstallTo>
				<DiskID>0</DiskID> 
				<PartitionID>3</PartitionID> 
			</InstallTo>
			<WillShowUI>OnError</WillShowUI>		
		</OSImage>
	</ImageInstall>  
</component>
```

To install drivers during an offline installation, first you need to create a folder called $WinpeDriver$ on the USB stick used by the Windows Media Creation program to copy the windows installation files. Next copy the extracted drivers into their own subfolders inside/below the $WinpeDriver$ eg.
```
USB Memory Stick\$WinpeDriver$\audio\
USB Memory Stick\$WinpeDriver$\graphics\
USB Memory Stick\$WinpeDriver$\motherboard\
USB Memory Stick\$WinpeDriver$\wlan\
```

[Component - Microsoft-Windows-PnpCustomizationsWinPE](AutoUnattend-WindowsPE-Microsoft-Windows-PnpCustomizationsWinPE.md)

[DriverPaths](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-pnpcustomizationswinpe-driverpaths)
[PathAndCredentials](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-pnpcustomizationswinpe-driverpaths-pathandcredentials)

```
<settings pass="windowsPE">
	<component name="Microsoft-Windows-PnpCustomizationsWinPE">
		<DriverPaths>
			<PathAndCredentials wcm:action="add" wcm:keyValue="1">
				<Path>%configsetroot%\Drivers</Path> /// <UseConfigurationSet>true</UseConfigurationSet> %configsetroot%
				<Credentials></Credentials>
			</PathAndCredentials>
		</DriverPaths>
	</component>
</settings>
```


## offlineServicing

Not Used

## generailize

Not Used

## specialize

[Microsoft-Windows-Shell-Setup](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup)
[ProductKey](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-productkey)
[Generic Product Keys](https://www.elevenforum.com/t/generic-product-keys-to-install-or-upgrade-windows-11-editions.3713/)


Microsoft-Windows-Shell-Setup
<ProductKey>AAAAA-BBBBB-CCCCC-DDDDD-EEEEE</ProductKey>

## auditSystem


## auditUser






