```
<component name="Microsoft-Windows-Shell-Setup">						/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup
																		/// This Component in offlineServicing is a test to see if these configurations occur.
	<ComputerName>*</ComputerName>										/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-computername
	<BluetoothTaskbarIconEnabled>false</BluetoothTaskbarIconEnabled>	/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-bluetoothtaskbariconenabled
	<OEMInformation>													/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-oeminformation
		<SupportProvider>offlineServicing pass</SupportProvider>		/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup-oeminformation-supportprovider
	</OEMInformation>
</component>
```