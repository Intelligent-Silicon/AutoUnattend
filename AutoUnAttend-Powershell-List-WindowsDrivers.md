# List Drivers 


List Drivers All

```
PS C:\WINDOWS\system32> Get-WindowsDriver -Online -All | Format-Table -AutoSize | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\ISO Files\WindowsDrivers.All.AllProperties.txt"
```

List Boot Critical Drivers

```
PS C:\WINDOWS\system32> Get-WindowsDriver -Online -All | Where-Object { $_.BootCritical -match "True" } | Format-Table -AutoSize | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\ISO Files\WindowsDrivers.AllProperties.BootCritical.True.txt"
```

List Boot Critical Drivers Unsigned 

```
PS C:\WINDOWS\system32> Get-WindowsDriver -Online -All | Where-Object { $_.BootCritical -match "True" -and $_.DriverSignature -match "Unsigned"} | Format-Table -AutoSize | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\ISO Files\WindowsDrivers.AllProperties.BootCritical.True.Unsigned.txt"
```

```



DriverSignature
```
# Get installed updates
$updates = Get-WmiObject -Class "Win32_QuickFixEngineering" | Where-Object { $_.HotFixID -like "KB*" }

# Add parsed KB number for sorting
$updates = $updates | Select-Object HotFixID, Description, InstalledOn, @{
    Name = 'KBNumber'; Expression = {
        [int]($_.HotFixID -replace '[^0-9]', '')
    }
}

# Group by Description (common title/name)
$grouped = $updates | Group-Object -Property Description

# Display each group sorted by KB number (oldest first)
foreach ($group in $grouped) {
    Write-Host "`n=== $($group.Name) ==="

    $group.Group | Sort-Object KBNumber | ForEach-Object {
        Write-Host "$($_.HotFixID) - Installed: $($_.InstalledOn)"
    }
}
```


```
# Get all installed Windows Defender updates
$defenderUpdates = Get-WmiObject -Class Win32_QuickFixEngineering | Where-Object {
    $_.HotFixID -like "KB*"
}

# Filter Security Intelligence updates only
$securityIntelligenceUpdates = $defenderUpdates | Where-Object {
    $_.Description -match "Security Intelligence Update"
}

# Sort updates by InstalledOn date
$sortedUpdates = $securityIntelligenceUpdates | Sort-Object InstalledOn

# Keep the newest update
$latestUpdate = $sortedUpdates | Select-Object -Last 1

# Uninstall all other updates
$updatesToRemove = $sortedUpdates | Where-Object { $_.HotFixID -ne $latestUpdate.HotFixID }

foreach ($update in $updatesToRemove) {
    Write-Host "Attempting to uninstall $($update.HotFixID)..." -ForegroundColor Yellow
    wusa /uninstall /kb:$($update.HotFixID.Replace("KB", "")) /quiet /norestart
}

```


Uninstall old KB's

# Ensure the required module is available
Import-Module PSWindowsUpdate

# Get list of all installed updates
$installedUpdates = Get-WmiObject -Class "Win32_QuickFixEngineering"

# Filter updates that are not hotfixes and have KB IDs
$kbUpdates = $installedUpdates | Where-Object { $_.HotFixID -like "KB*" }

# Placeholder list of superseded KBs (ideally pulled from an external list or database)
$supersededKBs = @(
    "KB5000802",
    "KB5000808",
    "KB5000842"
    # Add more KBs known to be superseded
)

# Loop through and uninstall updates matching the superseded list
foreach ($kb in $kbUpdates) {
    if ($supersededKBs -contains $kb.HotFixID) {
        Write-Host "Attempting to uninstall $($kb.HotFixID)..."
        wusa.exe /uninstall /kb:$($kb.HotFixID.Substring(2)) /quiet /norestart
    }
}



# Sample script to download a JSON list
$kbListUrl = "https://raw.githubusercontent.com/SomeUser/SupersededKBs/main/superseded_kbs.json"
$response = Invoke-WebRequest -Uri $kbListUrl
$kbData = $response.Content | ConvertFrom-Json

foreach ($kb in $kbData) {
    Write-Host "Superseded KB: $($kb.id)"
}

$kbListUrl = "https://raw.githubusercontent.com/SomeUser/SupersededKBs/main/superseded_kbs.json"
$response = Invoke-WebRequest -Uri $kbListUrl
$kbDataAll = $response | ConvertFrom-Json

foreach ($kb in $kbData) {
    Write-Host "Superseded KB: $($kb.id)"
}


# Configuration
$kbInsightUrl = "http://localhost:8501/kb-checker"  # Replace with actual KB Insight endpoint if hosted remotely
$installedUpdates = Get-WmiObject -Class "Win32_QuickFixEngineering" | Where-Object { $_.HotFixID -like "KB*" }

foreach ($kb in $installedUpdates) {
    $kbID = $kb.HotFixID
    Write-Host "Checking supersedence status of $kbID..."

    # Build the query URL for KB Insight API if exposed
    $response = Invoke-WebRequest -Uri "$kbInsightUrl?kb=$kbID" -UseBasicParsing

    # Assuming KB Insight returns JSON like { "kb": "KB5000802", "superseded": true }
    $kbData = $response.Content | ConvertFrom-Json

    if ($kbData.superseded -eq $true) {
        Write-Host "$kbID is superseded. Attempting to uninstall..."
        wusa.exe /uninstall /kb:$($kbID.Substring(2)) /quiet /norestart
    } else {
        Write-Host "$kbID is current. Skipping..."
    }
}


