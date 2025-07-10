# Windows Services Overview

Categorized summary of Typical Windows system services including their internal name, display name, and brief functional description.

---

## 🌐 Networking & Connectivity

| Name | DisplayName | Description |
|------|-------------|-------------|
| Dhcp | DHCP Client | Automatically assigns IP addresses and other network configuration parameters via DHCP. |
| Dnscache | DNS Client | Resolves domain names to IP addresses and caches responses to improve performance. |
| iphlpsvc | IP Helper | Provides IPv6 connectivity over an IPv4 network and support for network configuration. |
| Netlogon | Netlogon | Supports secure user and machine authentication for domain-based systems. |
| Netman | Network Connections | Manages network adapters and connections, enabling configuration and monitoring. |
| netprofm | Network List Service | Identifies connected networks and assigns names, locations, and connectivity status. |
| NlaSvc | Network Location Awareness | Detects network changes and identifies connection types for firewall and policy settings. |
| NcbService | Network Connection Broker | Coordinates connection logic for modern networking scenarios in UWP apps. |
| Wcmsvc | Windows Connection Manager | Manages Wi-Fi and mobile broadband connections. |
| WlanSvc | WLAN AutoConfig | Controls wireless network connectivity and authentication. |
| WinHttpAutoProxySvc | WinHTTP Web Proxy Auto-Discovery Service | Detects proxy server settings using the WPAD protocol. |
| wcncsvc | Windows Connect Now - Config Registrar | Supports easy configuration of wireless routers and access points. |
| WFDSConMgrSvc | Wi-Fi Direct Services Connection Manager Service | Manages discovery and connection of Wi-Fi Direct services. |
| RemoteAccess | Routing and Remote Access | Provides routing services for LAN and WAN connections, including VPN support. |
| RasMan | Remote Access Connection Manager | Manages dial-up and VPN connections to remote networks. |
| RasAuto | Remote Access Auto Connection Manager | Initiates a connection to a remote network whenever a program references a remote address. |
| NetSetupSvc | Network Setup Service | Assists in network-related setup and initialization operations. |
| NetTcpPortSharing | Net.Tcp Port Sharing Service | Allows multiple applications to share ports over the Net.Tcp protocol. |


---

## 🔊 Audio & Multimedia

| Name | DisplayName | Description |
|------|-------------|-------------|
| Audiosrv | Windows Audio | Controls audio playback and recording for Windows-based apps. |
| AudioEndpointBuilder | Windows Audio Endpoint Builder | Manages audio endpoints and ensures proper sound routing. |
| ApxSvc | Windows Virtual Audio Device Proxy Service | Acts as a proxy for virtual audio drivers to improve compatibility and function. |
| BTAGService | Bluetooth Audio Gateway Service | Streams Bluetooth audio through system drivers for connected devices. |
| BthAvctpSvc | AVCTP service | Supports Bluetooth audio transport control profile operations. |
| bthserv | Bluetooth Support Service | Facilitates Bluetooth device discovery and association. |
| DolbyDAXAPI | Dolby DAX API Service | Enables Dolby audio enhancements for immersive sound experience. |
| ElevocService | Elevoc Control Service | Enhances audio signal processing for devices supporting Elevoc tech. |
| hidserv | Human Interface Device Service | Enables input from HID devices like volume control buttons or headsets. |
| RtkAudioUniversalService | Realtek Audio Universal Service | Manages Realtek sound hardware and configuration settings. |
| GameInputSvc | GameInput Service | Provides low-latency input for game controllers and peripherals. |
| QWAVE | Quality Windows Audio Video Experience | Optimizes streaming by prioritizing audio/video data over networks. |
| SmsRouter | Microsoft Windows SMS Router Service | Routes text messages on devices that support telephony functions. |


---

## 🔒 Security & Authentication

| Name | DisplayName | Description |
|------|-------------|-------------|
| AppIDSvc | Application Identity | Determines application identity to enforce software restriction policies like AppLocker. |
| CryptSvc | Cryptographic Services | Manages encryption and certificate services used across Windows. |
| SamSs | Security Accounts Manager | Stores and manages security accounts like usernames and credentials. |
| SCardSvr | Smart Card | Manages smart card device connections and access. |
| SCPolicySvc | Smart Card Removal Policy | Enables locking of the system when a smart card is removed. |
| sppsvc | Software Protection | Protects system licensing and anti-piracy verification mechanisms. |
| wscsvc | Security Center | Monitors system security settings and reports status like antivirus, firewall, etc. |
| SecurityHealthService | Windows Security Service | Oversees threat protection, virus scanning, and security health checks. |
| NgcSvc | Microsoft Passport | Manages credentials for two-factor and biometric authentication. |
| NgcCtnrSvc | Microsoft Passport Container | Stores and isolates Passport credentials securely. |
| TokenBroker | Web Account Manager | Handles web account authentication tokens for connected services. |
| WinDefend | Microsoft Defender Antivirus Service | Detects and removes malware, and protects system in real-time. |
| WdNisSvc | Microsoft Defender Antivirus Network Inspection Service | Scans network traffic for malicious threats in coordination with Defender. |
| TPM Maintenance Service | TPM Maintenance Service | Handles Trusted Platform Module operations and maintenance (if present). |


---

## 🛠️ System Management

| Name | DisplayName | Description |
|------|-------------|-------------|
| AppReadiness | App Readiness | Prepares applications for first-time use during account sign-in or app installations. |
| AppXSvc | AppX Deployment Service (AppXSVC) | Deploys Store apps to users, handling installation and removal procedures. |
| BrokerInfrastructure | Background Tasks Infrastructure Service | Coordinates background tasks and ensures they run efficiently. |
| DcomLaunch | DCOM Server Process Launcher | Enables launching of COM+ components for distributed application support. |
| EventLog | Windows Event Log | Logs important system, security, and application events used for diagnostics. |
| EventSystem | COM+ Event System | Manages automatic event distribution between COM+ components. |
| FontCache | Windows Font Cache Service | Caches commonly used fonts to improve application performance. |
| msiserver | Windows Installer | Handles installation, modification, and removal of software via MSI packages. |
| ProfSvc | User Profile Service | Manages user profile loading and unloading during login/logout. |
| Schedule | Task Scheduler | Automates scheduled tasks across system and user-defined operations. |
| ShellHWDetection | Shell Hardware Detection | Detects and responds to hardware events like CD/DVD insertion. |
| SysMain | SysMain | Improves performance by optimizing system startup and app launch behavior. |
| TrkWks | Distributed Link Tracking Client | Maintains file shortcuts when files are moved between NTFS volumes. |
| TrustedInstaller | Windows Modules Installer | Installs, modifies, and removes Windows updates and optional system components. |
| Themes | Themes | Enables visual themes and styles across Windows desktop environments. |
| wbengine | Block Level Backup Engine Service | Executes block-level backup operations for system recovery. |
| Winmgmt | Windows Management Instrumentation | Provides management data and control operations for system administrators. |
| WinRM | Windows Remote Management (WS-Management) | Enables secure remote management using WS-Man protocol. |



---

## 🧠 AI & Diagnostics

| Name | DisplayName | Description |
|------|-------------|-------------|
| DiagTrack | Connected User Experiences and Telemetry | Collects diagnostic and usage data to improve system reliability, performance, and user experience. |
| DPS | Diagnostic Policy Service | Detects and troubleshoots problems with Windows components and hardware. |
| diagsvc | Diagnostic Execution Service | Executes troubleshooting processes initiated by diagnostic tools. |
| WdiServiceHost | Diagnostic Service Host | Provides diagnostic support for gathering and reporting system health data. |
| WdiSystemHost | Diagnostic System Host | Supports system-level data collection for diagnostics and assessments. |
| TroubleshootingSvc | Recommended Troubleshooting Service | Identifies and suggests automatic fixes for common issues based on system conditions. |
| perfhost | Performance Counter DLL Host | Allows remote access to Windows performance data; useful for monitoring tools. |
| pla | Performance Logs & Alerts | Collects performance data and triggers alerts based on thresholds or schedules. |
| SystemEventsBroker | System Events Broker | Handles delivery of system event notifications to background tasks. |


---

## 🧩 Device & Hardware Support

| Name | DisplayName | Description |
|------|-------------|-------------|
| PlugPlay | Plug and Play | Detects hardware changes and initiates drivers for newly connected devices. |
| DsmSvc | Device Setup Manager | Coordinates installation and setup of new hardware devices during OS run time. |
| DeviceInstall | Device Install Service | Facilitates installation of drivers and software for newly added hardware. |
| DevQueryBroker | DevQuery Background Discovery Broker | Searches for connected devices and makes them available to apps. |
| DmEnrollmentSvc | Device Management Enrollment Service | Enrolls devices into mobile device management systems like Microsoft Intune. |
| dmwappushservice | Device Management Wireless Application Protocol (WAP) Push Routing Service | Handles push messages from management servers to mobile-connected devices. |
| DisplayEnhancementService | Display Enhancement Service | Adjusts screen brightness, contrast, and HDR settings based on user and environmental factors. |
| FrameServer | Windows Camera Frame Server | Streams raw camera frames to applications and system processes. |
| FrameServerMonitor | Windows Camera Frame Server Monitor | Monitors the health and functionality of camera services. |
| UsbHub3 | USB Hub Service | Manages USB hubs and routing on modern Windows hardware (if applicable). |
| WPDBusEnum | Portable Device Enumerator Service | Detects portable storage and media devices like phones, cameras, and USB drives. |
| PrintScanBrokerService | PrintScanBrokerService | Handles scanning operations and device coordination with supported print/scanners. |
| CameraAccessControlApp | Camera Access Control | Manages permission and access requests for camera hardware (if applicable). |


---

## 🎮 Gaming & Experience Enhancers

| Name | DisplayName | Description |
|------|-------------|-------------|
| BcastDVRUserService_47e64 | GameDVR and Broadcast User Service_47e64 | Supports capturing screenshots, recording gameplay, and streaming via Xbox Game Bar. |
| CaptureService_47e64 | CaptureService_47e64 | Handles screen recording and game capture functionality. |
| GameInputSvc | GameInput Service | Delivers raw input data from game controllers for smoother gameplay experience. |
| MessagingService_47e64 | MessagingService_47e64 | Facilitates message synchronization and notifications for gaming and user sessions. |
| XboxGipSvc | Xbox Accessory Management Service | Manages Xbox accessories such as controllers and headsets. |
| XblAuthManager | Xbox Live Auth Manager | Authenticates user credentials for Xbox Live services. |
| XblGameSave | Xbox Live Game Save | Synchronizes and backs up game saves to the Xbox Live cloud. |
| XboxNetApiSvc | Xbox Live Networking Service | Provides real-time networking support for Xbox Live multiplayer games. |