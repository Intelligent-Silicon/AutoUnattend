# AutoUnattend-Powershell-List-Services-Uninstallable


List all services and their Dependant Services.

Output to CSV

```
PS C:\WINDOWS\system32> Get-Service | Select-Object Status, Name, DisplayName, CanPauseAndContinue, CanShutdown, CanStop, StartType, ServiceType, @{Name='DependentServices';Expression={$_.DependentServices -join ';'}} | Export-Csv -Path "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.csv" -NoTypeInformation
```

Output to TXT

```
PS C:\WINDOWS\system32> Get-Service | Select-Object Status, Name, DisplayName, CanPauseAndContinue, CanShutdown, CanStop, StartType, ServiceType, @{Name='DependentServices';Expression={$_.DependentServices -join ';'}} | Out-File -FilePath "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.txt"
```

[Typical Services by Category](AutoUnattend-Services-Typical-Category.md)

[Typical Services by CSV](AutoUnattend-Services-Typical-AlphaNumeric.csv)



```
PS C:\WINDOWS\system32> Get-Service | Select-Object Name, DisplayName| Export-Csv -Path "C:\Users\Admin1\Documents\ISO Files\Services.ServiceName.csv" -NoTypeInformation
```




Set Service LanmanWorkstation with a new DisplayName.
```
Set-Service -Name LanmanWorkstation -DisplayName "LanMan Workstation"
Set-Service -Name AarSvc_47e64 -DisplayName "Agent Activation Runtime_47e64"
```

Prefix the DisplayName of selected Services to allow running.
Service StartType options =  Automatic, Automatic (Delayed), Manual, and Disabled
V = VirtualPC {A = Automatic|S = Automatic (Delayed Start)|M = Manual|D = Disabled|U = Uninstall}_{DisplayName}

This indicates the Service has been processed. Services not processed will have no V{A|S|M|D|U}_{DisplayName}

Services.StartUpOptions.VMware.CSV File will contain two columns, ServiceName & Prefix which will be used to feed in the prefix to a script which will change the DisplayName.

A second script will alter the StartType setting or uninstall it based on the DisplayName Prefix. Where no Prefix exists, nothing happens.


```
Get-Service -Name wuauserv |
  ForEach-Object {
    Set-Service -Name $_.Name -DisplayName ("MyPrefix - " + $_.DisplayName)
  }
```


```
PS C:\WINDOWS\system32> Get-Service | Out-File -FilePath "C:\mount\sources\install.esd.txt"
```
-DependentServices]
   [-RequiredServices

```
PS C:\WINDOWS\system32> Get-Service | Where-Object {$_.DependentServices} |
>>     Format-List -Property Name, DependentServices, @{
>>         Label="NoOfDependentServices"
>>         Expression={$_.DependentServices.Count}
>>     }

https://learn.microsoft.com/en-us/dotnet/api/system.serviceprocess.servicecontroller.servicename?view=net-9.0-pp



```
Get-Service | ForEach-Object {$service = $_; $dependents = Get-Service | Where-Object { $_.DependentServices -contains $service }; [PSCustomObject]@{ServiceDisplayName   = $service.DisplayName; DependentDisplayNames = ($service.DependentServices | ForEach-Object { $_.DisplayName }) -join ', ' } } | Format-Table -AutoSize
```

```
Get-Service | ForEach-Object {$service = $_; $dependents = Get-Service | Where-Object { $_.DependentServices -contains $service }; [PSCustomObject]@{ServiceDisplayName   = $service.ServiceName; DependentDisplayNames = ($service.DependentServices | ForEach-Object { $_.ServiceName }) -join ', ' } } | Format-Table -AutoSize
```
```
Get-Service | ForEach-Object {$service = $_; $dependents = Get-Service | Where-Object { $_.DependentServices -contains $service }; [PSCustomObject]@{ServiceDisplayName   = $service.ServiceName; DependentDisplayNames = ($service.DependentServices | ForEach-Object { $_.ServiceName }) -join ', ' } } | Format-Table -AutoSize | Out-File -FilePath "C:\Users\Admin1\Documents\ISO Files\Services.Dependants.txt"
```

```
Get-Service | ForEach-Object {  $service = $_; $dependents = Get-Service | Where-Object { $_.DependentServices -contains $service }; 
[PSCustomObject]@{
                    ServiceName   = $service.ServiceName;
                    DisplayName   = ($service.DisplayName ) -join ', ';
                    Description   = ($service.Description) -join ', '; 
                    Status   = ($service.Status ) -join ', '; 
                    StartupType             = ($service.StartupType       | ForEach-Object { $_.ServiceName }) -join ', '                     
                    DependentServiceNames   = ($service.DependentServices | ForEach-Object { $_.ServiceName }) -join ', ' 
                 }
                             } | Format-Table -AutoSize | Out-File -FilePath "C:\Users\Admin1\Documents\ISO Files\Services.Dependants.txt"
```


```
Get-Service | ForEach-Object {
    $service = $_
    $serviceConfig = Get-WmiObject -Class Win32_Service -Filter "Name = '$($service.Name)'"

    [PSCustomObject]@{
        ServiceName       = $service.Name
        DisplayName       = $service.DisplayName
        Description       = $serviceConfig.Description
        StartupType       = $serviceConfig.StartMode
        DependentServices = ($service.DependentServices | Select-Object -ExpandProperty Name) -join ", "
    }
} | Format-Table -AutoSize | Out-File -FilePath "C:\Users\Admin1\Documents\ISO Files\Services.Dependants.txt"
```

```
Get-Service | ForEach-Object {
    $service = $_
    $serviceConfig = Get-WmiObject -Class Win32_Service -Filter "Name = '$($service.Name)'"

    [PSCustomObject]@{
        ServiceName       = $service.Name
        DisplayName       = $service.DisplayName
        StartupType       = $serviceConfig.StartMode
        DependentServices = ($service.DependentServices | Select-Object -ExpandProperty Name) -join ", "
    }
} | Format-Table -AutoSize | Out-File -FilePath "C:\Users\Admin1\Documents\ISO Files\Services.Dependants.txt"
```

```
Get-Service | ForEach-Object {
    $service = $_
    $serviceConfig = Get-WmiObject -Class Win32_Service -Filter "Name = '$($service.Name)'"

    [PSCustomObject]@{
        ServiceName       = $service.Name
        DisplayName       = $service.DisplayName
        StartupType       = $serviceConfig.StartMode
        DependentServices = ($service.DependentServices | Select-Object -ExpandProperty Name) -join ", "
        Description       = $serviceConfig.Description
    }
} | Format-Table -AutoSize | Out-File -FilePath "C:\Users\Admin1\Documents\ISO Files\Services.Dependants.txt"
```

```
gsv | fl *
Get-Service | Format-List * 
Get-Service | Format-Table -AutoSize *
Get-Service | Format-Table -AutoSize * | Out-File -FilePath "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.txt"
Get-Service | Format-Table -Property ServiceName, Name, DisplayName, RequiredServices, ServicesDependedOn, Status, CanPauseAndContinue, CanShutdown, CanStop, ServiceType, StartType | Out-File -FilePath "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.txt"
Get-Service | Format-Table -Property Status, ServiceName, DisplayName, RequiredServices, CanPauseAndContinue, CanShutdown, CanStop, StartType, ServiceType | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.txt"

Get-Service | Where-Object { $_.StartType -match "Automatic" -or $_.StartType -match "Automatic (Delayed Start)" -or $_.StartType -match "Manual" } | Format-Table -Property Status, ServiceName, DisplayName, RequiredServices, CanPauseAndContinue, CanShutdown, CanStop, StartType, ServiceType | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.txt"
Get-Service | Where-Object { $_.Status -match "Running" } | Format-Table -Property Status, ServiceName, DisplayName, RequiredServices, CanPauseAndContinue, CanShutdown, CanStop, StartType, ServiceType | Export-Csv -Path "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.txt"

Get-Service | Where-Object { $_.StartType -match "Automatic" } | Format-Table -Property Status, ServiceName, DisplayName, RequiredServices, CanPauseAndContinue, CanShutdown, CanStop, StartType, ServiceType | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.txt"

Get-Service | Where-Object { $_.Status -match "Running" } | Format-Table -Property Status, ServiceName, DisplayName, RequiredServices, CanPauseAndContinue, CanShutdown, CanStop, StartType, ServiceType | Out-File -Width 1000 -FilePath "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.txt"

Get-Service | Where-Object { $_.Status -match "Running" } | Format-Table -Property Status, ServiceName, DisplayName, RequiredServices, CanPauseAndContinue, CanShutdown, CanStop, StartType, ServiceType | Export-Csv -Path "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.csv"


Get-Service | Select-Object -Property Name,DisplayName,Status | Export-Csv -Path "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.csv" -NoTypeInformation

Get-Service | Select-Object -Property * | Export-Csv -Path "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.csv" -NoTypeInformation

Get-Service | Where-Object { $_.Status -match "Running" } | Format-Table -Property Status, ServiceName, DisplayName, RequiredServices, CanPauseAndContinue, CanShutdown, CanStop, StartType, ServiceType | Export-Csv -Path "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.csv" -NoTypeInformation

Get-Service | Select-Object -Property Status, ServiceName, DisplayName, RequiredServices, CanPauseAndContinue, CanShutdown, CanStop, StartType, ServiceType | Export-Csv -Path "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.csv" -NoTypeInformation

Get-Service | Select-Object Status, Name, DisplayName, CanPauseAndContinue, CanShutdown, CanStop, StartType, ServiceType, @{Name='DependentServices';Expression={$_.DependentServices -join ';'}} | Export-Csv -Path "C:\Users\Admin1\Documents\ISO Files\Services.AllProperties.csv" -NoTypeInformation
```

Announcing Zero Trust DNS Private Preview
https://techcommunity.microsoft.com/blog/networkingblog/announcing-zero-trust-dns-private-preview/4110366

| Out-File -FilePath "C:\mount\sources\install.esd.txt"


```
Get-Service | ForEach-Object {
    $service = $_
    $dependents = Get-Service | Where-Object { $_.DependentServices -contains $service }
    
    [PSCustomObject]@{
        ServiceDisplayName   = $service.DisplayName
        DependentDisplayNames = ($service.DependentServices | ForEach-Object { $_.DisplayName }) -join ', '
    }
} | Format-Table -AutoSize
```



PS C:\WINDOWS\system32> Get-Service | Where-Object {$_.DependentServices} | ForEach-Object {$_.DependentServices.DisplayName $_.DependentServices}


PS C:\WINDOWS\system32> Get-Service | Where-Object {$_.DependentServices} | ForEach-Object {$_.DependentServices.DisplayName $_.DependentServices} | Format-List -Property Name, DependentServices, @{Label="NoOfDependentServices"; Expression={$_.DependentServices.Count} }, @{Label="DependentServiceDisplayName"; Expression={$_.DependentServices.DisplayName} }

PS C:\WINDOWS\system32> Get-Service | Where-Object {$_.DependentServices} | 
    Format-List -Property Name, DependentServices, 
    @{
    Label="NoOfDependentServices";
    Expression={$_.DependentServices.Count}
    }, 
    @{
    Label="DisplayName"; 
    Expression={
    foreach ($_.DependentServices.DisplayName in $_.DependentServices);
    {$_.DependentServices.DisplayName}
    }}  
   
   foreach ($_.DependentServices.DisplayName in $_.DependentServices);
    {$_.DependentServices.DisplayName}
    }}
    
    
   Import-csv C:\filename.csv | Where-Object {$_.ExternalEmailAddress -ne "" } | ForEach-Object { New-MailContact -Name $_.Name -ExternalEmailAddress $_.ExternalEmailAddress }
   
   
$letterArray = 'a','b','c','d'
foreach ($letter in $letterArray)
{
  Write-Host $letter
}


```
PS C:\WINDOWS\system32> Get-Service

Status   Name               DisplayName
------   ----               -----------
Stopped  AarSvc_471cf       Agent Activation Runtime_471cf
Stopped  ALG                Application Layer Gateway Service
Stopped  AppIDSvc           Application Identity
Running  Appinfo            Application Information
Stopped  AppReadiness       App Readiness
```

```
PS C:\WINDOWS\system32> Get-Service "wmi*"

Status   Name               DisplayName
------   ----               -----------
Stopped  wmiApSrv           WMI Performance Adapter
Running  WMIRegistration... Intel(R) Management Engine WMI Prov...
```

```
PS C:\WINDOWS\system32> Get-Service -DisplayName "*network*"

Status   Name               DisplayName
------   ----               -----------
Stopped  NcaSvc             Network Connectivity Assistant
Running  NcbService         Network Connection Broker
Stopped  NcdAutoSetup       Network Connected Devices Auto-Setup
Stopped  Netman             Network Connections
Running  netprofm           Network List Service
Stopped  NetSetupSvc        Network Setup Service
Stopped  NlaSvc             Network Location Awareness
Running  nsi                Network Store Interface Service
Running  WdNisSvc           Microsoft Defender Antivirus Networ...
Stopped  XboxNetApiSvc      Xbox Live Networking Service
```

```
PS C:\WINDOWS\system32> Get-Service -Name "win*" -Exclude "WinRM"

Status   Name               DisplayName
------   ----               -----------
Running  WinDefend          Microsoft Defender Antivirus Service
Running  WinHttpAutoProx... WinHTTP Web Proxy Auto-Discovery Se...
Running  Winmgmt            Windows Management Instrumentation
```

```
PS C:\WINDOWS\system32> Get-Service | Where-Object {$_.Status -eq "Running"}

Status   Name               DisplayName
------   ----               -----------
Running  Appinfo            Application Information
Running  AppXSvc            AppX Deployment Service (AppXSVC)
Running  AudioEndpointBu... Windows Audio Endpoint Builder
Running  Audiosrv           Windows Audio
```

```
PS C:\WINDOWS\system32> Get-Service | Where-Object {$_.DependentServices} |
>>     Format-List -Property Name, DependentServices, @{
>>         Label="NoOfDependentServices"
>>         Expression={$_.DependentServices.Count}
>>     }


Name                  : AppIDSvc
DependentServices     : {applockerfltr}
NoOfDependentServices : 1

Name                  : AudioEndpointBuilder
DependentServices     : {AarSvc_471cf, RtkAudioUniversalService, AarSvc, Audiosrv}
NoOfDependentServices : 4

Name                  : Audiosrv
DependentServices     : {AarSvc_471cf, RtkAudioUniversalService, AarSvc}
NoOfDependentServices : 3

Name                  : BFE
DependentServices     : {ZTDNS, XboxNetApiSvc, webthreatdefsvc, wtd...}
NoOfDependentServices : 12
```

```
PS C:\WINDOWS\system32> Get-Service "s*" | Sort-Object Status

Status   Name               DisplayName
------   ----               -----------
Stopped  SensrSvc           Sensor Monitoring Service
Stopped  SessionEnv         Remote Desktop Configuration
Stopped  SensorDataService  Sensor Data Service
Stopped  sppsvc             Software Protection
```

```
PS C:\WINDOWS\system32> Get-Service "WinRM" -RequiredServices

Status   Name               DisplayName
------   ----               -----------
Running  NSI                Network Store Interface Service
Running  HTTP               HTTP Service
Running  RPCSS              Remote Procedure Call (RPC)
```

```
PS C:\WINDOWS\system32> "WinRM" | Get-Service

Status   Name               DisplayName
------   ----               -----------
Stopped  WinRM              Windows Remote Management (WS-Manag...
```

```
PS C:\WINDOWS\system32> Remove-Service -Name "TestService"
Remove-Service : The term 'Remove-Service' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path
is correct and try again.
At line:1 char:1
+ Remove-Service -Name "TestService"
+ ~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (Remove-Service:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
    
```

```
Get-Service -DisplayName "Test Service" | Remove-Service
```

```
New-Service -Name "TestService" -BinaryPathName 'C:\WINDOWS\System32\svchost.exe -k netsvcs'
```

```
$params = @{
  Name = "TestService"
  BinaryPathName = 'C:\WINDOWS\System32\svchost.exe -k netsvcs'
  DependsOn = "NetLogon"
  DisplayName = "Test Service"
  StartupType = "Manual"
  Description = "This is a test service."
}
New-Service @params
```
```
Get-CimInstance -ClassName Win32_Service -Filter "Name='testservice'"

ExitCode  : 0
Name      : testservice
ProcessId : 0
StartMode : Auto
State     : Stopped
Status    : OK
```

```
$SDDL = "D:(A;;CCLCSWRPWPDTLOCRRC;;;SY)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)(A;;CCLCSWLOCRRC;;;SU)"
$params = @{
  BinaryPathName = 'C:\WINDOWS\System32\svchost.exe -k netsvcs'
  DependsOn = "NetLogon"
  DisplayName = "Test Service"
  StartupType = "Manual"
  Description = "This is a test service."
  SecurityDescriptorSddl = $SDDL
}
New-Service @params
```

```
Set-Service -Name LanmanWorkstation -DisplayName "LanMan Workstation"
```

```
Set-Service -Name BITS -StartupType Automatic
Get-Service BITS | Select-Object -Property Name, StartType, Status
```

```
Get-CimInstance Win32_Service -Filter 'Name = "BITS"'  | Format-List  Name, Description

Name        : BITS
Description : Transfers files in the background using idle network bandwidth. If the service is
              disabled, then any applications that depend on BITS, such as Windows Update or MSN
              Explorer, will be unable to automatically download programs and other information.

Set-Service -Name BITS -Description "Transfers files in the background using idle network bandwidth."
Get-CimInstance Win32_Service -Filter 'Name = "BITS"' | Format-List  Name, Description

Name        : BITS
Description : Transfers files in the background using idle network bandwidth.
```

```
Set-Service -Name WinRM -Status Running -PassThru

Status   Name               DisplayName
------   ----               -----------
Running  WinRM              Windows Remote Management (WS-Manag...
```

```
Get-Service -Name Schedule | Set-Service -Status Paused
```

```
$S = Get-Service -Name Schedule
Set-Service -InputObject $S -Status Stopped
```

```
$Cred = Get-Credential
$S = Get-Service -Name Schedule
Invoke-Command -ComputerName server01.contoso.com -Credential $Cred -ScriptBlock {
  Set-Service -InputObject $S -Status Stopped
}
```

```
$credential = Get-Credential
Set-Service -Name Schedule -Credential $credential
```

```
$SDDL = "D:(A;;CCLCSWRPWPDTLOCRRC;;;SY)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)(A;;CCLCSWLOCRRC;;;SU)"
Set-Service -Name "BITS" -SecurityDescriptorSddl $SDDL
```

```
Get-Service SQLWriter,spooler |
    Set-Service -StartupType Automatic -PassThru |
    Select-Object Name, StartType

Name      StartType
----      ---------
spooler   Automatic
SQLWriter Automatic
```

