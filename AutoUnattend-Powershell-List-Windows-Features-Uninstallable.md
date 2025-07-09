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
| WorkFolders-Client | Enabled | Disabled | Disabled | Handles the synchronization process, ensuring that changes made on one device are reflected on others. This feature is particularly useful for organizations using a bring-your-own-device (BYOD) model, enabling users to access work files on personal devices while maintaining IT control. | [Microsoft.com](https://learn.microsoft.com/en-us/windows-server/storage/work-folders/work-folders-overview) |
| NetFx3 | Disabled* | Disabled* | Disabled* | Disabled* = DisabledWithPayloadRemoved NetFx3 aka .NET Framework 3.5, development platform created by Microsoft that provides a programming model, a runtime environment, and extensive class libraries for building and running various types of applications, particularly on Windows, used by applications especially older ones, were developed using .NET Framework 3.5 or earlier. | [Microsoft.com](https://learn.microsoft.com/en-us/dotnet/framework/install/versions-and-dependencies#net-framework-35) |
| WCF-HTTP-Activation | Disabled | Disabled | Disabled | The Windows Process Activation Service (WAS) allows common hosting within IIS regardless of the communications protocol being used. You can host your http WCF services on IIS and they will be dynamically activated once traffic starts arriving. | [Microsoft.com](https://learn.microsoft.com/en-us/archive/msdn-magazine/2007/september/iis-7-0-extend-your-wcf-services-beyond-http-with-was) |
| WCF-NonHTTP-Activation | Disabled | Disabled | Disabled | As above but where WCF Non-HTTP Activation refers to the ability of Windows Communication Foundation (WCF) services to be activated and hosted using protocols other than HTTP, such as TCP or Named Pipes. | |
| IIS-WebServerRole | Disabled | Disabled | Disabled | IIS-WebServerRole is the top level, the next level groups the three major feature areas within the IIS-WebServerRole. These are: IIS-WebServer, IIS-WebServerManagementTools, IIS-FTPPublishingService. Each of these groups contains one or more  installable features. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#package-updates) |
| IIS-WebServer | Disabled | Disabled | Disabled | IIS (Internet Information Services) is a flexible, general-purpose web server for Windows systems, offering features for hosting websites, web applications, and services. The IIS 7 and later web servers have a completely modular architecture which offers three key benefits: Componentization, Extensibility, ASP.NET Integration. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/get-started/introduction-to-iis/iis-web-server-overview) |
| IIS-CommonHttpFeatures | Disabled | Disabled | Disabled | Installs support for static Web server content such as HTML & image files, custom errors, and redirection. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#iis-70-and-above-components-overview) |
| IIS-HttpErrors | Disabled | Disabled | Disabled | Installs HTTP Error files. Allows you to customize the error messages returned to clients. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#iis-70-and-above-components-overview) |
| IIS-HttpRedirect | Disabled | Disabled | Disabled | Provides support to redirect client requests to a specific destination. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#iis-70-and-above-components-overview) |
| IIS-ApplicationDevelopment | Disabled | Disabled | Disabled | Installs support for application development such as ASP.NET, Classic ASP, CGI, and ISAPI extensions. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#iis-70-and-above-components-overview) |
| IIS-Security | Disabled | Disabled | Disabled | Enables additional security protocols to secure servers, sites, applications, vdirs, and files. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#iis-70-and-above-components-overview) |
| IIS-RequestFiltering | Disabled | Disabled | Disabled | Configures rules to block selected client requests. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#iis-70-and-above-components-overview) |
| IIS-NetFxExtensibility | Disabled | Disabled | Disabled | IIS7.0 ASP.NET Workload | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/install-typical-iis-workloads#aspnet-workload)|
| IIS-NetFxExtensibility45 | Disabled | Disabled | Disabled | IIS 8.5 on Windows Server 2012 R2 ASP.NET Workload| [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-85/installing-iis-85-on-windows-server-2012-r2) |
| IIS-HealthAndDiagnostics | Disabled | Disabled | Disabled | Provides support for logging, runtime status, and request tracing. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#health-and-diagnostics-updates) |
| IIS-HttpLogging | Disabled | Disabled | Disabled | The ```<httpLogging>``` element allows you to configure IIS to generate log entries for only successful requests, failed requests, or both. After you configure logging for each Web site at the server level, you can use this element to enable selective logging for individual URLs. By default, HTTP logging is enabled for all requests on Internet Information Services (IIS) 7. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#health-and-diagnostics-updates) |
| IIS-LoggingLibraries | Disabled | Disabled | Disabled | IIS logging libraries, specifically the Advanced Logging feature, allow for highly customizable logging of HTTP requests and client data on IIS servers. These libraries provide flexibility in specifying which fields to log, adding custom fields, and managing log file rollover and filtering. Essentially, they empower administrators to tailor logging to their specific needs, offering more control than the default logging options. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#health-and-diagnostics-updates) |
| IIS-RequestMonitor | Disabled | Disabled | Disabled | RSCA – an acronym for Runtime Service and Control API. Underneath the ‘Health and Diagnostics’ feature, you will need to make sure that the ‘Request Monitor’ checkbox is checked for RSCA to be installed.  | [Microsoft.com](https://techcommunity.microsoft.com/blog/iis-support-blog/using-rsca-to-help-you-understand-what-your-iis-server-requests-are-doing/773904) |
| IIS-HttpTracing | Disabled | Disabled | Disabled | Allows for detailed request-based tracing of HTTP requests, providing insights into the request processing pipeline. This feature is invaluable for diagnosing issues related to authentication, request handling, and performance bottlenecks within IIS | [microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#health-and-diagnostics-updates) |
| IIS-URLAuthorization | Disabled | Disabled | Disabled | Part of the Security update groups together all of the authentication, authorization, and filtering features. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#security-updates) |
| IIS-IPSecurity | Disabled | Disabled | Disabled | The IIS-IPSecurity feature, also known as IP Address and Domain Restrictions, allows administrators to control access to websites and applications based on IP addresses or domain names. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#security-updates) |
| IIS-Performance | Disabled | Disabled | Disabled | The Performance update grouping includes the two compression updates for static and dynamic content. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#performance) |
| IIS-HttpCompressionDynamic | Disabled | Disabled | Disabled | Part of the Performance update grouping which includes the two compression updates for static and this dynamic content. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#performance) |
| IIS-WebServerManagementTools | Disabled | Disabled | Disabled | The IIS-Web Server Management Tools feature, specifically the IIS Manager, provides a user interface and programmatic access for managing IIS (Internet Information Services) servers locally and remotely. It allows administrators to configure, monitor, and troubleshoot web servers, sites, and applications. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/overview/powerful-admin-tools) |
| IIS-ManagementScriptingTools | Disabled | Disabled | Disabled | The "IIS Management Scripting Tools" feature provides command-line utilities and scripts for automating various IIS configuration and management tasks. It allows administrators to manage IIS settings, worker processes, and application domains programmatically, complementing the graphical IIS Manager interface. | [microsoft.com](https://learn.microsoft.com/en-us/iis/overview/powerful-admin-tools#windows-powershell) |
| IIS-IIS6ManagementCompatibility | Disabled | Disabled | Disabled | The IIS 6 Management Compatibility feature in IIS (Internet Information Services) provides legacy management tools and APIs for managing newer IIS versions. It allows administrators to use familiar IIS 6.0 tools and scripts to configure and manage sites, application pools, and other aspects of an IIS 7.0 or later web server. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-iis-7/understanding-setup-in-iis#management-tools) |
| IIS-Metabase | Disabled | Disabled | Disabled | The IIS Metabase is a hierarchical database used in Internet Information Services (IIS) to store configuration settings for websites, virtual directories, applications, and other IIS components. It acts as the central repository for all configuration data in IIS versions prior to IIS 7. In later versions, the metabase is still used for backward compatibility and stores custom settings not present in the newer configuration system. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/manage/managing-your-configuration-settings/metabase-compatibility-with-iis-7-and-above) |
| WAS-WindowsActivationService | Disabled | Disabled | Disabled | The Windows Process Activation Service (WAS) is a crucial component in IIS (Internet Information Services) that manages the process model for hosting web applications and services. It handles the activation and lifetime of worker processes, providing features like process recycling, rapid failure protection, and health monitoring. WAS allows applications to be hosted more robustly and efficiently, extending beyond the limitations of the traditional IIS worker process. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) |
| WAS-ProcessModel | Disabled | Disabled | Disabled | The Windows Process Activation Service (WAS) process model, introduced with IIS 7.0, provides a robust and efficient way to manage worker processes for hosting applications, particularly WCF services. It extends the IIS 6.0 process model by removing the dependency on HTTP for activation and introducing features like message-based activation, recycling, and health monitoring. | [Microsoft.com](https://learn.microsoft.com/en-us/dotnet/framework/wcf/feature-details/hosting-in-windows-process-activation-service) |
| WAS-NetFxEnvironment | Disabled | Disabled | Disabled | The WAS-NetFxEnvironment feature in Windows refers to the Windows Process Activation Service (.NET Framework Environment) feature, which is a crucial component for enabling ASP.NET applications within Internet Information Services (IIS). It allows IIS to activate and manage worker processes for .NET applications by integrating with the .NET Framework. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/application-frameworks/scenario-build-an-aspnet-website-on-iis/configuring-step-2-configure-asp-net-settings) |
| WAS-ConfigurationAPI | Disabled | Disabled | Disabled | The "WAS-ConfigurationAPI" feature in Windows refers to the Windows Process Activation Service Configuration API. It's a feature of the Windows Process Activation Service (WAS), which is a core component of IIS (Internet Information Services) responsible for managing application pools and other configuration aspects of web applications. This API allows for programmatic configuration and management of IIS settings, enabling automation and remote administration of web servers. | [Microsoft.com](https://learn.microsoft.com/en-us/dotnet/framework/wcf/feature-details/was-activation-architecture) |
| IIS-HostableWebCore | Disabled | Disabled | Disabled | The IIS-Hostable Web Core feature, also known as Hosted Web Core (HWC), allows developers to embed the core functionality of IIS (Internet Information Services) within their own applications. This enables developers to create custom web servers or host web applications without relying on a separate IIS installation, offering greater control over the environment | [Microsoft.com](https://learn.microsoft.com/en-us/iis/web-development-reference/native-code-development-overview/creating-hosted-web-core-applications) |
| WCF-Services45 | Enabled | Disabled | Disabled | WCF-Services45 refers to the Windows Communication Foundation (WCF) features introduced in .NET Framework 4.5. It includes a variety of enhancements, such as simplified configuration, task-based async support, WebSocket support, and improvements to security and streaming. | [Microsoft.com](https://learn.microsoft.com/en-us/dotnet/framework/wcf/whats-new) |
| WCF-HTTP-Activation45 | Disabled | Disabled | Disabled | The WCF-HTTP-Activation45 feature, also known as HTTP Activation for .NET 4.5 and above, is a Windows feature that enables the hosting of Windows Communication Foundation (WCF) services using the HTTP protocol within Internet Information Services (IIS). It allows IIS to dynamically activate WCF services when HTTP requests are received, without requiring a dedicated listener process. | [Microsoft.com](https://learn.microsoft.com/en-us/exchange/plan-and-deploy/deployment-ref/ms-exch-setupreadiness-netwcfhttpactivation45notinstalled) |
| WCF-TCP-Activation45 | Disabled | Disabled | Disabled | The WCF-TCP-Activation45 feature in .NET Framework enables Windows Communication Foundation (WCF) services to be activated over the TCP protocol using the Windows Process Activation Service (WAS). This feature allows services to be hosted without requiring a dedicated HTTP listener and provides a more efficient way to communicate over TCP. | [Microsoft.com](https://learn.microsoft.com/en-us/dotnet/framework/wcf/samples/tcp-activation) |
| WCF-Pipe-Activation45 | Disabled | Disabled | Disabled | The WCF-Pipe-Activation45 feature, or NET-WCF-Pipe-Activation45, is a Windows feature that enables named pipe activation for WCF (Windows Communication Foundation) services. It's a part of the .NET Framework 4.5 and is required by some applications, like Microsoft Exchange Server, for proper functionality. | [Microsoft.com](https://learn.microsoft.com/en-us/dotnet/framework/wcf/samples/namedpipe-activation) |
| WCF-MSMQ-Activation45 | Disabled | Disabled | Disabled | The WCF-MSMQ-Activation45 feature in Windows refers to the Message Queuing (MSMQ) activation components within Windows Communication Foundation (WCF). It enables WCF services to be activated and hosted using MSMQ as a transport mechanism for message-based communication. This feature is crucial for building loosely coupled, reliable, and scalable applications using WCF. | [Microsoft.com](https://learn.microsoft.com/en-us/dotnet/framework/wcf/samples/msmq-activation) |
| WCF-TCP-PortSharing45 | Enabled | Disabled | Disabled | The WCF-TCP-PortSharing45 feature in .NET allows multiple Windows Communication Foundation (WCF) services to share the same TCP port, enabling efficient resource utilization. This feature is managed by the Net.TCP Port Sharing Service, which acts as a intermediary, forwarding messages to the appropriate service based on its destination. | [Microsoft.com](https://learn.microsoft.com/en-us/dotnet/framework/wcf/feature-details/net-tcp-port-sharing) |
| IIS-StaticContent | Disabled | Disabled | Disabled | The IIS-StaticContent feature in Internet Information Services (IIS) is responsible for serving static files like HTML, CSS, JavaScript, and images directly to web browsers without any processing by IIS. It's crucial for efficient website performance, as it allows IIS to handle these files quickly and easily, without involving more complex dynamic content processing | [Microsoft.com](https://learn.microsoft.com/en-us/answers/questions/1468747/404-error-on-scripts-in-iis) | 
| IIS-DefaultDocument | Disabled | Disabled | Disabled | The IIS (Internet Information Services) Default Document feature allows a web server to automatically serve a specific file when a user requests a website without specifying a file name (e.g., just requesting https://www.example.com instead of https://www.example.com/index.html). IIS checks a configured list of default documents and serves the first one found in the requested directory. | [Microsoft.com](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831665(v=ws.11)) |
| IIS-DirectoryBrowsing | Disabled | Disabled | Disabled | In IIS (Internet Information Services), Directory Browsing is a feature that allows users to view the contents of a directory on a web server through a web browser if a default file (like index.html) is not found within that directory. It essentially provides a directory listing of files and folders within a specified location on the server. | [Microsoft.com](https://learn.microsoft.com/en-us/troubleshoot/developer/webapps/iis/site-behavior-performance/http-403-14-forbidden-webpage) |
| IIS-WebDAV | Disabled | Disabled | Disabled | WebDAV (Web Distributed Authoring and Versioning) is a set of extensions to the HTTP protocol that enable collaborative authoring and management of files on web servers. In the context of Internet Information Services (IIS), WebDAV provides a way to extend web server functionality, allowing users to interact with files and resources on the server as if they were local files. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-webdav-on-iis) |
| IIS-WebSockets | Disabled | Disabled | Disabled | IIS-WebSockets is a feature in Internet Information Services (IIS) that enables bidirectional, real-time communication between a web server and a client over a single TCP connection. It leverages the WebSocket protocol, allowing for more efficient and persistent connections compared to traditional HTTP request-response models. This feature is particularly useful for applications requiring low-latency data transfer, such as chat applications, online games, and real-time dashboards. | [Microsoft.com](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/websockets?view=aspnetcore-9.0) |
| IIS-ApplicationInit | Disabled | Disabled | Disabled | The IIS Application Initialization feature allows you to preload your web application when the application pool starts, or when it is restarted, before the first user request arrives. This helps to avoid the "cold start" delay that users often experience when an application is accessed for the first time after a period of inactivity or after an application pool recycle. By preloading the application, you can reduce the time it takes for the application to respond to the first request, leading to improved performance and user experience. | [Microsoft.com](https://learn.microsoft.com/en-us/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) |
| IIS-ISAPIFiltern | Disabled | Disabled | Disabled | ISAPI filters in IIS (Internet Information Services) are DLLs that extend the functionality of the web server by intercepting and modifying HTTP requests. They can be used for various purposes, including authentication, content transformation, logging, and request screening. | [Microsoft.com](https://learn.microsoft.com/en-us/previous-versions/iis/6.0-sdk/ms524610(v=vs.90)) |
| IIS-ISAPIExtensions | Disabled | Disabled | Disabled | ISAPI (Internet Server Application Programming Interface) Extensions are a feature of Internet Information Services (IIS) that allow developers to extend the functionality of a web server by implementing custom code as DLLs (Dynamic Link Libraries). These extensions run within the IIS process, providing a more efficient way to handle requests compared to traditional CGI (Common Gateway Interface) methods. | [Microsoft.com](https://learn.microsoft.com/en-us/previous-versions/iis/6.0-sdk/ms525172(v=vs.90)) |
| IIS-ASPNET | Disabled | Disabled | Disabled | IIS and ASP.NET are closely integrated, with IIS acting as the web server and ASP.NET providing the framework for building web applications. ASP.NET modules can plug directly into the IIS pipeline, allowing developers to extend IIS with ASP.NET features like forms authentication, membership, and session state. This integration offers a unified experience for developers using the .NET framework and enables them to build powerful server features using the familiar ASP.NET APIs. | [Microsoft](https://learn.microsoft.com/en-us/iis/application-frameworks/scenario-build-an-aspnet-website-on-iis/overview-build-an-asp-net-website-on-iis) |
| IIS-ASPNET45 | Disabled | Disabled | Disabled | The "IIS-ASPNET45" feature refers to the ability to enable ASP.NET 4.5 within the Internet Information Services (IIS) web server on Windows systems. This allows you to host and run ASP.NET 4.5 web applications using IIS | [Microsoft.com](https://learn.microsoft.com/en-us/troubleshoot/developer/webapps/aspnet/configuration/install-aspnet-45-windows-8-server-2012) |
| IIS-ASP | Disabled | Disabled | Disabled | See Above. | |
| IIS-CGI | Disabled | Disabled | Disabled | See Above. | |
| IIS-ServerSideIncludes | Disabled | Disabled | Disabled | See Above. | |
| IIS-CustomLogging | Disabled | Disabled | Disabled | See Above. | |
| IIS-BasicAuthentication | Disabled | Disabled | Disabled | See Above. | |
| IIS-HttpCompressionStatic | Disabled | Disabled | Disabled | See Above. | |
| IIS-ManagementConsole | Disabled | Disabled | Disabled | See Above. | |
| IIS-ManagementService | Disabled | Disabled | Disabled | See Above. | |
| IIS-WMICompatibility | Disabled | Disabled | Disabled | See Above. | |
| IIS-LegacyScripts | Disabled | Disabled | Disabled | See Above. | |
| IIS-FTPServer | Disabled | Disabled | Disabled | See Above. | |
| IIS-FTPSvc | Disabled | Disabled | Disabled | See Above. | |
| IIS-FTPExtensibility | Disabled | Disabled | Disabled | See Above. | |
| MSMQ-Container | Disabled | Disabled | Disabled | See Above. | |
| MSMQ-DCOMProxy | Disabled | Disabled | Disabled | See Above. | |
| MSMQ-Server | Disabled | Disabled | Disabled | See Above. | |
| MSMQ-HTTP | Disabled | Disabled | Disabled | See Above. | |
| MSMQ-Multicast | Disabled | Disabled | Disabled | See Above. | |
| MSMQ-Triggers | Disabled | Disabled | Disabled | See Above. | |
| SMB1Protocol-Deprecation | Disabled | Disabled | See Above. | |
| MediaPlayback | Disabled | Disabled | Disabled | See Above. | |
| MSRDC-Infrastructure | Enabled | Disabled | Disabled | See Above. | |
| Printing-PrintToPDFServices-Features | Enabled | Disabled | Disabled | See Above. | |
| MicrosoftWindowsPowerShellV2Root | Enabled | Disabled | Disabled | See Above. | |
| MicrosoftWindowsPowerShellV2 | Enabled | Disabled | Disabled | See Above. | |
| SearchEngine-Client-Package | Enabled | Disabled | Disabled | See Above. | |
| Microsoft-RemoteDesktopConnection | Disabled | Disabled | Disabled | See Above. | |
| LegacyComponents | Disabled | Disabled | Disabled | See Above. | |
| DirectPlay | Disabled | Disabled | Disabled | See Above. | |
| Printing-Foundation-Features | Enabled | Disabled | Disabled | See Above. | |
| Printing-Foundation-InternetPrinting-Client | Enabled | Disabled | Disabled | See Above. | |
| Printing-Foundation-LPDPrintService | Disabled | Disabled | Disabled | See Above. | |
| Printing-Foundation-LPRPortMonitor | Disabled | Disabled | Disabled | See Above. | |
| NetFx4-AdvSrvs | Enabled | Disabled | Disabled | See Above. | |
| NetFx4Extended-ASPNET45 | Disabled | Disabled | Disabled | See Above. | |
| SMB1Protocol | Disabled | Disabled | Disabled | See Above. | |
| SMB1Protocol-Client | Disabled | Disabled | Disabled | See Above. | |
| SMB1Protocol-Server | Disabled | Disabled | Disabled | See Above. | |
| Recall | Disabled* | Disabled* | Disabled* | Disabled* = DisabledWithPayloadRemoved See Above. | |
| HypervisorPlatform | Enabled | Disabled | Disabled | See Above. | |
| Microsoft-Windows-Subsystem-Linux | Disabled | Disabled | Disabled | See Above. | |


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

