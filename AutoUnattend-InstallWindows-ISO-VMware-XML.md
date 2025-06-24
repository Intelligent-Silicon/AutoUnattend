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
					<Name>Admin1</Name>
					<DisplayName></DisplayName>
					<Group>Administrators</Group>
					<Password>
						<Value>admin1</Value>
						<PlainText>true</PlainText>
					</Password>
				</LocalAccount>
				<LocalAccount wcm:action="add">
					<Name>User1</Name>
					<DisplayName></DisplayName>
					<Group>Users</Group>
					<Password>
						<Value>user1</Value>
						<PlainText>true</PlainText>
					</Password>
				</LocalAccount>
			</LocalAccounts>
		</UserAccounts>
	</component>
	<component name="Microsoft-Windows-Sensors-Core">
		<Dim Supported>0</Dim Supported>
		<Adaptive Dimming>0</Adaptive Dimming>
	</component>
</settings>