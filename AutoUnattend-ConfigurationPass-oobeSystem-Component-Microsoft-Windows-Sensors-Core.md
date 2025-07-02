```
<component name="Microsoft-Windows-Sensors-Core">		/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-sensors-core
														/// These are a mess, with missing documentation, that doesnt match whats found in the Windows SIM.
														/// Dont want the adapative dimming, but will allow dimming when the laptop battery runs down to a percentage.
	<AllowPresenceSensingCapable>0</AllowPresenceSensingCapable>	/// Win 11 https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-sensors-core-humanpresencesetting-allowpresencesensingcapable
	<DefaultInstantDim>0</DefaultInstantDim>			/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-sensors-core-humanpresencesetting-defaultinstantdim 
	<AllowDimCapable>0<AllowDimCapable>					/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-sensors-core-humanpresencesetting-allowdimcapable
	<DimSupported>0</DimSupported>						/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-sensors-core-humanpresencesetting-allowdimcapable
	<AdaptiveDimming>0</AdaptiveDimming>				/// https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-sensors-core-humanpresencesetting-defaultinstantdim
</component>
```