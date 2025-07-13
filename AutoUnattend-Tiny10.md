# Tiny 10 ISO

https://ntdev.blog/2024/01/08/the-complete-tiny10-and-tiny11-list/

https://archive.org/details/tiny-10_202301

https://archive.org/download/tiny-10_202301/tiny10%202303%20x86.iso



Windows 10 Enterprise LTSC

Chose Win10 Pro N but it installed Enterprise LTSC

Disk Space
7.41 GB used
32.5GB Free
39.9GB total

Got to Activate Windows before any changes can be made.

No Taskbar icons

Folder Heirachy
```
Get-ChildItem -Path "C:\" -Directory -Recurse | Select-Object -ExpandProperty FullName | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Folder.Hierarchy.txt"
```

List Drivers
```
Dism /Online /Get-Drivers | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\3rd.Party.Drivers.txt"
```

Lists All Hardware with and without Drivers.
```
PS C:\WINDOWS\system32> Get-WmiObject Win32_PNPEntity | Select * | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Hardware.All.And.Driver.Status.txt"
```

List All Hardware missing a Driver.
```
PS C:\WINDOWS\system32> Get-WmiObject Win32_PnPEntity | Where {$_.Status -ne "OK"} | Select * | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Hardware.And.MissingDrivers.txt"
```

List All VMware Hard Drivers
```
PS C:\WINDOWS\system32> Get-WmiObject Win32_PnPEntity | Where {$_.Name -match "VMware"} | Select * | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Hardware.VMware.Drivers.txt"
```

List All AppX
```
PS C:\WINDOWS\system32> Get-AppxPackage -Allusers | Format-Table -AutoSize | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Windows.AppX.AllUsers.txt"
```

List All Services
```
Get-Service | ForEach-Object { 
    $service = $_
    $serviceDetails = Get-WmiObject -Class Win32_Service -Filter "Name='$($service.Name)'"
    $dependentServices = $serviceServices.DependentServices | Select-Object -ExpandProperty Name

    [PSCustomObject]@{
        Name             = $service.Name
        DisplayName      = $service.DisplayName
        Status           = $service.Status
        StartType        = $serviceDetails.StartMode
        ServiceType      = $serviceDetails.ServiceType
        LogOnAs          = $serviceDetails.StartName
        CanPauseAndContinue = $serviceDetails.AcceptPause
        CanStop          = $serviceDetails.AcceptStop
        Dependencies     = ($dependentServices -join ", ")
    }
} | Format-Table -AutoSize | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Services.AllProperties.txt" 
```

List all Software

```
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Services.AllProperties.txt" 
```

```
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Software.Installed.txt" 
```

List All Packages

```
Get-Package | Format-List | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Packages.txt"
```

PS C:\Windows\system32> Get-Package
WARNING: Network connectivity may not be available, unable to reach remote sources.
WARNING: Unable to bootstrap the required package provider due to problems with network connectivity. Please fix your network connection. If this is not possible, refer to 'Get-Help Install-PackageProvider' or
https:/go.microsoft.com/fwlink/?LinkId=626941 for guidance on installing the package provider manually.
WARNING: MSG:UnableToDownload «https://go.microsoft.com/fwlink/?LinkID=627338&clcid=0x409» «»
WARNING: Unable to download the list of available providers. Check your internet connection.
PS C:\Windows\system32>