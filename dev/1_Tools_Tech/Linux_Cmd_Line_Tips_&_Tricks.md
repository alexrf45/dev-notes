awk examples:

#commandline #linux #enumeration 

```bash
#enumerate users
cat /etc/passwd | awk -F ":" '{print $1}'

cat /etc/passwd | awk -F ":" '{print $1 $6 $7}'

cat /etc/passwd | awk 'BEGIN{FS=":"; OFS="-"} {print $1,$6,$7}'

#view shells on machine
awk -F "/" '/^\// {print $NF}' /etc/shells

awk -F "/" '/^\// {print $NF}' /etc/shells | uniq | sort 


```

grep examples:
#commandline #post-recon #post-exploitation #enumeration #leakedcreds 
```bash
#recursive seach
grep -r search_term .

#execlude lines that start with a certain character and grep for remaining lines. useful for detecting custom changes to config files.

grep -v "^#" FILE | grep .

```

mkdir examples:

#commandline #linuxserver #sysadmin #file-management #automation #mkdir

```bash
mkdir -p test/{recon,exploit,report}
```

cat examples:

#file-management #file-enum #commandline #linux #linuxserver #bugbounty #subdomain #recon #cat

```bash
Cat hosts.txt | sort -u > subs.txt

Cat subs.txt | httpx -o live.txt

```

strings examples:

#linux #forensics #blueteam #malware_analysis #post-recon #strings

```bash

strings -e L

```

ln examples:

#symbolic-links #linuxserver #remotelinux #linux #linuxprivesc #file-management #ln

```
ln -s ~/.dotiles/.zshrc ~/.zshrc
```

xargs examples:

#file-management #commandline #xargs

```bash

#renames all the files created 
seq 1000 | xargs -I {} touch {}.txt

ls | cut -d. -f1 | xargs -I {} mv {}.txt {}.text - renames all the files created 

#prints out a seq of 1-5 with one arguement, process at a time with a sleep of 1 second in between
seq 5 | xargs -n 1 -P 1 bash -c 'echo $0; sleep 1'

#deletes all files with a .text extension
find foo -type f -name "*.text" -exec rm {}
```

wget examples:

#wget #webapp #devops #exfill 

```bash
#recursive file download
wget -r --no-parent --reject "index.html*" $ADDRESS

#ignore ssl cert check
--no-check-certificate
```

aria2c examples:

#filedownload #webapp #linux #remotelinux 

```bash

alias download='aria2c'
```

asciinema: 

#linuxlogging #terminal #commandline 

Config file example:
```
[api]

; API server URL, default: https://asciinema.org
; If you run your own instance of asciinema-server then set its address here
; It can also be overriden by setting ASCIINEMA_API_URL environment variable
url = https://asciinema.example.com

[record]

; Command to record, default: $SHELL
command = /bin/bash -l

; Enable stdin (keyboard) recording, default: no
stdin = yes

; List of environment variables to capture, default: SHELL,TERM
env = SHELL,TERM,USER

; Limit recorded terminal inactivity to max n seconds, default: off
idle_time_limit = 2

; Answer "yes" to all interactive prompts, default: no
yes = true

; Be quiet, suppress all notices/warnings, default: no
quiet = false

[play]

; Playback speed (can be fractional), default: 1
speed = 2

; Limit replayed terminal inactivity to max n seconds, default: off
idle_time_limit = 1


```

stat: #stat #commandline #file-enum #file-management 

```bash
$ stat FILE 

#reveals recent access date, modify date and file creation date. useful for finding significant changes on a box, usually as part of forensics. 


```

find : #find #commandline 

```bash

# used to find files modified between the ranges
find DIR -type f -newermt YYYY-MM-DD ! -newermt YYYY-MM-DD > /dev/null


```

cmp, diff, tput: #commandline #cmp #diff #tput

```

```

grep: #commandline #grep #diff 

```
# grep for string and display associated file, cat does not provide the filename, merely acts as stdin
grep --with-filename "string " *

```