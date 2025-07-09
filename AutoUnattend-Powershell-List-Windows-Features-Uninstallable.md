| Feature | Initial State | Vmware Guest Lockdown State | Host/Main PC Lockdown State | Comments | Website |
| --- | --- | --- | --- | --- | --- | 
| Windows-Defender-Default-Definitions | Enabled | Enabled | Enabled | Disabled when newer definitions are downloaded and installed. | [Reddit.com](https://www.reddit.com/r/windows/comments/ssay3k/windowsdefenderdefaultdefenitions_was/) |
| Printing-XPSServices-Features | Disabled | Disabled | Disabled | Microsoft XPS Document Writer, which allows users to save documents as XPS files (an XML-based document format), part of the broader XPS Services. | [Microsoft.com](https://learn.microsoft.com/en-us/windows-hardware/drivers/print/xps-printing-features) |
| TelnetClient | Disabled | Disabled | Disabled | Software that enables users to interact with a remote computer via a text-based interface, essentially acting as a terminal emulator. See [Putty.org](https://www.putty.org/) for an alternative Telnet + more client. | [Microsoft.com](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/telnet)  | 
| TFTP | Disabled | Disabled | Disabled | Trivial File Transfer Protocol (UDP), is a simple network protocol used for transferring files between a client and a server. TFTP does not support features like user authentication, directory listing, or file deletion | [Microsoft.com](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/tftp) |
| TIFFIFilter | Disabled | Disabled | Disabled | Windows TIFF IFilter performs optical character recognition (OCR) processing of TIFF images, and then it provides the recognized text to the caller to build the search index | [Microsoft.com](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-7/dd755985(v=ws.10)) |
| VirtualMachinePlatform | Disabled | Disabled | Enabled | Disabled Provides the underlying platform support for running virtual machines, WSL 2 (Windows Subsystem for Linux) & used to create MSIX Application packages for an App-V or MSI. May require [Virtualisation switched on in UEFI bios](https://support.microsoft.com/en-au/windows/enable-virtualization-on-windows-c5578302-6e43-4b4b-a449-8ced115f58e1). | [Microsoft.com](https://support.microsoft.com/en-us/windows/options-to-optimize-gaming-performance-in-windows-11-a255f612-2949-4373-a566-ff6f3f474613#:~:text=Microsoft%20uses%20virtualization%20in%20Windows,to%20turn%20off%20these%20features.) |
| Windows-Identity-Foundation | Disabled | Disabled | Disabled | Windows Identity Foundation (WIF) is a new extension to the Microsoft .NET Framework that makes it easy for developers to enable advanced identity capabilities in the .NET Framework applications | [Microsoft.com](https://learn.microsoft.com/en-us/previous-versions/troubleshoot/dotnet/framework/windows-identity-foundation) |
| Client-ProjFS | Disabled | Disabled | Disabled | The Windows Projected File System (ProjFS) allows a user-mode application called a "provider" to project hierarchical data from a backing data store into the file system, making it appear as files and directories in the file system. | [Microsoft.com](https://learn.microsoft.com/en-us/windows/win32/projfs/projected-file-system) | 
| SimpleTCP | Disabled | Disabled | Disabled | System service name: SimpTcp. Simple TCP/IP Services implements support for these protocols: Echo, port 7, RFC 862; Discard, port 9, RFC 863; Character Generator, port 19, RFC 864; Daytime, port 13, RFC 867; Quote of the Day, port 17, RFC 865. | [Microsoft.com](https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/service-overview-and-network-port-requirements#simple-tcpip-services) |

 

```
PS C:\WINDOWS\system32> Get-WindowsOptionalFeature -Online




FeatureName : Windows-Defender-Default-Definitions
State       : Disabled

FeatureName : Printing-XPSServices-Features
State       : Disabled

FeatureName : TelnetClient
State       : Disabled

FeatureName : TFTP
State       : Disabled

FeatureName : TIFFIFilter
State       : Disabled

FeatureName : VirtualMachinePlatform
State       : Disabled

FeatureName : Windows-Identity-Foundation
State       : Disabled

FeatureName : Client-ProjFS
State       : Disabled

FeatureName : SimpleTCP
State       : Disabled

FeatureName : WorkFolders-Client
State       : Enabled

FeatureName : NetFx3
State       : DisabledWithPayloadRemoved

FeatureName : WCF-HTTP-Activation
State       : Disabled

FeatureName : WCF-NonHTTP-Activation
State       : Disabled

FeatureName : IIS-WebServerRole
State       : Disabled

FeatureName : IIS-WebServer
State       : Disabled

FeatureName : IIS-CommonHttpFeatures
State       : Disabled

FeatureName : IIS-HttpErrors
State       : Disabled

FeatureName : IIS-HttpRedirect
State       : Disabled

FeatureName : IIS-ApplicationDevelopment
State       : Disabled

FeatureName : IIS-Security
State       : Disabled

FeatureName : IIS-RequestFiltering
State       : Disabled

FeatureName : IIS-NetFxExtensibility
State       : Disabled

FeatureName : IIS-NetFxExtensibility45
State       : Disabled

FeatureName : IIS-HealthAndDiagnostics
State       : Disabled

FeatureName : IIS-HttpLogging
State       : Disabled

FeatureName : IIS-LoggingLibraries
State       : Disabled

FeatureName : IIS-RequestMonitor
State       : Disabled

FeatureName : IIS-HttpTracing
State       : Disabled

FeatureName : IIS-URLAuthorization
State       : Disabled

FeatureName : IIS-IPSecurity
State       : Disabled

FeatureName : IIS-Performance
State       : Disabled

FeatureName : IIS-HttpCompressionDynamic
State       : Disabled

FeatureName : IIS-WebServerManagementTools
State       : Disabled

FeatureName : IIS-ManagementScriptingTools
State       : Disabled

FeatureName : IIS-IIS6ManagementCompatibility
State       : Disabled

FeatureName : IIS-Metabase
State       : Disabled

FeatureName : WAS-WindowsActivationService
State       : Disabled

FeatureName : WAS-ProcessModel
State       : Disabled

FeatureName : WAS-NetFxEnvironment
State       : Disabled

FeatureName : WAS-ConfigurationAPI
State       : Disabled

FeatureName : IIS-HostableWebCore
State       : Disabled

FeatureName : WCF-Services45
State       : Enabled

FeatureName : WCF-HTTP-Activation45
State       : Disabled

FeatureName : WCF-TCP-Activation45
State       : Disabled

FeatureName : WCF-Pipe-Activation45
State       : Disabled

FeatureName : WCF-MSMQ-Activation45
State       : Disabled

FeatureName : WCF-TCP-PortSharing45
State       : Enabled

FeatureName : IIS-StaticContent
State       : Disabled

FeatureName : IIS-DefaultDocument
State       : Disabled

FeatureName : IIS-DirectoryBrowsing
State       : Disabled

FeatureName : IIS-WebDAV
State       : Disabled

FeatureName : IIS-WebSockets
State       : Disabled

FeatureName : IIS-ApplicationInit
State       : Disabled

FeatureName : IIS-ISAPIFilter
State       : Disabled

FeatureName : IIS-ISAPIExtensions
State       : Disabled

FeatureName : IIS-ASPNET
State       : Disabled

FeatureName : IIS-ASPNET45
State       : Disabled

FeatureName : IIS-ASP
State       : Disabled

FeatureName : IIS-CGI
State       : Disabled

FeatureName : IIS-ServerSideIncludes
State       : Disabled

FeatureName : IIS-CustomLogging
State       : Disabled

FeatureName : IIS-BasicAuthentication
State       : Disabled

FeatureName : IIS-HttpCompressionStatic
State       : Disabled

FeatureName : IIS-ManagementConsole
State       : Disabled

FeatureName : IIS-ManagementService
State       : Disabled

FeatureName : IIS-WMICompatibility
State       : Disabled

FeatureName : IIS-LegacyScripts
State       : Disabled

FeatureName : IIS-FTPServer
State       : Disabled

FeatureName : IIS-FTPSvc
State       : Disabled

FeatureName : IIS-FTPExtensibility
State       : Disabled

FeatureName : MSMQ-Container
State       : Disabled

FeatureName : MSMQ-DCOMProxy
State       : Disabled

FeatureName : MSMQ-Server
State       : Disabled

FeatureName : MSMQ-HTTP
State       : Disabled

FeatureName : MSMQ-Multicast
State       : Disabled

FeatureName : MSMQ-Triggers
State       : Disabled

FeatureName : SMB1Protocol-Deprecation
State       : Disabled

FeatureName : MediaPlayback
State       : Disabled

FeatureName : MSRDC-Infrastructure
State       : Enabled

FeatureName : Printing-PrintToPDFServices-Features
State       : Enabled

FeatureName : MicrosoftWindowsPowerShellV2Root
State       : Enabled

FeatureName : MicrosoftWindowsPowerShellV2
State       : Enabled

FeatureName : SearchEngine-Client-Package
State       : Enabled

FeatureName : Microsoft-RemoteDesktopConnection
State       : Disabled

FeatureName : LegacyComponents
State       : Disabled

FeatureName : DirectPlay
State       : Disabled

FeatureName : Printing-Foundation-Features
State       : Enabled

FeatureName : Printing-Foundation-InternetPrinting-Client
State       : Enabled

FeatureName : Printing-Foundation-LPDPrintService
State       : Disabled

FeatureName : Printing-Foundation-LPRPortMonitor
State       : Disabled

FeatureName : NetFx4-AdvSrvs
State       : Enabled

FeatureName : NetFx4Extended-ASPNET45
State       : Disabled

FeatureName : SMB1Protocol
State       : Disabled

FeatureName : SMB1Protocol-Client
State       : Disabled

FeatureName : SMB1Protocol-Server
State       : Disabled

FeatureName : Recall
State       : DisabledWithPayloadRemoved

FeatureName : HypervisorPlatform
State       : Enabled

FeatureName : Microsoft-Windows-Subsystem-Linux
State       : Disabled
```


```
PS C:\WINDOWS\system32> Get-WindowsOptionalFeature -Online | Where-Object {$_.State -eq "Enabled"}


FeatureName : WorkFolders-Client
State       : Enabled

FeatureName : WCF-Services45
State       : Enabled

FeatureName : WCF-TCP-PortSharing45
State       : Enabled

FeatureName : MSRDC-Infrastructure
State       : Enabled

FeatureName : Printing-PrintToPDFServices-Features
State       : Enabled

FeatureName : MicrosoftWindowsPowerShellV2Root
State       : Enabled

FeatureName : MicrosoftWindowsPowerShellV2
State       : Enabled

FeatureName : SearchEngine-Client-Package
State       : Enabled

FeatureName : Printing-Foundation-Features
State       : Enabled

FeatureName : Printing-Foundation-InternetPrinting-Client
State       : Enabled

FeatureName : NetFx4-AdvSrvs
State       : Enabled

FeatureName : HypervisorPlatform
State       : Enabled

```

```
PS C:\WINDOWS\system32> Get-WindowsOptionalFeature -Online -FeatureName "DirectPlay"


FeatureName      : DirectPlay
DisplayName      : DirectPlay
Description      : Enables the installation of DirectPlay component.
RestartRequired  : Possible
State            : Disabled
CustomProperties :
                   ServerComponent\Description : Direct Play provides support for applications that use the DirectPlay networked gaming API
                   ServerComponent\DisplayName : Direct Play
                   ServerComponent\Id : 488
                   ServerComponent\Type : Feature
                   ServerComponent\UniqueName : Direct-Play
                   ServerComponent\Deploys\Update\Name : DirectPlay

```

