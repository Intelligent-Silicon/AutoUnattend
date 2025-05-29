# <offlineServicing>

Packages listed in the <servicing> section and settings in the <offlineServicing> section of the answer file are applied to the offline Windows image.
The offlineServicing pass is where you can add language packs, update packages, device drivers, or other packages to the offline image.


Update Windows faster by using DISM to make your changes without ever booting Windows. Mount an image to a temporary location, install apps, drivers, languages, and more, and then commit the changes so they can be applied to new devices. DISM requires an elevated command-line or from PowerShell, which makes it easier to automate your changes with scripts.



https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/offlineservicing


# ```<DriverPaths>```

```<DriverPaths>``` is where you can specify one or more paths to folders that can contain out of the box drivers aka drivers extracted from their setup installer program. These are copied to the driver store of the windows image during the windowsPE pass.

# ```<Path>```
```<Path>``` is the local or UNC (Universal Naming Convention) path to the location that contains out of the box drivers. It does not look in subfolders of the specified path. 

```<Path>\\myUNCdriverpath\aDriverFolder</Path>```
```<Path>C:\Drivers</Path>```
```<Path>\Drivers</Path>```


$OEM$ folder must be created under the ISO's \sources directory.

For me, I put unattend.xml and $oem$ in sources folders.

- sources \ $oem$ \ $1 \ FileWillBeAtRoot.txt

\sources\$OEM$
\sources\$OEM$\$$\file1
\sources\$OEM$\$1\file2

Don't expect Driver's to work from $oem$ copy because here as I've noticed by looking at the drive i'm installing to, $oem$ don't get copied till right after it does the update thing and right before doing the Performance check
Driver's are working here but you have to point to them.
Note: Driver's are done early on, PE is looking for them or right after if you are pointing otherwise an error.

https://www.tenforums.com/installation-upgrade/178735-answer-file-autounattend-xml-diskid-changes-after-loading-drivers.html

https://www.tenforums.com/general-support/203561-add-applications-answer-file-windows-10-a-2.html

https://www.tenforums.com/tutorials/96683-create-media-automated-unattended-install-windows-10-a-91.html

https://pastebin.com/KJB1agCZ

https://brookspeppin.com/2022/01/29/build-a-fast-diy-usb-zero-touch-provisioning-process-for-dell/

https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-pnpcustomizationswinpe-driverpaths



<settings pass="offlineServicing">
        <component name="Microsoft-Windows-LUA-Settings" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <EnableLUA>false</EnableLUA>
        </component>
    </settings>
	
	<settings pass="offlineServicing">
<component name="Microsoft-Windows-LUA-Settings" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<EnableLUA>true</EnableLUA>
</component>
</settings>

windowsPE and offlineServicing configuration passes:

\Sources directory in a Windows distribution

All other passes:

%WINDIR%\System32\Sysprep


<settings pass="offlineServicing">
		<component name="Microsoft-Windows-PnpCustomizationsNonWinPE" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
			<DriverPaths>
				<PathAndCredentials wcm:keyValue="1" wcm:action="add">
					<Path>\Drivers</Path>
				</PathAndCredentials>
			</DriverPaths>
		</component>
	</settings>
	
	
	<settings pass="offlineServicing">
    <component name="Microsoft-Windows-PnpCustomizationsNonWinPE" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <DriverPaths>
            <PathAndCredentials wcm:keyValue="1" wcm:action="add">
                <Path>C:\Drivers</Path>
            </PathAndCredentials>
        </DriverPaths>
    </component>
</settings>

<settings pass="offlineServicing">
    <component name="Microsoft-Windows-PnpCustomizationsNonWinPE" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <DriverPaths>
            <PathAndCredentials wcm:keyValue="1" wcm:action="add">
                <Path>C:\Drivers</Path>
            </PathAndCredentials>
        </DriverPaths>
    </component>
</settings>