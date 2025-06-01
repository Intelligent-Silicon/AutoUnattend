# generalize

The generalize configuration pass of Windows Setup is used to create a Windows Reference image that can be used throughout an organization.

This pass is geared towards big business with a fleet of identical computers that would typically be deployed to a department. This section is beyond the scope of this repo which is aimed for individual pc's and small businesses.

However, developing the Reference Image is primarily developed on a virtual pc like VMWare, VirtualBox or others, before being run through the System Preparation (Sysprep) tool and saved as a Windows IMage (WIM) file. The Reference Image will not only contain windows, the correct device drivers, but also user programs which the organisation allows their users to use like MS Office, custom built programs pertinent to their department tasks or business functions and access to other resources like network printer's and other devices, and even restrict access to certain websites.

Developing a Reference Image on a virtual pc has these advantages:
```
	It reduces the time spent developing the Reference Image because you can use different virtual pc snapshots to test different configurations.
	Using a virtual pc removes hardware issues because millions or even billions of virtual pc's exist and run with standardised hardware and drivers removing some problems. 
	You can remove unwanted applications that might be installed as part of the device driver install that is not removed by the Sysprep program.
	You can copy, move and duplicate virtual pc's for lab's, testing and production.
```

To develop a Reference Image visit these links.

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/generalize

https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/deployment/deploy-windows-mdt/create-a-windows-10-reference-image

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation
	