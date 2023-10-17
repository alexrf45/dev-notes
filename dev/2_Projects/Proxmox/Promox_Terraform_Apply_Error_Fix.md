
Ref: https://github.com/Telmate/terraform-provider-proxmox/issues/460

Set iothread = 0 in the disk block. permits the rest of the apply to work. Not sure why this is the case but it worked for me. 