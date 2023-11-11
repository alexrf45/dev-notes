Objective: Host a personal Gitea Instance to store local projects. 

Installation method: proxmox vm w/ docker compose for services

Server private IP: 10.3.3.61

1. Add service user to server:
```
sudo adduser --system --shell /bin/bash --gecos 'Git Version Control' --group --disabled-password --home /home/gitea gitea
```


