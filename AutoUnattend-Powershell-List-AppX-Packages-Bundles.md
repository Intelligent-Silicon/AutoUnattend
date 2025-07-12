# AppX Packages and Bundles

```
Get-AppxPackage -Allusers | Format-Table -AutoSize | Out-File -width 1000 -FilePath "C:\Users\Admin1\Documents\ISO Files\WindowsAppX.AllUsers.txt"

Name                                           Publisher                                                                        PublisherId   Architecture ResourceId Version           PackageFamilyName                                            PackageFullName                                                                         InstallLocation                                                                                            IsFramework
----                                           ---------                                                                        -----------   ------------ ---------- -------           -----------------                                            ---------------                                                                         ---------------                                                                                            -----------
1527c705-839a-4832-9118-54d4Bd6a0c89           CN=Microsoft Windows, O=Microsoft Corporation, L=Redmond, S=Washington, C=US     cw5n1h2txyewy      Neutral neutral    10.0.19640.1000   1527c705-839a-4832-9118-54d4Bd6a0c89_cw5n1h2txyewy           1527c705-839a-4832-9118-54d4Bd6a0c89_10.0.19640.1000_neutral_neutral_cw5n1h2txyewy      C:\Windows\SystemApps\Microsoft.Windows.FilePicker_cw5n1h2txyewy                                                 False
c5e2524a-ea46-4f67-841f-6a9465d9d515           CN=Microsoft Windows, O=Microsoft Corporation, L=Redmond, S=Washington, C=US     cw5n1h2txyewy      Neutral neutral    10.0.26100.1      c5e2524a-ea46-4f67-841f-6a9465d9d515_cw5n1h2txyewy           c5e2524a-ea46-4f67-841f-6a9465d9d515_10.0.26100.1_neutral_neutral_cw5n1h2txyewy         C:\Windows\SystemApps\Microsoft.Windows.FileExplorer_cw5n1h2txyewy                                               False
E2A4F912-2574-4A75-9BB0-0D023378592B           CN=Microsoft Windows, O=Microsoft Corporation, L=Redmond, S=Washington, C=US     cw5n1h2txyewy      Neutral neutral    10.0.19640.1000   E2A4F912-2574-4A75-9BB0-0D023378592B_cw5n1h2txyewy           E2A4F912-2574-4A75-9BB0-0D023378592B_10.0.19640.1000_neutral_neutral_cw5n1h2txyewy      C:\Windows\SystemApps\Microsoft.Windows.AppResolverUX_cw5n1h2txyewy                                              False
F46D4000-FD22-4DB4-AC8E-4E1DDDE828FE           CN=Microsoft Windows, O=Microsoft Corporation, L=Redmond, S=Washington, C=US     cw5n1h2txyewy      Neutral neutral    10.0.26100.1      F46D4000-FD22-4DB4-AC8E-4E1DDDE828FE_cw5n1h2txyewy           F46D4000-FD22-4DB4-AC8E-4E1DDDE828FE_10.0.26100.1_neutral_neutral_cw5n1h2txyewy         C:\Windows\SystemApps\Microsoft.Windows.AddSuggestedFoldersToLibraryDialog_cw5n1h2txyewy                         False
Microsoft.AccountsControl                      CN=Microsoft Windows, O=Microsoft Corporation, L=Redmond, S=Washington, C=US     cw5n1h2txyewy      Neutral            10.0.26100.1      Microsoft.AccountsControl_cw5n1h2txyewy                      Microsoft.AccountsControl_10.0.26100.1_neutral__cw5n1h2txyewy                           C:\Windows\SystemApps\Microsoft.AccountsControl_cw5n1h2txyewy                                                    False
Microsoft.AsyncTextService                     CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US 8wekyb3d8bbwe      Neutral            10.0.26100.1      Microsoft.AsyncTextService_8wekyb3d8bbwe                     Microsoft.AsyncTextService_10.0.26100.1_neutral__8wekyb3d8bbwe                          C:\Windows\SystemApps\Microsoft.AsyncTextService_8wekyb3d8bbwe                                                   False
Microsoft.BioEnrollment                        CN=Microsoft Windows, O=Microsoft Corporation, L=Redmond, S=Washington, C=US     cw5n1h2txyewy      Neutral            10.0.19587.1000   Microsoft.BioEnrollment_cw5n1h2txyewy                        Microsoft.BioEnrollment_10.0.19587.1000_neutral__cw5n1h2txyewy                          C:\Windows\SystemApps\Microsoft.BioEnrollment_cw5n1h2txyewy                                                      False
Microsoft.CredDialogHost                       CN=Microsoft Windows, O=Microsoft Corporation, L=Redmond, S=Washington, C=US     cw5n1h2txyewy      Neutral            10.0.19595.1001   Microsoft.CredDialogHost_cw5n1h2txyewy                       Microsoft.CredDialogHost_10.0.19595.1001_neutral__cw5n1h2txyewy                         C:\Windows\SystemApps\microsoft.creddialoghost_cw5n1h2txyewy                                                     False
```

PowerShell's Remove-AppxPackage cmdlet is commonly used, especially for built-in Windows Store apps. 
You can also use Uninstall-Package for other types of packages, but it requires specifying the package provider  (e.g., msi, msu, Programs, PowerShellGet).

```
Get-AppxPackage -AllUsers | Remove-AppxPackage -AllUsers
```

get-appxprovisionedpackage -path "yourmountdirhere" | out-file .\apps.txt

get-appxprovisionedpackage -path "yourmountdirhere" | out-file .\apps.txt


https://github.com/Sycnex/Windows10Debloater
Script/Utility/Application to debloat Windows 10, to remove Windows pre-installed unnecessary applications, stop some telemetry functions, 
stop Cortana from being used as your Search Index, disable unnecessary scheduled tasks, and more...