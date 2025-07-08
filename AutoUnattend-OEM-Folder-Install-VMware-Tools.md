# VMware Tools installation

https://techdocs.broadcom.com/us/en/vmware-cis/vsphere/tools/12-4-0/vmware-tools-administration-12-4-0/installing-vmware-tools.html

Download the Vmware Tools 12.4.5.49151, create a mount folder, extract the setup program and copy it to the Windows installer ISO.

```
PS C:\WINDOWS\system32> Invoke-WebRequest "https://packages-prod.broadcom.com/tools/releases/12.4.5/windows/VMware-tools-windows-12.4.5-23787635.iso" -OutFile "C:\Users\Admin1\Documents\ISO Files\VMware-tools-windows-12.4.5-23787635.iso"
PS C:\WINDOWS\system32> md -path "C:\mount_VMware-Tools-12.4.5"
PS C:\WINDOWS\system32> $DiskImageResult = Mount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\VMware-tools-windows-12.4.5-23787635.iso"
PS C:\WINDOWS\system32> $DiskImageDriveLetter = ($DiskImageResult | Get-Volume).DriveLetter
PS C:\WINDOWS\system32> Copy-Item -Path "$($DiskImageDriveLetter):\*" -Destination "C:\mount_VMware-Tools-12.4.5" -Recurse
PS C:\WINDOWS\system32> Dismount-DiskImage -ImagePath "C:\Users\Admin1\Documents\ISO Files\VMware-tools-windows-12.4.5-23787635.iso"
PS C:\WINDOWS\system32> $OEMPath = 'C:\mount_Win10_22H2_x32_ISO\$OEM$\$1\Software Installers\Vmware-Tools'
PS C:\WINDOWS\system32> md -path $OEMPath
PS C:\WINDOWS\system32> Copy-Item "C:\mount_VMware-Tools-12.4.5\setup.exe" -Destination "$OEMPath\setup.exe"

```

https://www.vgemba.net/vmware/VMware-Tools-Drivers/

https://techdocs.broadcom.com/us/en/vmware-cis/desktop-hypervisors/fusion-pro/13-0/specify-vmware-tools-components-for-silent-installations.html

VMware Tools Workstation
VMCI
CBHelper
Perfmon
VmwTimeProvider
FileIntrospection
NetworkIntrospection
ServiceDiscovery
DeviceHelper
Hgfs
SVGA
VMXNet
VMXNet3
PVSCSI
EFIFW
MemCtl
Mouse
MouseUsb
Audio
VSS
BootCamp
SaltMinion


```
<settings pass="specialize">
    <component name="Microsoft-Windows-Deployment" processorArchitecture="x86" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <RunSynchronous>
            <RunSynchronousCommand wcm:action="add">
                <Description>Install VMware Tools</Description>
                <Order>nnn</Order>
                <Path>"C:\Software Installers\Vmware-Tools\setup.exe" /S /v"/qn REBOOT=R ADDLOCAL=ALL REMOVE=AppDefense,FileIntrospection,NetworkIntrospection,Hgfs" /l "C:\Software Installers\Vmware-Tools\VMwareToolsInstall.log"</Path>
                <WillReboot>Never</WillReboot>
            </RunSynchronousCommand>
        </RunSynchronous>    
    </component>
</settings>
```


 