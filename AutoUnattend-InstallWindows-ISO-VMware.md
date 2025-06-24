# Install Windows 10 x32bit

## ISO file for VMware

Setup for:
British English
Single Hard Drive
Ethernet
No Bluetooth
No Printer
VMware Tools Installed

Download Windows 10 2022 Update | version 22H2 Media Creation Tool

https://www.microsoft.com/en-gb/software-download/windows10

Mount ISO file: C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32_x64.iso

Load Powershell, check ExecutionPolicy is set to Bypass, Mount ISO as Cd/DVD, Get ISO Drive Letter, Copy ISO contents to C:\Mount, Dismount ISO. 
```
PS C:\WINDOWS\system32> Get-ExecutionPolicy -list

PS C:\WINDOWS\system32> $DiskImageResult = Mount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32_x64.iso"
PS C:\WINDOWS\system32> $DiskImageDriveLetter = ($DiskImageResult | Get-Volume).DriveLetter
PS C:\WINDOWS\system32> Copy-Item -Path "$($DiskImageDriveLetter):\*" -Destination "C:\mount" -Recurse
PS C:\WINDOWS\system32> Dismount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\Win10_22H2_x32_x64.iso"
```
Check what versions & variants of Windows 10 exist in the install.esd file.
```
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount\x64\sources\install.esd" | Out-File -FilePath "C:\mount\x64\sources\install.esd.txt"
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"C:\mount\x86\sources\install.esd" | Out-File -FilePath "C:\mount\x86\sources\install.esd.txt"
```

Create AutoUnattend.xml file and save it to C:\mount.

```
<settings pass="windowsPE">
	<component name="Microsoft-Windows-International-Core-WinPE">
		<InputLocale>0809:00000809</InputLocale>
		<SetupUILanguage>
			<UILanguage>en-GB</UILanguage>
		</SetupUILanguage>
		<SystemLocale>en-GB</SystemLocale>
		<UILanguage>en-GB</UILanguage>
		<UserLocale>en-GB</UserLocale>
	</component>
	<component name="Microsoft-Windows-Setup">
		<Diagnostics>
			<OptIn>false</OptIn>
		</Diagnostics>
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
						<Size>21000</Size>		 
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
		<UseConfigurationSet>true</UseConfigurationSet>  /// %configsetroot%
		<ImageInstall>
			<OSImage>
				<InstallFrom>
					<Path>%configsetroot%\sources\install.esd</Path>
					<MetaData wcm:action="add">
						<Key>/IMAGE/INDEX</Key>
						<Value>6</Value> /// Windows 10 Pro
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
		<DynamicUpdate>
			<Enable>false</Enable>
			<WillShowUI>OnError</WillShowUI>
		</DynamicUpdate>		
	</component>
</settings>
<settings pass="offlineServicing">
	<component name="Microsoft-Windows-Shell-Setup">
		<ComputerName>""</ComputerName>
		<BluetoothTaskbarIconEnabled>false</BluetoothTaskbarIconEnabled>
	</component>
</settings>
<settings pass="specialize">
	<component name="Microsoft-Windows-Shell-Setup">
		<ProductKey>VK7JG-NPHTM-C97JM-9MPGT-3V66T</ProductKey> /// Windows 10/11 Pro
		<ShowPowerButtonOnStartScreen>false</ShowPowerButtonOnStartScreen>
		<TimeZone>GMT Standard Time</TimeZone>
	</component>
	<component name="Microsoft-Windows-STObject">
		<FlyoutAutoPowerScheme>8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c</FlyoutAutoPowerScheme> /// High performance
	</component>
	<component name="Microsoft-Windows-ErrorReportingCore">
		<DefaultConsent>0</DefaultConsent>
		<DisableWER>1</DisableWER>
	</component>
</settings>
<settings pass="oobe">
	<component name="Microsoft-Windows-Shell-Setup">
		<HideEULAPage>true</HideEULAPage>
		<HideOEMRegistrationScreen>true</HideOEMRegistrationScreen>
		<HideOnlineAccountScreens>true</HideOnlineAccountScreens>
		<HideWirelessSetupInOOBE>true</HideWirelessSetupInOOBE>
		<ProtectYourPC>3</ProtectYourPC>
		<Themes>
			<UWPAppsUseLightTheme>false</UWPAppsUseLightTheme> /// Dark Mode
		</Themes>
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
	</component>
</settings>



