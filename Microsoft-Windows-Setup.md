# windowsPE Component
# Microsoft-Windows-Setup

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-setup

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

If you have been hacked, this option is generally the best as it will wipe malware stored on the drive that is not removed by simply removing the index of disk files.

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

[Generic Windows product keys](https://www.elevenforum.com/t/generic-product-keys-to-install-or-upgrade-windows-11-editions.3713/)

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