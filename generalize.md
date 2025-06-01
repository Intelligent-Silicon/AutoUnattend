# generalize

The generalize configuration pass of Windows Setup is used to create a Windows Reference Image that can be used throughout an organization.

Developing the Reference Image is primarily developed on a virtual pc like VMWare, VirtualBox or others, before being run through the System Preparation (Sysprep) tool and saved as a Windows Imaging (WIM) file.

Developing a Reference Image on a virtual pc brings these 


    To reduce development time and can use snapshots to test different configurations quickly.
    To rule out hardware issues. You get the best possible image, and if you've a problem, it's not likely to be hardware related.
    To ensure that you won't have unwanted applications that could be installed as part of a driver install but not removed by the Sysprep process.
    The image is easy to move between lab, test, and production.
  


Settings in the generalize configuration pass enable you to automate the behavior for all deployments of this reference image. In comparison, settings applied in the specialize configuration pass enable you to override behavior for a single, specific deployment.


https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/generalize

https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/deployment/deploy-windows-mdt/create-a-windows-10-reference-image

The reference image described in this guide is designed primarily for deployment to physical devices. However, the reference image is typically created on a virtual platform, before being automatically run through the System Preparation (Sysprep) tool process and captured to a Windows Imaging (WIM) file. The reasons for creating the reference image on a virtual platform are: