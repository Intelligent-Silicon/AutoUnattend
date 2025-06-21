```
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\WINDOWS\system32> $DiskImageResult = Mount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\WS_2016_en-us.ISO"
PS C:\WINDOWS\system32> $DiskImageDriveLetter = ($DiskImageResult | Get-Volume).DriveLetter
PS C:\WINDOWS\system32> Copy-Item -Path "$($DiskImageDriveLetter):\*" -Destination "C:\mount" -recurse

PS C:\WINDOWS\system32>
```