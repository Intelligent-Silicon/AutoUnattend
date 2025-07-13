# Window 10

Audit PC
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

```

PS C:\Users\Admin1> Get-CimInstance Win32_OperatingSystem | Select-Object Caption

Caption
-------
Microsoft Windows 11 Home

SystemDirectory     Organization BuildNumber RegisteredUser SerialNumber            Version
---------------     ------------ ----------- -------------- ------------            -------
C:\WINDOWS\system32              26100                      00356-07439-96876-AAOEM 10.0.26100
```

```
md "C:\Users\Admin1\Documents\Windows10_Enterprise_LTSC"
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



GUI
```
Add-Type -AssemblyName System.Windows.Forms

# Create the form
$form = New-Object System.Windows.Forms.Form
$form.Text = "Network Traffic Logger"
$form.Size = New-Object System.Drawing.Size(400,200)
$form.StartPosition = "CenterScreen"

# Create a button
$button = New-Object System.Windows.Forms.Button
$button.Text = "Log Network Traffic"
$button.Size = New-Object System.Drawing.Size(200,40)
$button.Location = New-Object System.Drawing.Point(100,50)

# Status label
$label = New-Object System.Windows.Forms.Label
$label.Text = "Press the button to start logging."
$label.AutoSize = $true
$label.Location = New-Object System.Drawing.Point(100,110)

$form.Controls.Add($button)
$form.Controls.Add($label)

# Function to log network traffic
$button.Add_Click({
    $timestamp = Get-Date -Format "yyyy-MM-dd_HH-mm-ss"
    $logFile = "NetworkTrafficLog_$timestamp.txt"
    $connections = Get-NetTCPConnection | Where-Object { $_.State -eq "Established" }
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

    $log | Out-File -FilePath $logFile -Encoding UTF8
    $label.Text = "Log saved: $logFile"
})

# Run the form
[void]$form.ShowDialog()
```