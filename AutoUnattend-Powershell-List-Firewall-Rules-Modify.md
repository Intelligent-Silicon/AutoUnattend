
```
PS C:\WINDOWS\system32> Get-NetFirewallRule -PolicyStore ActiveStore | more


Name                          : NETDIS-UPnPHost-Out-TCP
DisplayName                   : Network Discovery (UPnP-Out)
Description                   : Outbound rule for Network Discovery to allow use of Universal Plug and Play. [TCP]
DisplayGroup                  : Network Discovery
Group                         : @FirewallAPI.dll,-32752
Enabled                       : False
Profile                       : Public
Platform                      : {}
Direction                     : Outbound
Action                        : Allow
EdgeTraversalPolicy           : Block
LooseSourceMapping            : False
LocalOnlyMapping              : False
Owner                         :
PrimaryStatus                 : Inactive
Status                        : The rule was parsed successfully from the store. (65536)
EnforcementStatus             : OptimizedOut
PolicyStoreSource             : PersistentStore
PolicyStoreSourceType         : Local
RemoteDynamicKeywordAddresses : {}
PolicyAppId                   :
PackageFamilyName             :
```

```
PS C:\WINDOWS\system32> Get-NetFirewallProfile -Name Public | Get-NetFirewallRule | more


Name                          : NETDIS-UPnPHost-Out-TCP
DisplayName                   : Network Discovery (UPnP-Out)
Description                   : Outbound rule for Network Discovery to allow use of Universal Plug and Play. [TCP]
DisplayGroup                  : Network Discovery
Group                         : @FirewallAPI.dll,-32752
Enabled                       : False
Profile                       : Public
Platform                      : {}
Direction                     : Outbound
Action                        : Allow
EdgeTraversalPolicy           : Block
LooseSourceMapping            : False
LocalOnlyMapping              : False
Owner                         :
PrimaryStatus                 : OK
Status                        : The rule was parsed successfully from the store. (65536)
EnforcementStatus             : NotApplicable
PolicyStoreSource             : PersistentStore
PolicyStoreSourceType         : Local
RemoteDynamicKeywordAddresses : {}
PolicyAppId                   :
PackageFamilyName             :
```

```
Remove-NetFirewallRule
```

```
Remove-NetFirewallRule -DisplayName "Network Discovery (NB-Name-In)"
```

```
Remove-NetFirewallRule -Enabled False -PolicyStore contoso.com\gpo_name
```

```
$fwAppFilter = Get-NetFirewallApplicationFilter -Program "C:\Program Files (x86)\Messenger\msmsgs.exe"
Remove-NetFirewallRule -InputObject $fwAppFilter
```

```
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True
```

```
Set-NetFirewallProfile -DefaultInboundAction Block -DefaultOutboundAction Allow -NotifyOnListen False -AllowUnicastResponseToMulticast True -LogFileName %SystemRoot%\System32\LogFiles\Firewall\pfirewall.log
```

```
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False
```

```
New-NetFirewallRule -DisplayName "Allow Inbound Telnet" -Direction Inbound -Program %SystemRoot%\System32\tlntsvr.exe -RemoteAddress LocalSubnet -Action Allow
```

```
New-NetFirewallRule -DisplayName "Block Outbound Telnet" -Direction Outbound -Program %SystemRoot%\System32\tlntsvr.exe -Protocol TCP -LocalPort 23 -Action Block -PolicyStore domain.contoso.com\gpo_name
```

```
Set-NetFirewallRule -DisplayName "Allow Web 80" -RemoteAddress 192.168.0.2
```

```
Get-NetFirewallPortFilter | ?{$_.LocalPort -eq 80} | Get-NetFirewallRule | ?{ $_.Direction -eq "Inbound" -and $_.Action -eq "Allow"} | Set-NetFirewallRule -RemoteAddress 192.168.0.2
```

```
Get-NetFirewallApplicationFilter -Program "*svchost*" | Get-NetFirewallRule
```

```
Remove-NetFirewallRule -DisplayName "Allow Web 80"
```

```
Remove-NetFirewallRule -Action Block
```
