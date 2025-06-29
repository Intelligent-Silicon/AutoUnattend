Install Windows using an ISO with a WIM file.

Windows installers with an install.esd doesnt work with the ADK Deployment Tools.

https://theitbros.com/convert-windows-esd-file-to-windows-wim-file/

dism /export-image /SourceImageFile:install.esd /SourceIndex:6 /DestinationImageFile:install.wim /Compress:max /CheckIntegrity

Mount the MCT ISO. Decide on the version of windows to install. Index 6 = Windows 10 Pro
Save Index 6 to install.wim

PS C:\WINDOWS\system32> dism /export-image /SourceImageFile:"C:\mount\sources\install.esd" /SourceIndex:6 /DestinationImageFile:"C:\mount\sources\install.wim" /Compress:max /CheckIntegrity

PS C:\WINDOWS\system32> DISM /Get-WimInfo /WimFile:"C:\mount\sources\install.wim"