```bash
pacman -Ql #show all packages installed

pacman -Qs #show all files installed by packages

#number of explictly installed packages
pacman -Qe | wc -l

#manually installed or yay packages
pacman -Qm 


```

#archlinux #pacman #packagemanager #linux 

Enter virtual tty for fixes:

```
CtrlAltF2

```

#commandline 

https://www.maketecheasier.com/use-aur-in-arch-linux/ - set up yay for AUR package managment instead of aur build
#yay #archlinux 

https://www.fosslinux.com/46511/switch-between-different-linux-kernels-arch-linux.htm - to switch kernels if needed

#linux-kernel #linux

Enable parallel downloads in arch:

Pacman 6.0 introduced the option to download packages in parallel. ParallelDownloads under [options] needs to be set to a positive integer in /etc/pacman.conf to use this feature (e.g., 5). Packages will otherwise be downloaded sequentially if this option is unset.

