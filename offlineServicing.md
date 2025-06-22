# offlineServicing Pass

This pass can be used to install setup programs which accept command line switches to an offline Windows image.

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/offlineservicing

These can include language packs, windows update packages, drivers, and programs for users.

This pass runs during the windowsPE Microsoft-Windows-Setup pass, by extracting and installing windows, and the runs the DISM (Deployment Image Servicing and Management) program. 

As at 20250613:YYYYMMDD, DISM using WimFltr v2 extractor, with the command Dism /add-package used in ```offlineServicing``` fails to add Window's Update .MSU packages to the ```.ESD``` windows image file format.

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/what-is-dism

The full technical detail of the WIM file can be found at this link. https://go.microsoft.com/fwlink/?LinkId=92227

WindowsPE will only mount an image, which means it will be offline only and never online because its never booted up at this stage. 

If you want to use the ```%configsetroot%``` with DISM, make sure the below ```<UseConfigurationSet>``` line is added to ```<component name="Microsoft-Windows-Setup"``` as seen in the example below.

```
<component name="Microsoft-Windows-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
	<UseConfigurationSet>true</UseConfigurationSet>
</component>
```

DISM command to mount the windows WIM file on the USB stick. This assumes a subfolder call mount exists. If you use a folder name with spaces, make sure paths are encapsulated with quotes as seen below, but using quotes is a good habit to have. 
```
Dism /Image:"%configsetroot%\sources\install.wim":\offline /Source:"%configsetroot%\mount\windows"
```

The USB stick created by the Media Creation tool contains a folder called ```\sources```. There will be two WIM files. ```Boot.WIM``` is the windowsPE operating system and ```Install.WIM``` is is the copy of windows which will be installed. 
Once ```Install.WIM``` is mounted to a subfolder on the usb stick, you can then run other DISM commands to customise the windows which will be installed. 

For more information on mounting and customising the installation version of windows, see this link.
https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-mount-and-customize


# ```/commit```

Remember to use the ```/commit``` option when unmounting the ```Install.WIM``` in order to save your changes before its installed on the computer.

# DISM windowsPE command reference

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/deployment-image-servicing-and-management--dism--command-line-options?view=windows-11



# Add driver

```
Dism /Add-Driver /Image:"%configsetroot%\mount" /Driver:"%configsetroot%:\SomeSubFolder\driver.inf"
```
The .INF file contains a list of additional files which are needed for the device driver to work, so those files will be copied onto the mount automatically for you.

If copying user programs onto the mounted Install.WIM, remember this may need more than just files copied across to the mount.  These files could also be located in other folders, like ```.ini``` files in ```C:\Windows```, and config or data files in %USERPROFILE% folders and %appdata% folders, as well as also having registry settings in a variety of registry hives like HKLM, HKU and HKCU. 

This can be more work generally unless the program is a simple standalone program, and its generally best, if the setup program accepts command line switches, to install the program using its setup program with the neccessary command line switches.
  

