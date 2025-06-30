Install Windows using an ISO with a WIM file.

Windows installers with an install.esd doesnt work with the ADK Deployment Tools.

https://theitbros.com/convert-windows-esd-file-to-windows-wim-file/

dism /export-image /SourceImageFile:install.esd /SourceIndex:6 /DestinationImageFile:install.wim /Compress:max /CheckIntegrity

Mount the MCT ISO. Decide on the version of windows to install. Index 6 = Windows 10 Pro
Save Index 6 to install.wim

PS C:\WINDOWS\system32> dism /export-image /SourceImageFile:"C:\mount\sources\install.esd" /SourceIndex:6 /DestinationImageFile:"C:\mount\sources\install.wim" /Compress:max /CheckIntegrity

PS C:\WINDOWS\system32> DISM /Get-WimInfo /WimFile:"C:\mount\sources\install.wim"


Dism /unmount-image /mountdir:"C:\mount_Win10_Pro_x32" /discard

Dism /get-mountedwiminfo

Dism /mount-image /imagefile:"C:\mount\sources\install.wim" /index:1 /mountdir:"C:\mount_Win10_Pro_x32"

Dism /Image:"C:\mount_Win10_Pro_x32" /Add-Package /PackagePath:"C:\Users\Admin1\Documents\ISO Files\windows10.0-kb5060533-x86_de4a47dde17d91023f93eb9a37c6c96faebf768c.msu" /LogPath:"C:\Users\Admin1\Documents\ISO Files\Add-Package-windows10.0-kb5060533-x86.log"

Dism /Image:"C:\mount_Win10_Pro_x32" /Get-Features | Out-File -FilePath "C:\Users\Admin1\Documents\ISO Files\Win10-Pro-x32-Features.txt"

Dism /Image:"C:\mount_Win10_Pro_x32" /Get-Features /Format:Table | Find "Disabled" 

Dism /Image:"C:\mount_Win10_Pro_x32" /Get-Features /Format:Table | Where-Object {$_.State -eq "Disabled"}

/Online /Disable-Feature /FeatureName:TFTP /Remove



PS C:\WINDOWS\system32> Get-WindowsOptionalFeature -Path "C:\mount_Win10_Pro_x32" -FeatureName NetFx3 | select featurename, state

FeatureName                      State
-----------                      -----
NetFx3      DisabledWithPayloadRemoved


Get-WindowsOptionalFeature -Path "C:\mount_Win10_Pro_x32"  | select featurename, state | Where-Object {$_.State -eq "Disabled" -OR $_.State -eq "DisabledWithPayloadRemoved"}

Get-WindowsOptionalFeature -Path "C:\mount_Win10_Pro_x32" /Disable-Feature /FeatureName:TFTP /Remove

Get-WindowsFeature | Where-Object -FilterScript { $_.Installed -Eq $TRUE } | Uninstall-WindowsFeature

Get-WindowsOptionalFeature -Path "C:\mount_Win10_Pro_x32" | Where-Object -FilterScript { $_.Installed -Eq $TRUE }


Get-WindowsFeature | Where-Object -FilterScript { $_.Installed -Eq $TRUE }

Get-WindowsOptionalFeature -Path "C:\mount_Win10_Pro_x32" | Where-Object {$_.State -eq "Enabled"}

Get-WindowsOptionalFeature -Path "C:\mount_Win10_Pro_x32" | select featurename, state | Where-Object {$_.State -eq "Disabled"} | Uninstall-WindowsFeature

Get-WindowsOptionalFeature -Path "C:\mount_Win10_Pro_x32" -FeatureName SMB1Protocol | Disable-WindowsOptionalFeature -Remove

Get-WindowsOptionalFeature -Path "C:\mount_Win10_Pro_x32"  | select featurename, packagename, state | Where-Object {$_.State -eq "Disabled" -OR $_.State -eq "DisabledWithPayloadRemoved"}

Get-WindowsOptionalFeature -Path "C:\mount_Win10_Pro_x32" -PackageName * | Where-Object {$_.State -eq "Disabled"} | Remove-WindowsPackage

Remove-WindowsPackage -Path "C:\mount_Win10_Pro_x32" -PackageName * | Where-Object {$_.State -eq "Disabled"}

Remove-WindowsPackage -Path "C:\mount_Win10_Pro_x32" | Where-Object {$_.State -eq "Disabled"}

Get-WindowsPackage -Path "C:\mount_Win10_Pro_x32" | Where-Object {$_.State -eq "Disabled"}

Get-WindowsOptionalFeature -Path "C:\mount_Win10_Pro_x32"  -FeatureName * | select featurename, packagename, state | Where-Object {$_.State -eq "Disabled"}

