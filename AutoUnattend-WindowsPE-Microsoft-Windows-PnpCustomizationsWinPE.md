# windowsPE Component

# Microsoft-Windows-PnpCustomizationsWinPE

This is the first opportunity in the autounattend.xml process where you can create the $WinpeDriver$ folder and subfolders containing drivers on the memory stick for installation during the windowsPE pass and other passes in the autounattend.xml .

The windowsPE component pass is where the drivers are added to the windows driver store during the installation process and for the windowsPE installation process to gain additional boot-critical driver functionality.

https://learn.microsoft.com/en-us/windows-hardware/drivers/install/driver-store

To install drivers during an offline installation, first you need to create a folder called $WinpeDriver$ on the USB stick used by the Windows Media Creation program to copy the windows installation files. Next copy the extracted drivers into their own subfolders inside/below the $WinpeDriver$ eg.

USB Memory Stick\$WinpeDriver$\audio\
USB Memory Stick\$WinpeDriver$\graphics\
USB Memory Stick\$WinpeDriver$\motherboard\
USB Memory Stick\$WinpeDriver$\wlan\

In order to do this, you will need to logon on to your computer manufacturers website and download the current drivers specifically for your computer.

Some driver installation programs offer the option to install or extract the drivers when running the driver installation program. Others have a command line switch which can extract the drivers, that needs to be run from the DOS command window or powershell window. Other manufacturers will provide one or a few CAB (Window's Cabinet) files or in the case of Dell, a DUP (Dell Update Package) file where the required driver files can be extracted, typically using something 7zip if they are not already self extracting.

Its important to have all the latest drivers your computer needs in order for the installation process to work smoothly as missing drivers or older drivers can make it impossible to get online without the use of a second device to download missing drivers for copying across.

When its possible to get online despite having missing drivers, windows will generally offer the option to install missing drivers to resolve the situation when using the windows update process. Likewise the manufacturer may also have a program which can detect and install any missing drivers, but these remedies rely on being able to get online.

There are limitations for "injecting" drivers which are explained in this link. https://learn.microsoft.com/en-us/troubleshoot/windows-client/setup-upgrade-and-drivers/limitations-dollar-sign-winpedriver-dollar-sign
<Path>

%configsetroot% is a placeholder variable that represents the root drive/folder of the USB Memory stick. In order to use this variable you need to add

<UseConfigurationSet>true</UseConfigurationSet>

to the

<component name="Microsoft-Windows-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
</component>

section.

eg

<component name="Microsoft-Windows-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
	<UseConfigurationSet>true</UseConfigurationSet>
</component>

The next stage is to list the driver folders that are copied onto the USB memory stick using the Microsoft-Windows-PnpCustomizationsWinPE section in the autounattend.xml file.

eg.

<settings pass="windowsPE">
	<component name="Microsoft-Windows-PnpCustomizationsWinPE" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
		<DriverPaths>
			<PathAndCredentials wcm:keyValue="1" wcm:action="add">
				<Path>%configsetroot%\AudioDriverFolder</Path>
				<Path>%configsetroot%\GraphicsDriverFolder</Path>
				<Path>%configsetroot%\MotherboardDriverFolder</Path>
				<Path>%configsetroot%\WlanDriverFolder</Path>
				<Credentials></Credentials>
			</PathAndCredentials>
		</DriverPaths>
	</component>
</settings>

Other variations are shown below, but you increase the risk of failure when drive letters are pointing to the wrong drive or the network share is not accessible because network drivers are missing or the network share login credentials are missing from the AutoUnattend.xml file preventing windowsPE from connecting to password protected network shares.

More information on Network shares, their limitations and how to enable them for use in the autoUnattend.xml file can be found in this link. https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-network-drivers-initializing-and-adding-drivers

<Path>C:\Drivers</Path>
<Path>\\MyUNC\Path\to\Drivers</Path>

Workarounds for the wrong drive letter include listing all the driver letters from A to Z in the <Path> section as seen below, but this approach also requires the drivers folder name not being used elsewhere on the computer or USB memory stick.

<Path>A:\Drivers</Path>
<Path>B:\Drivers</Path>
<Path>C:\Drivers</Path>
<Path>D:\Drivers</Path>
<Path>E:\Drivers</Path>
<Path>F:\Drivers</Path>
<Path>G:\Drivers</Path>
<Path>H:\Drivers</Path>
<Path>I:\Drivers</Path>

Its worth pointing out that a UNC path whilst not being a web address like the type used in web browsers, can still refer to a server that is online in another location around the world. This situation requires the network the computer connects to, being setup in such a way as to access the UNC network share, either directly over the internet, using a VPN running over the internet or a private WAN.

A script based solution to finding the correct drive is shown in this link. https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-identify-drive-letters

Drives which are not accessed via network shares but but the Storage Area Policy (SAN) is shown in this link. https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-storage-area-network--san--policy
<Credentials>

For network shares or servers that are password protected.

<Credentials>
	<Domain>MyDomain.local</Domain>
    <Username>MyAdministratorUsername</Username>
    <Password>MyAdministratorPassword</Password>
</Credentials>

<Credentials>
	<Domain>MyDomain.com</Domain>
    <Username>MyOnlineUsername</Username>
    <Password>MyOnlinePassword</Password>
</Credentials>

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-pnpcustomizationswinpe

<settings pass="windowsPE">
	<component name="Microsoft-Windows-PnpCustomizationsWinPE" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
		<DriverPaths>
			<PathAndCredentials wcm:keyValue="1" wcm:action="add">
				<Path>%configsetroot%\audio</Path>
				<Path>%configsetroot%\graphics</Path>
				<Path>%configsetroot%\motherboard</Path>
				<Path>%configsetroot%\wlan</Path>
				<Credentials></Credentials>
			</PathAndCredentials>
		</DriverPaths>
	</component>
</settings>

\\myFirstDriverPath\DriversFolder MyDomain.local MyAdministratorUsername MyAdministratorPassword C:\Drivers MyComputerName MyUsername MyPassword

https://www.tenforums.com/installation-upgrade/178735-answer-file-autounattend-xml-diskid-changes-after-loading-drivers.html