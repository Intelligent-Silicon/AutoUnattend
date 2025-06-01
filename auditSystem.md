# auditSystem




The auditSystem and auditUser configuration passes only run when you configure Windows Setup to boot into audit mode using the methods below.

During the auditSystem configuration pass, if ```<Mode>``` is not specified, then Mode defaults to Audit.

During the auditUser configuration pass, if ```<Mode>``` is not specified, then the computer shows the Sysprep tool's user interface (UI), prompting the user to select between Audit or OOBE mode.

During the oobeSystem configuration pass, if ```<Mode>``` is not specified, then Mode defaults to OOBE.


# auditMode methods

### ```<Microsoft-Windows-Deployment>```

```
<component name="Microsoft-Windows-Deployment" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
	<Reseal>
		<Mode>Audit</Mode>
	</Reseal>
</component>
```
```
<component name="Microsoft-Windows-Deployment" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
	<Reseal>
		<Mode>OOBE</Mode>
	</Reseal>
</component>
```
https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-deployment-reseal

### Command line

This command can be used in the ```specialize``` configuration pass from a ```RunSynchronousCommand``` either as a command or from within a script stored in ```<Extensions>```. It uses the auditpol program to set the mode.

auditpol command line switches.

https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/auditpol

auditpol SubcategoryGUID {0CCE922B-69AE-11D9-BED3-505054503030}

https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-gpac/77878370-0712-47cd-997d-b07053429f6d


```  
<settings pass="specialize">
	<component name="Microsoft-Windows-Deployment" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
		<RunSynchronousCommand wcm:action="add">
			<Order>1</Order>
			<Path>
			 auditpol.exe /set /subcategory:"{0CCE922B-69AE-11D9-BED3-505054503030}" /success:enable /failure:enable
			</Path>
		</RunSynchronous>
	</component>
</settings>
```

```  
<settings pass="specialize">
	<component name="Microsoft-Windows-Deployment" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
		<RunSynchronousCommand wcm:action="add">
			<Order>1</Order>
			<Path>
			powershell.exe -WindowStyle Normal -NoProfile -Command "Get-Content -LiteralPath 'C:\Windows\Setup\Scripts\Specialize.ps1' -Raw | Invoke-Expression;"
			</Path>
		</RunSynchronous>
	</component>
</settings>
```


During the auditSystem configuration pass, if Mode is not specified, then Mode defaults to Audit.

During the auditUser configuration pass, if Mode is not specified, then the computer shows the Sysprep tool's user interface (UI), prompting the user to select between Audit or OOBE mode.

During the oobeSystem configuration pass, if Mode is not specified, then Mode defaults to OOBE.




{0CCE922B-69AE-11D9-BED3-505054503030}
	

Identifies the Process Creation audit subcategory.

This subcategory audits events generated when a process is created or starts. The name of the application or user that created the process is also audited.




IF auditMode is activated, the auditSystem configuration pass processes unattended Windows Setup settings in system context in audit mode.

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/auditsystem

```
<settings pass="auditSystem">
</settings>
```