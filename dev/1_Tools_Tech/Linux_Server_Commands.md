#linux #linuxserver #sysadmin 

### Add sudo user

Ref: https://linuxize.com/post/how-to-create-a-sudo-user-on-debian/

```bash
usermod -aG sudo username

export EDITOR=nano

echo "username  ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/username
```

### Mounting nfs remote share

#remotelinux #file-management 

```bash
#install nfs-kernel-server, nfs-common

#edit /etc/exports to grant share access

#create mount point:

sudo mkdir dir/dir #usually /media/nfs

#mount the drive:

sudo mount IP:/dir/dir /remotedir/dir

#verify mount:

df -h 

#umount drive

sudo umount dir/dir
```