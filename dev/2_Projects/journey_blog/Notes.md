
- The goal is to host this on an EC2 instance with Lets Encrypt and Nginx via terraform (cloud init userdata to bootstrap installation) and potential github actions but maybe only for backups. That could be sustained by a cron job script. I can either generate certs via terraform or Nginx. A few scripts to set things up isn't the worse, 


# Main components
- Journey binary ran as a service
- SSL Certificate
- Server (EC2)
- Proxy (Nginx)


# Nice to have: 
- Separate database
- Backup cron job
- Fail2Ban
- CrowdSec or Wazuh
- EC2 Snapshot


https://github.com/kabukky/journey/wiki/Setting-up-Journey


```

#download binary
wget https://github.com/kabukky/journey/releases/download/v0.2.0/journey-linux-amd64.zip -O journey.zip && unzip journey.zip -d journey && cd journey



```