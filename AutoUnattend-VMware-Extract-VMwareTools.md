Extracting VMware Tools to a folder.

This appears to be only possible using the command below from within a Virtual PC.

setup.exe /a c:\extract

or 

setup.exe /a /p c:\extract

Then you need to copy the folder contents of c:\extract back to the host using cut and paste.

PS C:\WINDOWS\system32> Invoke-WebRequest "https://packages-prod.broadcom.com/tools/releases/12.4.5/windows/VMware-tools-windows-12.4.5-23787635.iso" -OutFile "C:\Users\Admin1\Documents\ISO Files\VMware-tools-windows-12.4.5-23787635.iso"
PS C:\WINDOWS\system32> New-Item -ItemType Directory -Path "C:\mount_VmwareTools"
PS C:\WINDOWS\system32> New-Item -ItemType Directory -Path "C:\VmwareTools"
PS C:\WINDOWS\system32> New-Item -ItemType Directory -Path "C:\VmwareTools Drivers"
PS C:\WINDOWS\system32> $DiskImageResult = Mount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\VMware-tools-windows-12.4.5-23787635.iso"
PS C:\WINDOWS\system32> $DiskImageDriveLetter = ($DiskImageResult | Get-Volume).DriveLetter
PS C:\WINDOWS\system32> Copy-Item -Path "$($DiskImageDriveLetter):\*" -Destination "C:\VmwareTools" -Recurse
PS C:\WINDOWS\system32> Dismount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\VMware-tools-windows-12.4.5-23787635.iso"
PS C:\WINDOWS\system32> C:\VmwareTools\setup /A "C:\VmwareTools Drivers"

C:\VmwareTools\Program Files\VMware\VMware Tools\Drivers\pvscsi

https://techdocs.broadcom.com/us/en/vmware-cis/vsphere/tools/12-4-0/vmware-tools-administration-12-4-0/configuring-vmware-tools-components/using-vmware-tools-configuration-file/security-considerations-to-configure-vmware-tools.html


https://techdocs.broadcom.com/us/en/vmware-cis/vsphere/tools/12-4-0/vmware-tools-administration-12-4-0/configuring-vmware-tools-components/using-vmware-tools-configuration-file/security-considerations-to-configure-vmware-tools.html

isolation.tools.copy.disable = "TRUE"
isolation.tools.paste.disable = "TRUE"

isolation.device.connectable.disable = "TRUE"
isolation.device.edit.disable = "TRUE"

tools.setInfo.sizeLimit = "1048576"

isolation.tools.unity.push.update.disable = "TRUE"
isolation.tools.ghi.launchmenu.change = "TRUE"
isolation.tools.ghi.autologon.disable = "TRUE"
isolation.tools.hgfsServerSet.disable = "TRUE"
isolation.tools.memSchedFakeSampleStats.disable = "TRUE"
isolation.tools.getCreds.disable = "TRUE"

 https://packages.vmware.com/tools/releases/latest/windows/