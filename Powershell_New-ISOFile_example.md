```
PS C:\WINDOWS\system32> import-Module "C:\Users\Admin1\Documents\ISO Files\New-ISOFile.psm1"
PS C:\WINDOWS\system32> New-ISOFile "C:\mount" "C:\Users\Admin1\Documents\ISO Files\WS2016test.iso" -verbose
VERBOSE: Function start.
VERBOSE: Processing nested system
VERBOSE: Adding ISOFile type.
VERBOSE: Adding type for PowerShell 5.
VERBOSE: Selected media type is DVDPLUSRW_DUALLAYER with value 13
VERBOSE: Initialising image object.
VERBOSE: initialised.
VERBOSE: Performing the operation "New-ISOFile" on target "C:\Users\Admin1\Documents\ISO Files\WS2016test.iso".
VERBOSE: Fetching items from source directory.
VERBOSE: Got source items.
VERBOSE: Adding items to image. Wait at least 1 min, then if new lines below dont appear press Enter - It sometimes hangs.
VERBOSE: Adding boot
VERBOSE: Adding efi
VERBOSE: Adding NanoServer
VERBOSE: Adding sources
VERBOSE: Adding support
VERBOSE: Adding autorun.inf
VERBOSE: Adding bootmgr
VERBOSE: Adding bootmgr.efi
VERBOSE: Adding setup.exe
VERBOSE: Writing out ISO file to C:\Users\Admin1\Documents\ISO Files\WS2016test.iso
VERBOSE: Target File Created:C:\Users\Admin1\Documents\ISO Files\WS2016test.iso Starting to write contents, this can take several minutes... Press Enter to update in case it hangs.
VERBOSE: Writing out to ISO file:C:\Users\Admin1\Documents\ISO Files\WS2016test.iso completed.
VERBOSE: File complete.


    Directory: C:\Users\Admin1\Documents\ISO Files


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        21/06/2025     18:29     6971064320 WS2016test.iso
VERBOSE: Function complete.


PS C:\WINDOWS\system32>
```