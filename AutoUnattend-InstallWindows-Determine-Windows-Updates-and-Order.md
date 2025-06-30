# Determine the Windows Updates and Order of Installation


Run Windows Update and make a note of the updates.

Windows 10 Pro 2004 
Download Windows 10 2022 Update | version 22H2 Media Creation Tool

Windows 10, versions 2004 and 20H2 share a common core operating system with an identical set of system files. 
Therefore, the new features in Windows 10, version 20H2 are included in the latest monthly quality update for
Windows 10, version 2004 (released October 13, 2020).

DISM /mount-image shows "Image Version: 10.0.19041.1" where 19041 is the Build Number.
In this document https://learn.microsoft.com/en-us/windows/release-health/release-information it refers to Version 2004 which you can see if you scroll down the page.
OFfline updates for this are no longer available are only available from Windows Updates. I suspect this might be to keep an eye on how many people are downloading updates of this version because the Windows Catalog updates are more generic and cover a wider range of windows versions.

Dism /mount-image /imagefile:"C:\mount\sources\install.wim" /index:1 /mountdir:"C:\mount_Win10_Pro_x32"

Windows Updates: In Order of Installation
1. Windows Malicious Software Removal Tool - v5.134 (KB890830)
2. 2023-10 Update for Windows 10 Version 22H2 for x86-based Systems (KB4023057)
3. 2025-06 Cumulative Update for Windows 10 Version 22H2 for x86-based Systems (KB5060533)


1. https://www.catalog.update.microsoft.com/Search.aspx?q=Windows%20Malicious%20Software%20Removal%20Tool%20-%20v5.134%20(KB890830) 
2. https://www.microsoft.com/en-us/download/details.aspx?id=103324
3. 

Windows 11 Home - 

 
VMware Workstation

Windows 10 Pro

Windows Updates: In Order of Installation
*****************************************************************
Windows Malicious Software Removal Tool - v5.134 (KB890830)
*****************************************************************
https://www.catalog.update.microsoft.com/Search.aspx?q=Windows%20Malicious%20Software%20Removal%20Tool%20-%20v5.134%20(KB890830)

This is an EXE "windows-kb890830-v5.134_b63113971ecc78f959b6e08d9348d943292d2a4e.exe" so it needs to be installed after the Windows Setup from a 
Yes, copy the files on the computer, then create a batch file here with the command to start your executable: %WINDIR%\Setup\Scripts\SetupComplete.cmd 
Windows will start the batch file after Windows setup. https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/add-a-custom-script-to-windows-setup?view=windows-11

*****************************************************************
2023-10 Update for Windows 10 Version 22H2 for x86-based Systems (KB4023057)
*****************************************************************

Download from https://www.microsoft.com/en-us/download/details.aspx?id=103324
Its not visible as a KB4023057 for Windows 22H2 in windows catalog (Sort Last Updated by Descending)
https://www.catalog.update.microsoft.com/Search.aspx?q=KB4023057+x86&scol=DateComputed&sdir=desc
So download from the first link above.
https://support.microsoft.com/en-gb/topic/kb4023057-update-health-tools-windows-update-service-components-fccad0ca-dc10-2e46-9ed1-7e392450fb3a

Dism /Image:"C:\mount_Win10_Pro_x32" /Add-Package /PackagePath:"C:\Users\Admin1\Documents\ISO Files\Expedite_packages_KB4023057_update-health-tools\Expedite_packages\Windows 10\UpdHealthTools.msi"

*****************************************************************
2025-06 Cumulative Update for Windows 10 Version 22H2 for x86-based Systems (KB5060533)
*****************************************************************

Dism /Image:"C:\mount_Win10_Pro_x32" /Add-Package /PackagePath:"C:\Users\Admin1\Documents\ISO Files\windows10.0-kb5060533-x86_de4a47dde17d91023f93eb9a37c6c96faebf768c.msu"




