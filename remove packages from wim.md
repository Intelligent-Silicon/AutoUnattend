# Remove Packages from .WIM & .ESD

Take Stock of the WIM

PS C:\WINDOWS\system32> Dism /Get-ImageInfo /imagefile:"C:\mount\sources\install.esd" /Index:6

PS C:\WINDOWS\system32> Dism /image:"C:\mount_Win10_Pro_x32" /Get-Drivers /all | Out-File -FilePath "C:\Users\Admin1\Documents\Drivers\DriverStoreDrivers.txt"


PS C:\WINDOWS\system32> pnputil.exe -e | format:list | Out-File -FilePath "C:\Users\Admin1\Documents\Drivers\installeddrivers.txt"


PS C:\WINDOWS\system32> Dism /image:"C:\mount_Win10_Pro_x32" /Get-DriverInfo /driver:1394.inf

https://www.reddit.com/r/SCCM/comments/cazudh/deleting_windows_builtin_apps_from_wim_files/