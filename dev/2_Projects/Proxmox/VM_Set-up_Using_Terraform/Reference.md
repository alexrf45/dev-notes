Reference: https://austinsnerdythings.com/2021/09/01/how-to-deploy-vms-in-proxmox-with-terraform/

https://pve.proxmox.com/wiki/Cloud-Init_Support

https://cloud-images.ubuntu.com/ - Cloud init images

https://github.com/Telmate/terraform-provider-proxmox/issues/164 - enable log tracing for troubleshooting by 

https://cloudalbania.com/posts/2022-01-homelab-with-proxmox-and-terraform/ - for loop

https://developer.hashicorp.com/terraform/language/values/variables#type-constraints

```bash
export TF_LOG=TRACE

then to turn off verbose logging:

export TF_LOG=ERROR
```

Let's create a repo to store the code for continuity

```bash
gh repo create proxmox-r0land --add-readme -d 'Repo for Proxmox VM set up using Ansible' --private --clone
```

- code works. creates two vms with 4GB ram and 10GB HDD. we can make it bigger as we look to install kubernetes
- 