# Mount/Unmount Windows ISO

Mount-DiskImage -ImagePath "C:\path\to\your\iso.iso"

Dismount-DiskImage -ImagePath "C:\path\to\your\iso.iso"



Using DISM
DISM /Mount-image /imagefile:<path_to_Image_file> {/Index:<image_index> | /Name:<image_name>} /MountDir:<target_mount_directory> [/readonly] /[optimize]}

Dism /Unmount-image /MountDir:<target_mount_directory> {/Commit | /Discard}

https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/mount-and-modify-a-windows-image-using-dism?view=windows-11