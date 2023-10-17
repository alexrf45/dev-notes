Ref: (incorrect permissions though):
https://github.com/chippawah/proxmox-cluster-example


https://github.com/Telmate/terraform-provider-proxmox/issues/432

```
sudo pveum role add TerraformProv -privs "Datastore.AllocateSpace Datastore.Audit Pool.Allocate Sys.Audit Sys.Console Sys.Modify VM.Allocate VM.Audit VM.Clone VM.Config.CDROM VM.Config.Cloudinit VM.Config.CPU VM.Config.Disk VM.Config.HWType VM.Config.Memory VM.Config.Network VM.Config.Options VM.Migrate VM.Monitor VM.PowerMgmt"

sudo pveum user add terraform-prov@pve

sudo pveum aclmod / -user terraform-prov@pve -role TerraformProv

sudo pveum user token add terraform-prov@pve terraform-provisioner --privsep 0

sudo pveum aclmod / -token 'terraform-prov@pve!terraform-provisioner' -role TerraformProv

```