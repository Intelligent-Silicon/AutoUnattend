# Firefox installation

https://support.mozilla.org/en-US/kb/deploy-firefox-msi-installers

Download the MSI installer for Firefox from [www.mozilla.org/en-GB/firefox/all/](https://www.mozilla.org/en-GB/firefox/all/)

```
1. Browser: Firefox
2. Platform: Windows 32-bit
3. Language: English (British) - English (British)
4. Download Now: 
```

For a complete list of the MSI command line switches visit [support.mozilla.org/en-US/kb/deploy-firefox-msi-installers](https://support.mozilla.org/en-US/kb/deploy-firefox-msi-installers).


```
PS C:\WINDOWS\system32> $OEMPath = 'C:\mount_Win10_22H2_x32_ISO\$OEM$\$1\Software Installers\Firefox x32'
PS C:\WINDOWS\system32> md -path $OEMPath
PS C:\WINDOWS\system32> Copy-Item "C:\Users\Admin1\Downloads\Firefox Setup 140.0.2.exe" -Destination "$OEMPath\Firefox Setup 140.0.2.exe"

```



```
<settings pass="specialize">
    <component name="Microsoft-Windows-Deployment" processorArchitecture="x86" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <RunSynchronous>
            <RunSynchronousCommand wcm:action="add">
                <Description>Disable Win10 First Login Animation</Description>
                <Order>nnn</Order>
                <Path>"C:\Software Installers\Firefox x32\Firefox Setup 140.0.2.exe" /S </Path>
                <WillReboot>Never</WillReboot>
            </RunSynchronousCommand>
        </RunSynchronous>    
    </component>
</settings>
```


 