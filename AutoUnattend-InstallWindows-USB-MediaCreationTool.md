# Install Windows


  ********* Still to Do ProductKey
https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup



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

```
<settings pass="windowsPE">
</settings>
```

[Component - Microsoft-Windows-International-Core-WinPE](AutoUnattend-WindowPE-Microsoft-Windows-International-Core-WinPE.md)

```
	<component name="Microsoft-Windows-International-Core-WinPE">
	</component>
```

[InputLocale](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-inputlocale)
[SetupUILanguage](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-setupuilanguage)
[SystemLocale](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-systemlocale)
[UILanguage](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-uilanguage)
[UserLocale](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-international-core-winpe-userlocale)

```
		<InputLocale>0809:00000809</InputLocale>
		<SetupUILanguage>
			<UILanguage>en-GB</UILanguage>
		</SetupUILanguage>
		<SystemLocale>en-GB</SystemLocale>
		<UILanguage>en-GB</UILanguage>
		<UserLocale>en-GB</UserLocale>
```

## WindowsPE - Windows Setup Settings

[Component - Microsoft-Windows-Setup](AutoUnattend-WindowsPE-Microsoft-Windows-Setup.md)

```
	<component name="Microsoft-Windows-Setup">
	</component>
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
```

To install drivers during an offline installation, one way is to create a folder called $WinpeDriver$ on the USB stick used by the Windows Media Creation program to copy the windows installation files. 
Next copy the extracted drivers into their own subfolders inside/below the $WinpeDriver$ eg.
```
USB Memory Stick\$WinpeDriver$\audio\
USB Memory Stick\$WinpeDriver$\graphics\
USB Memory Stick\$WinpeDriver$\motherboard\
USB Memory Stick\$WinpeDriver$\wlan\
```

or

```
USB Memory Stick\$WinpeDriver$\Drivers\
```


[Component - Microsoft-Windows-PnpCustomizationsWinPE](AutoUnattend-WindowsPE-Microsoft-Windows-PnpCustomizationsWinPE.md)

```
	<component name="Microsoft-Windows-PnpCustomizationsWinPE">
	</component>
```

[DriverPaths](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-pnpcustomizationswinpe-driverpaths)
[PathAndCredentials](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-pnpcustomizationswinpe-driverpaths-pathandcredentials)

```
		<DriverPaths>
			<PathAndCredentials wcm:action="add" wcm:keyValue="1">
				<Path>%configsetroot%\Drivers</Path> /// <UseConfigurationSet>true</UseConfigurationSet> = %configsetroot%
				<Credentials></Credentials>
			</PathAndCredentials>
		</DriverPaths>
```





## offlineServicing

Add drivers using Dism - Works with .Wim but not .ESD image files as at 20250613:YYYYMMDD

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/offlineservicing?view=windows-11

```
<settings pass="offlineServicing">
</settings>
```

[Microsoft-Windows-Shell-Setup](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup)

```
	<component name="Microsoft-Windows-Shell-Setup">
	</component>
```

[ComputerName](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-computername)

```
	<ComputerName>""</ComputerName>
```

[BluetoothTaskbarIconEnabled](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-bluetoothtaskbariconenabled)

```
	<BluetoothTaskbarIconEnabled>false</BluetoothTaskbarIconEnabled)
```

[OfflineUserAccounts](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-offlineuseraccounts)

```
<OfflineUserAccounts>
     <OfflineAdministratorPassword>
        <Value>[PasswordValue]</Value>
        <PlainText>[true/false]</PlainText>
     </OfflineAdministratorPassword>

     <OfflineLocalAccounts>
         <LocalAccount>
             <Password>
                 <Value>[PasswordValue]</Value>
                 <PlainText>[true/false]</PlainText>
             </Password>
             <Group>[groups]</Group>
             <Name>[user]</Name>
             <DisplayName>[userdisplayname]</DisplayName>
         </LocalAccount>
     </OfflineLocalAccounts>

     <OfflineDomainAccounts>
         <OfflineDomainAccount>
             <SID>[SID1]</SID>
             <Group>[groups]</Group>
         </OfflineDomainAccount>
         <OfflineDomainAccount>
             <SID>[SID2]</SID>
             <Group>[groups]</Group>
         </OfflineDomainAccount>
    </OfflineDomainAccounts>
</OfflineUserAccounts>
```


## generailize

Not Used

## specialize

[Microsoft-Windows-Shell-Setup](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup)
[ProductKey](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-productkey)
[Generic Product Keys](https://www.elevenforum.com/t/generic-product-keys-to-install-or-upgrade-windows-11-editions.3713/)

```
<settings pass="specialize">
	<component name="Microsoft-Windows-Shell-Setup">
		<ProductKey>AAAAA-BBBBB-CCCCC-DDDDD-EEEEE</ProductKey>
	</component>
</settings>	
```

[Microsoft-Windows-Shell-Setup](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup)

```
	<component name="Microsoft-Windows-Shell-Setup">
	</component>
```

[CopyProfile](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-copyprofile)

[NotificationArea](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-notificationarea)

[ShowPowerButtonOnStartScreen](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-showpowerbuttononstartscreen)

```
<ShowPowerButtonOnStartScreen>true</ShowPowerButtonOnStartScreen>
```


[TimeZone](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-timezone)

```
<TimeZone>Greenwich Mean Time (GMT)</TimeZone>
```

## auditSystem


## auditUser

## oobe

[Microsoft-Windows-Shell-Setup](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup)

```
	<component name="Microsoft-Windows-Shell-Setup">
	</component>
```

[OOBE](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-oobe)

[HideEULAPage](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-oobe-hideeulapage)

```
<HideEULAPage>true</HideEULAPage>
```

[HideOEMRegistrationScreen](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-oobe-hideoemregistrationscreen)

```
<HideOEMRegistrationScreen>true</HideOEMRegistrationScreen>
```

[HideOnlineAccountScreens](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-oobe-hideonlineaccountscreens)

```
<HideOnlineAccountScreens>true</HideOnlineAccountScreens>
```

[HideWirelessSetupInOOBE](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-oobe-hidewirelesssetupinoobe)

```
<HideWirelessSetupInOOBE>true</HideWirelessSetupInOOBE>
```

[ProtectYourPC](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-oobe-protectyourpc)

```
<ProtectYourPC>3</ProtectYourPC>
```

[VMModeOptimizations](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-oobe-vmmodeoptimizations)

```
<VMModeOptimizations>
	<SkipAdministratorProfileRemoval>true</SkipAdministratorProfileRemoval>
    <SkipNotifyUILanguageChange>true</SkipNotifyUILanguageChange>
    <SkipWinREInitialization>true</SkipWinREInitialization>
</VMModeOptimizations>
```

[TaskbarLinks](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-taskbarlinks)

[Themes](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-themes)
[UWPAppsUseLightTheme](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-themes-uwpappsuselighttheme)

```
<Themes>
	<UWPAppsUseLightTheme>false</UWPAppsUseLightTheme> /// Dark Mode
</Themes>
```

[WindowColor](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-themes-windowcolor)


[UserAccounts](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-useraccounts)
[LocalAccounts](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-useraccounts-localaccounts)
[LocalAccount](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-useraccounts-localaccounts-localaccount)

```
<UserAccounts>
   <LocalAccounts>
      <LocalAccount wcm:action="add">
         <Password>
            <Value>cAB3AFAAYQBzAHMAdwBvAHIAZAA</Value>
            <PlainText>false</PlainText>
         </Password>
         <Description>Test account</Description>
         <DisplayName>Admin/Power User Account</DisplayName>
         <Group>Administrators;Power Users</Group>
         <Name>Test1</Name>
      </LocalAccount>
      <LocalAccount wcm:action="add">
         <Password>
            <Value>cABhAHMAcwB3AG8AcgBkAFAAYQBzAHMAdwBvAHIAZAA=</Value>
            <PlainText>false</PlainText>
         </Password>
         <Description>For testing</Description>
         <DisplayName>Admin Account</DisplayName>
         <Group>Administrators</Group>
         <Name>Test2</Name>
      </LocalAccount>
   </LocalAccounts>
</UserAccounts>
```


[VisualEffects](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-visualeffects)

