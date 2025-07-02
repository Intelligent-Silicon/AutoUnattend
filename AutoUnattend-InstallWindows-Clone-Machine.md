Set up a computer just how like it.

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/update-windows-settings-and-scripts-create-your-own-answer-file-sxs?view=windows-11#step-2-create-an-answer-file


Open up the command window and type:
%WINDIR%\system32\sysprep /Generalize /OOBE /Shutdown /unattend:

After /Generalize the Specialize is ALWAYS run immediately afterwards, regardless of whether /AuditMode or /OOBE is specified.

Use /AuditMode to boot to windows, where you can add/remove drivers and apps before capturing the image. 
You can rerun into /AuditMode several times, because when installing some programs, the system needs to be rebooted, eg MS Office, or Clarion.

If you are making a master copy of your computer, set it up just how you like it.
Add Apps,
Remove Apps,
Configure File Explorer how you like, ie Detail view. 


You can also use the setting: 
Microsoft-Windows-PnpSysPrep\PersistAllDeviceInstalls
https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-pnpsysprep-persistalldeviceinstalls
https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-7/ee832798(v=ws.10)#hardware-configuration-changes 
This setting keeps the drivers that your machine uses, but uninstalls them.

Use /OOBE to boot direct to OOBE without any further changes in order to capture the image.


Boot to the WindowsPE window, needs an answer file.

This answer file 

<?xml version="1.0" encoding="utf-8"?>
<unattend xmlns="urn:schemas-microsoft-com:unattend" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State">
	<settings pass="generalize">
		<component name="Microsoft-Windows-PnpSysPrep">
			<PersistAllDeviceInstalls>true</PersistAllDeviceInstalls>
		</component>
	</settings>
	<settings pass="oobeSystem">	
		<component name="Microsoft-Windows-Deployment">
			<Reseal>
			   <ForceShutdownNow>false</ForceShutdownNow>
			   <Mode>OOBE</Mode>
			</Reseal>
		</component>
	</settings>
</unattend>


https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/boot-windows-to-audit-mode-or-oobe?view=windows-11#boot-to-audit-mode-automatically-from-an-existing-image

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/update-windows-settings-and-scripts-create-your-own-answer-file-sxs?view=windows-11#step-2-create-an-answer-file

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-automation-overview?view=windows-11#implicit-answer-file-search-order

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-mount-and-customize?view=windows-11#add-updates-to-winpe-if-needed

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-create-usb-bootable-drive?view=windows-11

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-mount-and-customize?view=windows-11#add-updates-to-winpe-if-needed


5. Capture the Image:
Boot the computer using Windows PE (using bootable media like a USB flash drive). Use the Dism /capture-image command to capture the generalized image. 
6. Deploy the Image:
The captured image can now be deployed to other machines

C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools> copype x86 C:\mount_Win10x32_WinPE

PS C:\WINDOWS\system32> Dism /Mount-Image /ImageFile:"C:\mount_Win10x32_WinPE\media\sources\boot.wim" /Index:1 /MountDir:"C:\mount_Win10x32_WinPE\mount"