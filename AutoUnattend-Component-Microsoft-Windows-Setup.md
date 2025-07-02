```
<component name="Microsoft-Windows-Setup" processorArchitecture="X86"> 	/// https://learn.microsoft.com/en-us/dotnet/api/microsoft.build.utilities.processorarchitecture?view=msbuild-17-netcore
																		/// x86 specified as this will be used for the Windows 10 32bit installation running on VMware.
																		/// Add this line to the virtual PC's VMX file. guestOS = "windows-32"
																		/// https://techblog.paalijarvi.fi/2022/10/25/forcing-vmware-virtual-machines-to-appear-32-bit-on-64-bit-hosts/
	<UseConfigurationSet>true</UseConfigurationSet> /// Allows use of %configsetroot% which is the drive letter of the ISO or USB stick installing windows.
													/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-useconfigurationset

	<Diagnostics>									/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diagnostics
		<OptIn>false</OptIn> 						/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diagnostics-optin
	</Diagnostics>

	<DiskConfiguration>								/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration
		<WillShowUI>OnError</WillShowUI>			/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-willshowui
		<Disk wcm:action="add">
			<DiskID>0</DiskID>					/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk
			<WillWipeDisk>true</WillWipeDisk>	/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-willwipedisk

			/// Partition Layout https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/configure-uefigpt-based-hard-drive-partitions?view=windows-11#partition-layout
			<CreatePartitions>					/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-createpartitions
				<!-- System -->
				<CreatePartition wcm:action="add">	/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-createpartitions-createpartition
					<Order>1</Order> 			/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-createpartitions-createpartition-order
					<Type>EFI</Type> 			/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-createpartitions-createpartition-type
					<Size>300</Size> 			/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-createpartitions-createpartition-size
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

			<ModifyPartitions>				/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-modifypartitions

				<!-- EFI -->
				<ModifyPartition wcm:action="add">		/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-modifypartitions-modifypartition
					<Order>1</Order> 					/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-modifypartitions-modifypartition-order
					<PartitionID>1</PartitionID> 		/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-modifypartitions-modifypartition-partitionid
					<Format>FAT32</Format> 				/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-diskconfiguration-disk-modifypartitions-modifypartition-format
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
					<TypeID>de94bba4-06d1-4d40-a16a-bfd50179d6ac</TypeID>	/// https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/configure-uefigpt-based-hard-drive-partitions?view=windows-11#recovery-tools-partition		
																			/// https://support.microsoft.com/en-gb/topic/kb5028997-instructions-to-manually-resize-your-partition-to-install-the-winre-update-400faa27-9343-461c-ada9-24c8229763bf
					<Format>FAT32</Format>			
				</ModifyPartition>
			</ModifyPartitions>
		</Disk>
	</DiskConfiguration> 
	
	<ImageInstall>			/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall
		<OSImage>			/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage

			<Compact>false</Compact>	/// Make of this what you will... Conflicting info.
										/// ADK for Windows 10, version 1607 Compact OS https://learn.microsoft.com/en-us/windows-hardware/get-started/what-s-new-in-kits-and-tools#whats-new-in-the-windows-adk-for-windows-10-version-1607
										/// Support from WinPE starting in Windows 11, version 24H2. Beforehand it was Automatic. https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-command-line-options?view=windows-11#compactos
										/// https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/compact-os?view=windows-11
										/// Not listed in <OSImage> for ?view=windows-11 https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage?view=windows-11

			<InstallFrom>				/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installfrom
				<Path>%configsetroot%\sources\install.wim</Path>	/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installfrom-path
				<MetaData wcm:action="add">							/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installfrom-metadata
					<Key>/IMAGE/INDEX</Key>							/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installfrom-metadata-key
					<Value>1</Value> 								/// Windows 10 Pro N https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installfrom-metadata-value
				</MetaData>
			</InstallFrom>
			<InstallToAvailablePartition>false</InstallToAvailablePartition>		/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installtoavailablepartition
			<InstallTo>												/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installto
				<DiskID>0</DiskID>									/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installto-diskid 
				<PartitionID>3</PartitionID> 						/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-installto-partitionid
			</InstallTo>
			<WillShowUI>OnError</WillShowUI>						/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-imageinstall-osimage-willshowui		
		</OSImage>
	</ImageInstall>
	
	<UserData>												/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-userdata?view=windows-11
		<ProductKey>										/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-userdata-productkey?view=windows-11
			
			/// <Key> and <MetaData><Key><Value> both determine what version of windows to install.
			<Key>KYNWQ-VFYMR-DG2GG-BPXGF-KBVKB</Key>		/// Optional https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-userdata-productkey-key?view=windows-11
															/// Optional Generic Product Keys https://www.elevenforum.com/t/generic-product-keys-to-install-or-upgrade-windows-11-editions.3713/
															/// Optional As there is only one index in the install.wim, and <MetaData><Key><Value> specifies the Version to install.
															/// PS C:\WINDOWS\system32> (Get-WmiObject -query 'select * from SoftwareLicensingService').OA3xOriginalProductKey
			<WillShowUI>OnError</WillShowUI>				/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-userdata-productkey-willshowui?view=windows-11
		</ProductKey>
		<AcceptEula>true</AcceptEula>						/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-userdata-accepteula?view=windows-11
		<FullName>Fullname X86</FullName>					/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-userdata-fullname?view=windows-11
		<Organization>Organization X86</Organization>		/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup-userdata-organization?view=windows-11
	</UserData>
	
	/// Applies To: Windows 7, Windows 8, Windows 8.1, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Vista
	<DynamicUpdate>											/// https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-8.1-and-8/ff715725(v=win.10)
		<Enable>false</Enable>								/// https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-8.1-and-8/ff716469(v=win.10)
															/// True would require internet access to be available. This might also prevent the OOBE\BYPASSNRO hack seen in the Windows 11 setup process.						
															/// Not tested to see if it does affect OOBE\BYPASSNRO or not.
		<WillShowUI>OnError</WillShowUI>					/// https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-8.1-and-8/ff715476(v=win.10)
	</DynamicUpdate>
</component>
```