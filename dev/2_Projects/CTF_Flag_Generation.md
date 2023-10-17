```
while true; do head /dev/urandom | md5sum | cut -d " " -f1| xargs printf "flag{%s}\n" | tee -a flags.txt; done
```
