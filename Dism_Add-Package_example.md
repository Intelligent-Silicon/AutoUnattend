```
PS C:\WINDOWS\system32> Dism /Image:"C:\mount" /Add-Package /PackagePath:"C:\Users\Admin1\Documents\WIM files\windows11.0-kb5060842-x64_07871bda98c444c14691e0a90560306703b739cf.msu"

Deployment Image Servicing and Management tool
Version: 10.0.26100.1150

Image Version: 10.0.26100.2033

Processing 1 of 1 -
[==========================100.0%==========================]

```

```
PS C:\WINDOWS\system32> Dism /mount-image /imagefile:"C:\Users\Admin1\Documents\WIM files\installW11.wim" /mountdir:"C:\mount" /index:7   # If not already Mounted

Deployment Image Servicing and Management tool
Version: 10.0.26100.1150

Mounting image
[==========================100.0%==========================]
The operation completed successfully.
PS C:\WINDOWS\system32>
PS C:\WINDOWS\system32> Dism /Image:"C:\mount" /Add-Package /PackagePath:"C:\Users\Admin1\Documents\WIM files\windows11.0-kb5060842-x64_07871bda98c444c14691e0a90560306703b739cf.msu"
>>

Deployment Image Servicing and Management tool
Version: 10.0.26100.1150

Image Version: 10.0.26100.2033

Processing 1 of 1 -
[==========================100.0%==========================]
[=====                      10.0%                          ] C:\Users\Admin1\Documents\WIM files\windows11.0-kb5060842-x64_07871bda98c444c14691e0a90560306703b739cf.msu: An error occurred applying the Unattend.xml file from the .msu package.
For more information, review the log file.
 Error: 0x80070780

Error: 1920

The file cannot be accessed by the system.

The DISM log file can be found at C:\WINDOWS\Logs\DISM\dism.log
PS C:\WINDOWS\system32>
```