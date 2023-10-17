Ref: https://guides.wp-bullet.com/add-custom-ascii-banner-logo-to-ssh-login/

Text to ASCCCII art converter: http://patorjk.com/software/taag/#p=display&f=Graffiti&t=Type%20Something%20

Sets a motd (message of the day) displayed for ssh login:
```bash
sudo vim /etc/motd
```

Changed SSH prelogin banner:

```bash
sudo vim /etc/ssh/sshd-banner

#add text
```

```bash
sudo vim /etc/sshd/sshd_config

Banner /etc/ssh/sshd-banner

restart ssh server service

sudo systemctl restart sshd.service

```

SSH forwarding from SSH cli:

#ssh #ssh-forwarding #commandline #linux #pivoting 

```bash
~C after newline

#redirection
-L port:host:port or -R port:host:port -D port 

```


Legacy SSH access: 

https://askubuntu.com/questions/836048/ssh-returns-no-matching-host-key-type-found-their-offer-ssh-dss

#ssh #ubuntu #troubleshooting

Persistant SSH agent via .bashrc or .zshrc: 

```bash
if [ -z "$SSH_AUTH_SOCK" ] ; then
    eval `ssh-agent -s`
    ssh-add ~/.ssh/<NAME OF YOUR PRIVATE KEY>
    fi
```

#ssh #remotelinux #linux #sysadmin 

Rtop - remote system monitor via ssh: 

```
go install github.com/rapidloop/rtop@latest

rtop <ssh login alias is using ssh config>
```

#ssh #remotelinux #system-monitoring #sysadmin

Hardening SSH:

/etc/ssh/sshd_config changes:
```
AuthorizedKeysFile /etc/ssh/authorized-keys/%u #removes this file from user's folder and puts its in more secure /etc folder
Protocol2
IgnoreRhosts to yes
HostbasedAuthentication no
PermitEmptyPasswords no
X11Forwarding no
MaxAuthTries 5
Ciphers aes128-ctr,aes192-ctr,aes256-ctr
HostKeyAlgorithms ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,ssh-rsa,ssh-dss
KexAlgorithms ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group14-sha1,diffie-hellman-group-exchange-sha256
MACs hmac-sha2-256,hmac-sha2-512,hmac-sha1
ClientAliveInterval 900 
ClientAliveCountMax 0
UsePAM yes
```

```
chown root:root /etc/ssh/sshd_config
chmod 600 /etc/ssh/sshd_config
```