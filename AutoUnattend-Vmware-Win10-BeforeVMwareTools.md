# Window 10

Audit PC

```
md "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC"
```

```
PS C:\Users\Admin1> Get-CimInstance Win32_OperatingSystem | Select-Object Caption | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\Get-CimInstance.Win32_OperatingSystem.Caption.txt"

Caption
-------
Microsoft Windows 10 Enterprise LTSC

PS C:\Users\Admin1> Get-CimInstance Win32_OperatingSystem | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\Get-CimInstance.Win32_OperatingSystem.txt"

SystemDirectory     Organization BuildNumber RegisteredUser SerialNumber            Version
---------------     ------------ ----------- -------------- ------------            -------
C:\Windows\system32              19044       Windows User   00425-00000-00002-AA384 10.0.19044
```



DriverStore.Inf.Files.txt
```
PS C:\WINDOWS\system32> Dism /Online /Get-Drivers | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\DriverStore.Inf.Files.txt"
```

InstalledDrivers.Get-WMIobject.Win32_PNPEntity.txt
```
PS C:\WINDOWS\system32> Get-WmiObject Win32_PNPEntity | Select * | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\InstalledDrivers.Get-WMIobject.Win32_PNPEntity.txt"
```

InstalledDrivers.Get-WMIobject.Win32_PNPEntity.MissingDrivers.txt
```
PS C:\WINDOWS\system32> Get-WmiObject Win32_PnPEntity | Where {$_.Status -ne "OK"} | Select * | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\InstalledDrivers.Get-WMIobject.Win32_PNPEntity.MissingDrivers.txt"
```

InstalledDrivers.Get-WMIobject.Win32_PNPEntity.VMwareDrivers.txt
```
PS C:\WINDOWS\system32> Get-WmiObject Win32_PnPEntity | Where {$_.Name -match "VMware"} | Select * | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\InstalledDrivers.Get-WMIobject.Win32_PNPEntity.VMwareDrivers.txt"
```


WindowsAppX.AllUsers.txt
```
PS C:\WINDOWS\system32> Get-AppxPackage -Allusers | Format-Table -AutoSize | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\InstalledAppX.Get-AppxPackage.AllUsers.txt"
```

Services.AllProperties.txt
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
} | Format-Table -AutoSize | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\Services.All.Dependancies.txt" 
```

List all Services with Recovery Options
```
Get-Service | ForEach-Object {
    $service = $_
    $serviceName = $service.Name
    $regPath = "HKLM:\SYSTEM\CurrentControlSet\Services\$serviceName"

    # Recovery info from registry
    $recovery = Get-ItemProperty -Path $regPath -ErrorAction SilentlyContinue

    # Extended details via WMI
    $wmi = Get-WmiObject -Class Win32_Service -Filter "Name='$serviceName'"
    $displayName  = $wmi.DisplayName
    $logonAccount = $wmi.StartName
    $startMode    = $wmi.StartMode
    $status       = $wmi.State

     [PSCustomObject]@{
        ServiceName           = $serviceName
        DisplayName           = $displayName
        Status                = $status
        StartupType           = $startMode
        LogonAccount          = $logonAccount
        FirstFailureAction    = if ($recovery.PSObject.Properties['FirstFailure'])  { $recovery.FirstFailure } else { 'N/A' }
        SecondFailureAction   = if ($recovery.PSObject.Properties['SecondFailure']) { $recovery.SecondFailure } else { 'N/A' }
        ThirdFailureAction    = if ($recovery.PSObject.Properties['ThirdFailure'])  { $recovery.ThirdFailure } else { 'N/A' }
        ResetPeriodInSeconds  = if ($recovery.PSObject.Properties['ResetPeriod'])   { $recovery.ResetPeriod } else { 'N/A' }
        RebootMessage         = if ($recovery.PSObject.Properties['RebootMessage']) { $recovery.RebootMessage } else { 'N/A' }
        CommandLineAction     = if ($recovery.PSObject.Properties['Command'])       { $recovery.Command } else { 'N/A' }
    }
} | Format-Table -AutoSize | Out-File -width 2000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\Services.All.Recovery.txt" 
```

Software.All.Uninstall.txt
```
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\Software.All.Uninstall.txt" 
```

Tasks.Scheduled.txt
```
PS C:\WINDOWS\system32> Get-ScheduledTask | Format-Table -AutoSize  | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\Tasks.Scheduled.txt"
```

Tasks.Scheduled.Info.txt
```
# Get all scheduled tasks
$tasks = Get-ScheduledTask

$taskInfoList = foreach ($task in $tasks) {
    try {
        $info = Get-ScheduledTaskInfo -TaskName $task.TaskName -TaskPath $task.TaskPath
        $principal = $task.Principal.UserId

        [PSCustomObject]@{
            TaskName        = $task.TaskName
            TaskPath        = $task.TaskPath
            User            = $principal
            LastRunTime     = $info.LastRunTime
            NextRunTime     = $info.NextRunTime
            LastTaskResult  = $info.LastTaskResult
            LastStatus      = $info.LastStatus
        }
    } catch {
        [PSCustomObject]@{
            TaskName        = $task.TaskName
            TaskPath        = $task.TaskPath
            User            = "N/A"
            LastRunTime     = "N/A"
            NextRunTime     = "N/A"
            LastTaskResult  = "N/A"
            LastStatus      = "Failed to retrieve ScheduledTaskInfo"
        }
    }
}

# Display results in a table
$taskInfoList | Format-Table -AutoSize | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\Tasks.Scheduled.Info.txt"
```



Startups.txt
```
# Get startup entries from registry and startup folders
$startupEntries = @()

# Check common registry locations
$regPaths = @(
    "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run",
    "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run"
)

foreach ($path in $regPaths) {
    try {
        $entries = Get-ItemProperty -Path $path | Select-Object -Property * -ExcludeProperty PSPath, PSParentPath, PSChildName, PSDrive, PSProvider
        foreach ($entry in $entries.PSObject.Properties) {
            $startupEntries += [PSCustomObject]@{
                Source      = $path
                AppName     = $entry.Name
                Command     = $entry.Value
                User        = if ($path -like "HKCU*") { "$env:USERNAME (Current User)" } else { "System" }
            }
        }
    } catch {
        Write-Warning "Failed to access $path"
    }
}

# Check Startup folders
$startupFolders = @(
    "$env:APPDATA\Microsoft\Windows\Start Menu\Programs\Startup",   # Current user
    "$env:ProgramData\Microsoft\Windows\Start Menu\Programs\Startup" # All users
)

foreach ($folder in $startupFolders) {
    if (Test-Path $folder) {
        Get-ChildItem -Path $folder -File | ForEach-Object {
            $startupEntries += [PSCustomObject]@{
                Source      = $folder
                AppName     = $_.Name
                Command     = $_.FullName
                User        = if ($folder -like "*ProgramData*") { "System (All Users)" } else { "$env:USERNAME (Current User)" }
            }
        }
    }
}

# Display results in a table
$startupEntries | Format-Table -AutoSize | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\Startups.txt"

```

Firewall.Rules.txt
```
# List all firewall rules
Get-NetFirewallRule |
Select-Object Name, DisplayName, Enabled, Direction, Action, Profile |
Format-Table -AutoSize | Out-File -width 1000 -Encoding UTF8 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\Firewall.Rules.txt"
```

Firewall.Rules.2.txt
```
$profiles = @('Private', 'Public', 'Domain')

foreach ($profile in $profiles) {
    $rules = Get-NetFirewallRule |
        Where-Object { $_.Profile -contains $profile } |
        Select-Object DisplayName, Direction, Action, Enabled, Profile |
        Sort-Object DisplayName

    $header = "`n=== $profile Profile Firewall Rules ===`n"
    $outputPath = "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\Firewall.Rules.2.txt"

    $header | Out-File -width 1000 -Encoding UTF8 -FilePath $outputPath 
    $rules | Out-String | Out-File -width 1000 -Encoding UTF8 -FilePath $outputPath -Append
}
```
Firewall.Backup.wfw
```
# Export current firewall rules to a file which can be used to replace and restore existing firewall rules
$backupPath = "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\Firewall.Backup.wfw"
netsh advfirewall export $backupPath
```
Import Firewall Rules - Overwrites existing rules.
```
# Import firewall rules to a file
$backupPath = "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\Firewall.Backup.wfw"
netsh advfirewall import $backupPath
```

```
Get-ChildItem -Path 'C:\' -Directory -Recurse | Select-Object FullName | Out-File -width 1000 -Encoding UTF8 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\Folders.txt"
``` 

```
Get-WmiObject -Class Win32_LogicalDisk -Filter "DriveType=3" | Select-Object `
    @{Name="Drive";Expression={$_.DeviceID}},
    @{Name="Size (GB)";Expression={[math]::round($_.Size / 1GB, 2)}},
    @{Name="Free Space (GB)";Expression={[math]::round($_.FreeSpace / 1GB, 2)}},
    @{Name="Used Space (GB)";Expression={[math]::round(($_.Size - $_.FreeSpace) / 1GB, 2)}} |
    Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\DiskSpace.txt"
```

```
Get-ChildItem -Path 'C:\' -Directory -Recurse -ErrorAction SilentlyContinue | ForEach-Object {
    $folderPath = $_.FullName
    $folderSize = (Get-ChildItem -Path $folderPath -Recurse -File -ErrorAction SilentlyContinue |
        Measure-Object -Property Length -Sum).Sum
    $sizeMB = [math]::round($folderSize / 1MB, 2)
    $output = "Folder: $folderPath | Size: $sizeMB MB"
    Write-Output $output
    $output
} | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC\Folder.Size.txt"
```


Lockdown Firewall allowing only Github and Firefox
```
# Step 1: Block all outbound traffic
Set-NetFirewallProfile -Profile Domain,Private,Public -DefaultOutboundAction Block -DefaultInboundAction Block

# Step 2: Allow Firefox outbound
New-NetFirewallRule -DisplayName "Allow Firefox Outbound" `
  -Direction Outbound `
  -Program "C:\Program Files\Mozilla Firefox\firefox.exe" `
  -Action Allow `
  -Profile Domain,Private,Public

# Step 3: Allow GitHub Domains (via port 443 + 80 for HTTPS/HTTP)
$githubDomains = @(
  "github.com", "api.github.com", "githubusercontent.com"
)

foreach ($domain in $githubDomains) {
    New-NetFirewallRule -DisplayName "Allow $domain" `
        -Direction Outbound `
        -RemoteFqdn $domain `
        -Protocol TCP `
        -LocalPort Any `
        -RemotePort 443 `
        -Action Allow `
        -Profile Domain,Private,Public
}

# Optional: Log dropped packets for analysis
Set-NetFirewallProfile -LogBlocked -LogFileName "C:\Users\Admin1\Documents\Firewall Log\FirewallLogs.txt"
```






List inbound rules
```
Get-NetFirewallRule -Direction Inbound | Where-Object {$_.Enabled -eq "True"} |
Select-Object Name, DisplayName, Action, Profile, Protocol, LocalPort, RemoteAddress |
Format-Table -AutoSize | Out-File -width 1000 -Encoding UTF8 -FilePath "C:\Users\Admin1\Documents\Firewall.Rules.Inbound.txt"
```

Log Network Traffic
```
# Get the current date and time for log naming
$timestamp = Get-Date -Format "yyyy-MM-dd_HH-mm-ss"
$logFile = "NetworkTrafficLog_$timestamp.txt"

# Collect all TCP connections
$connections = Get-NetTCPConnection | Where-Object { $_.State -eq "Established" }

# Prepare a log entry
$log = @()

foreach ($conn in $connections) {
    $procId = $conn.OwningProcess
    $proc = Get-Process -Id $procId -ErrorAction SilentlyContinue
    $appName = if ($proc) { $proc.ProcessName } else { "Unknown" }

    $entry = @{
        Timestamp       = Get-Date
        Application     = $appName
        LocalAddress    = $conn.LocalAddress
        LocalPort       = $conn.LocalPort
        RemoteAddress   = $conn.RemoteAddress
        RemotePort      = $conn.RemotePort
        State           = $conn.State
    }

    $log += ($entry | Out-String)
}

# Save the log to a file
$log | Out-File -FilePath $logFile -Encoding UTF8

Write-Host "Network traffic logged to $logFile"
```