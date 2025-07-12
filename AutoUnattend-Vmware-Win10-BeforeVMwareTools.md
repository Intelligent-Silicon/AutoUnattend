# Window 10

After installation, before VMware Tools installed, but VMware Drivers installed

Drivers
List installed Drivers

List two files OEM0.inf and OEM1.inf. Get-Drivers shows more info, namely original filename.
```
PS C:\WINDOWS\system32> Dism /Online /Get-Drivers | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Drivers\InstalledDrivers.BeforeVMwareTools.Get-Drivers.txt"
```

Lists All Hardware with and without Drivers.
```
PS C:\WINDOWS\system32> Get-WmiObject Win32_PNPEntity | Select * | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Drivers\InstalledDrivers.BeforeVMwareTools.Get-WMIobject.Win32_PNPEntity.txt"
```

List All Hardware missing a Driver.
```
PS C:\WINDOWS\system32> Get-WmiObject Win32_PnPEntity | Where {$_.Status -ne "OK"} | Select * | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Drivers\InstalledDrivers.BeforeVMwareTools.Get-WMIobject.Win32_PNPEntity.MissingDrivers.txt"
```

List All VMware Hard Drivers
```
PS C:\WINDOWS\system32> Get-WmiObject Win32_PnPEntity | Where {$_.Name -match "VMware"} | Select * | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Drivers\InstalledDrivers.BeforeVMwareTools.Get-WMIobject.Win32_PNPEntity.VMwareDrivers.txt"
```


List All AppX
```
PS C:\WINDOWS\system32> Get-AppxPackage -Allusers | Format-Table -AutoSize | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\ISO Files\WindowsAppX.AllUsers.txt"
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




