# Install Windows

## Windows Media Creation Tool 


https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/how-configuration-passes-work

Lists Window's 10 variants to txt. Both x86 (32bit) and x64 (64bit) versions on the USB stick. 
```
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"D:\x64\sources\install.esd" | Out-File -FilePath "D:\x64\sources\install.esd.txt"
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"D:\x86\sources\install.esd" | Out-File -FilePath "D:\x86\sources\install.esd.txt"
```

Lists Window's 11 variants to txt. Windows 11 only comes in 64bit versions.
```
PS C:\WINDOWS\system32> Dism /Get-ImageInfo /ImageFile:"D:\sources\install.esd" | Out-File -FilePath "D:\sources\install.esd.txt"
```



## WindowsPE - WindowsPE Settings

[Component - Microsoft-Windows-International-Core-WinPE](AutoUnattend-WindowPE-Microsoft-Windows-International-Core-WinPE.md)

## WindowsPE - Windows Setup Settings

[Component - Microsoft-Windows-Setup](AutoUnattend-WindowsPE-Microsoft-Windows-Setup.md)

[Component - Microsoft-Windows-PnpCustomizationsWinPE](AutoUnattend-WindowsPE-Microsoft-Windows-PnpCustomizationsWinPE.md)

## offlineServicing





