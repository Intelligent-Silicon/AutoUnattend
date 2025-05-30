# windowsPE Component
# Microsoft-Windows-PnpCustomizationsWinPE

This is the first opportunity in the autounattend.xml process where you can create the ```$WinpeDriver$``` folder and subfolders containing drivers on the memory stick to install during the windowsPE pass and use during the installation process.

This is where the drivers are added to the windows driver store and the drivers available on the memory stick can also be used to for such things as installing RAID controller drivers to access the raid drive in order to install windows; raid drives are typically used in servers or high end desktops, but involve two or more drives which is beyond the scope of this repo.

https://learn.microsoft.com/en-us/windows-hardware/drivers/install/driver-store

To install drivers during an offline installation, first you need to create a folder called ```$WinpeDriver$``` on the USB stick used by the Windows Media Creation program to copy the windows installation files. Next copy the extracted drivers into their own subfolders inside the ```$WinpeDriver$``` eg.

```
USB Memory Stick\$WinpeDriver$\audio\
USB Memory Stick\$WinpeDriver$\graphics\
USB Memory Stick\$WinpeDriver$\wlan\
USB Memory Stick\$WinpeDriver$\motherboard\
```
 
In order to do this, you will need to logon on to your computer manufacturers website and download the drivers specifically for you computer. Some driver installation programs offer the option to install or extract the drivers when running the driver installation program. Others have a command line switch which can extract the drivers, that needs to be run from the DOS command window or powershell window. Other manufacturers will provide a CAB or the newer DUP file where it can be extracted 




https://learn.microsoft.com/en-us/troubleshoot/windows-client/setup-upgrade-and-drivers/limitations-dollar-sign-winpedriver-dollar-sign


https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-pnpcustomizationswinpe



<DriverPaths>
<!-- First PathAndCredentials list item -->
   <PathAndCredentials wcm:action="add" wcm:keyValue="1">
      <Path>\\myFirstDriverPath\DriversFolder</Path>
      <Credentials>
         <Domain>MyDomain.local</Domain>
         <Username>MyAdministratorUsername</Username>
         <Password>MyAdministratorPassword</Password>
      </Credentials>
   </PathAndCredentials>
<!-- Second PathAndCredentials list item -->
   <PathAndCredentials wcm:action="add" wcm:keyValue="2">
      <Path>C:\Drivers</Path>
      <Credentials>
         <Domain>MyComputerName</Domain>
         <Username>MyUsername</Username>
         <Password>MyPassword</Password>
      </Credentials>
   </PathAndCredentials>
</DriverPaths>


https://www.tenforums.com/installation-upgrade/178735-answer-file-autounattend-xml-diskid-changes-after-loading-drivers.html