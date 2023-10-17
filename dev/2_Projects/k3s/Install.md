https://www.suse.com/c/rancher_blog/deploying-k3s-with-ansible/

https://pet2cattle.com/2021/11/k3s-multimaster-embedded-db

so much easier to install!! now I can have k3s up and running in less than 5 minutes. boom!!!

https://0to1.nl/post/k3s-kubectl-permission/

control-plane primary:
```
curl -sfL https://get.k3s.io | sh -s server --cluster-init --token "r0land_home_lab"
```

secondary, third control plane:
```
curl -sfL https://get.k3s.io | K3S_TOKEN="r0land_home_lab" sh -s server --server https://10.3.3.51:6443
```


change permissions of kubeconfig:

```
sudo chmod 755 /etc/rancher/k3s/ks3.yaml
```

export KUBECONFIG on control plane(s):
```
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
```

copy kubeconfig to local machine:
```
scp ubuntu@10.3.3.51:/etc/rancher/k3s/ks3.yaml .kube/config 

edit config ip to reflect primary IP: 10.3.3.51
```

add workers: 
```
curl -sfL https://get.k3s.io | K3S_URL=https://10.3.3.51:6443 K3S_TOKEN=home_lab_r0land  sh - 
```

verify nodes are up and running: 

```
kubectl get nodes
```

Remove cluster-init from main worker (Manual, won't work with ansible)
```
sed -e '/server \\/,$d' -e 's@ExecStart=.*@ExecStart=/usr/local/bin/k3s server@' -i /etc/systemd/system/k3s.service
systemctl daemon-reload
```
