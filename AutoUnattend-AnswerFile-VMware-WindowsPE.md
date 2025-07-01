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
						<Size>40000</Size>		 
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
```