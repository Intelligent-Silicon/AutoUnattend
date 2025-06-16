PS C:\WINDOWS\system32> Dism /Image:"C:\mount" /Add-Package /PackagePath:"C:\Users\Admin1\Documents\WIM files\windows11.0-kb5063060-x64_96be31e3e3e1cbc216229abb83e5be9da4e08496.msu"

Deployment Image Servicing and Management tool
Version: 10.0.26100.1150

Image Version: 10.0.26100.2033

Processing 1 of 1 -
[==========================100.0%==========================]
[=====                      10.0%                          ] C:\Users\Admin1\Documents\WIM files\windows11.0-kb5063060-x64_96be31e3e3e1cbc216229abb83e5be9da4e08496.msu: An error occurred applying the Unattend.xml file from the .msu package.
For more information, review the log file.
 Error: 0x80070780

Error: 1920

The file cannot be accessed by the system.

The DISM log file can be found at C:\WINDOWS\Logs\DISM\dism.log
PS C:\WINDOWS\system32>