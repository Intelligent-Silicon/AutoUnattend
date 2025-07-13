# Window 10

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

    # Safely collect dependent services
    $dependentList = @()
    foreach ($dep in $service.DependentServices) {
        $dependentList += $dep.DisplayName
    }
    $dependentServices = if ($dependentList.Count -gt 0) { $dependentList -join ', ' } else { 'None' }

    [PSCustomObject]@{
        ServiceName           = $serviceName
        DisplayName           = $displayName
        Status                = $status
        StartupType           = $startMode
        LogonAccount          = $logonAccount
        DependentServices     = $dependentServices
        FirstFailureAction    = if ($recovery.PSObject.Properties['FirstFailure'])  { $recovery.FirstFailure } else { 'N/A' }
        SecondFailureAction   = if ($recovery.PSObject.Properties['SecondFailure']) { $recovery.SecondFailure } else { 'N/A' }
        ThirdFailureAction    = if ($recovery.PSObject.Properties['ThirdFailure'])  { $recovery.ThirdFailure } else { 'N/A' }
        ResetPeriodInSeconds  = if ($recovery.PSObject.Properties['ResetPeriod'])   { $recovery.ResetPeriod } else { 'N/A' }
        RebootMessage         = if ($recovery.PSObject.Properties['RebootMessage']) { $recovery.RebootMessage } else { 'N/A' }
        CommandLineAction     = if ($recovery.PSObject.Properties['Command'])       { $recovery.Command } else { 'N/A' }
    }
} | Format-Table -AutoSize | Out-File -width 2000 -FilePath "C:\Users\Admin1\Documents\Services.AllProperties.txt" 
```
```
Get-Service | ForEach-Object {
    $service = $_
    $serviceName = $service.Name
    $regPath = "HKLM:\SYSTEM\CurrentControlSet\Services\$serviceName"

    # Recovery info from registry
    $recovery = Get-ItemProperty -Path $regPath -ErrorAction SilentlyContinue
    echo $recovery 
    # Extended details via WMI
    $wmi = Get-WmiObject -Class Win32_Service -Filter "Name='$serviceName'"
    $displayName  = $wmi.DisplayName
    $logonAccount = $wmi.StartName
    $startMode    = $wmi.StartMode
    $status       = $wmi.State

    # Collect dependent services by ServiceName
    $dependentList = @()
    foreach ($dep in $service.DependentServices) {
        $dependentList += $dep.Name
    }
    $dependentServices = if ($dependentList.Count -gt 0) { $dependentList -join ', ' } else { 'None' }

    [PSCustomObject]@{
        ServiceName           = $serviceName
        DisplayName           = $displayName
        Status                = $status
        StartupType           = $startMode
        LogonAccount          = $logonAccount
        DependentServices     = $dependentServices
        FirstFailureAction    = if ($recovery.PSObject.Properties['FirstFailure'])  { $recovery.FirstFailure } else { 'N/A' }
        SecondFailureAction   = if ($recovery.PSObject.Properties['SecondFailure']) { $recovery.SecondFailure } else { 'N/A' }
        ThirdFailureAction    = if ($recovery.PSObject.Properties['ThirdFailure'])  { $recovery.ThirdFailure } else { 'N/A' }
        ResetPeriodInSeconds  = if ($recovery.PSObject.Properties['ResetPeriod'])   { $recovery.ResetPeriod } else { 'N/A' }
        RebootMessage         = if ($recovery.PSObject.Properties['RebootMessage']) { $recovery.RebootMessage } else { 'N/A' }
        CommandLineAction     = if ($recovery.PSObject.Properties['Command'])       { $recovery.Command } else { 'N/A' }
    }
} | Format-Table -AutoSize | Out-File -width 2000 -FilePath "C:\Users\Admin1\Documents\Services.AllProperties.txt" 
```



List all Software

```
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Services.AllProperties.txt" 
```

List All Tasks

```
PS C:\WINDOWS\system32> Get-ScheduledTask | Format-Table -AutoSize  | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Tasks.Scheduled.txt"
```

List All Tasks with Info
```
# Get all scheduled tasks
$tasks = Get-ScheduledTask

$taskInfoList = foreach ($task in $tasks) {
    try {
        $info = Get-ScheduledTaskInfo -TaskName $task.TaskName -TaskPath $task.TaskPath
        [PSCustomObject]@{
            TaskName        = $task.TaskName
            TaskPath        = $task.TaskPath
            LastRunTime     = $info.LastRunTime
            NextRunTime     = $info.NextRunTime
            LastTaskResult  = $info.LastTaskResult
            LastStatus      = $info.LastStatus
        }
    } catch {
        [PSCustomObject]@{
            TaskName        = $task.TaskName
            TaskPath        = $task.TaskPath
            LastRunTime     = "N/A"
            NextRunTime     = "N/A"
            LastTaskResult  = "N/A"
            LastStatus      = "Failed to retrieve info"
        }
    }
}

# Display results in a table
$taskInfoList | Format-Table -AutoSize | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Tasks.Scheduled.Info.txt"
```
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
            LastStatus      = "Failed to retrieve info"
        }
    }
}

# Display results in a table
$taskInfoList | Format-Table -AutoSize | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Tasks.Scheduled.Info.txt"
```


List Startups
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
$startupEntries | Format-Table -AutoSize

```


List Firewall Rules

```
$profiles = @('Private', 'Public', 'Domain')

foreach ($profile in $profiles) {
    Write-Host "`n=== $profile Profile Firewall Rules ===" -ForegroundColor Cyan

    Get-NetFirewallRule |
        Where-Object { $_.Profile -contains $profile } |
        Select-Object DisplayName, Direction, Action, Enabled, Profile |
        Sort-Object DisplayName       
} Format-Table -AutoSize | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\Firewall.Rules.txt"
```
```
$profiles = @('Private', 'Public', 'Domain')

foreach ($profile in $profiles) {
    $rules = Get-NetFirewallRule |
        Where-Object { $_.Profile -contains $profile } |
        Select-Object DisplayName, Direction, Action, Enabled, Profile |
        Sort-Object DisplayName

    $header = "`n=== $profile Profile Firewall Rules ===`n"
    $outputPath = "$env:USERPROFILE\Desktop\FirewallRules_$profile.txt"

    $header | Out-File -FilePath $outputPath -Encoding UTF8
    $rules | Out-String | Out-File -width 1000 -Append -Encoding UTF8 -FilePath "C:\Users\Admin1\Documents\Firewall.Rules.txt"
}
```

```
# List all firewall rules
Get-NetFirewallRule |
Select-Object Name, DisplayName, Enabled, Direction, Action, Profile |
Format-Table -AutoSize | Out-File -width 1000 -Encoding UTF8 -FilePath "C:\Users\Admin1\Documents\Firewall.Rules.txt"
```


Lockdown Firewall allowing only Github and Firefox
```
# Step 1: Block all outbound traffic
Set-NetFirewallProfile -Profile Domain,Private,Public -DefaultOutboundAction Block

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
Set-NetFirewallProfile -LogBlocked -LogFileName "C:\FirewallLogs.txt"
```

Backup Existing Firewall Rules
```
# Export current firewall rules to a file
$backupPath = "C:\Users\Admin1\Documents\Win 11 Home\FirewallBackup.wfw"
netsh advfirewall export $backupPath
```


Import Firewall Rules - Overwrites existing rules.
```
# Import firewall rules to a file
$backupPath = "C:\Users\Admin1\Documents\Win 11 Home\FirewallBackup.wfw"
netsh advfirewall import $backupPath
```


