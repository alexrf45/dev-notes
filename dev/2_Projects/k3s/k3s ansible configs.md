#ansible #k3s
## ansible.cfg
```
[defaults]
inventory = inventory/hosts.ini  ; Adapt this to the path to your inventory file

; Don't type "yes" for every new server in your inventory.
; This is very useful when many servers are being configured.
host_key_checking = False

; An alternative to specifying "ansible_ssh_private_key_file" over
; and over in the inventory file
private_key_file = live/development/prox

; Run playbooks faster. There are some gotchas so read the docs before using this one!
pipelining = True

; Default hosts value in a playbook to a bogus value.
; Prevents running a playbook against all servers on accident.
hosts = noDefaultForSafety

```

## k3main.yaml

```yaml
- name: Install k3 on primary controlplane
  gather_facts: true
  hosts: control-plane-001
  tasks:
    - name: main controlplane
      shell: |
        curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION="v1.27.3+k3s1"         INSTALL_K3S_EXEC="server" sh -s - --cluster-init \
        --cluster-dns=1.1.1.1 --cluster-domain=cloud-fam.com \
        --flannel-backend=wireguard-native --write-kubeconfig-mode=644 \
        --https-listen-port 7500 --cluster-cidr "10.20.0.0/16" 
        --service-cidr "10.21.0.0/16" --cluster-dns "10.21.0.5" \
        --token "r0land_home_lab"

- name: Install k3 on secondary controlplane
  gather_facts: true
  hosts: control-plane-001a
  tasks:
    - name: k3 controlplane 2
      shell: |
        curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION="v1.27.3+k3s1"         INSTALL_K3S_EXEC="server" sh -s - \
        --cluster-dns=1.1.1.1 --cluster-domain=cloud-fam.com \
        --flannel-backend=wireguard-native --write-kubeconfig-mode=644 
        --https-listen-port 7500 \
        --cluster-cidr "10.20.0.0/16" --service-cidr "10.21.0.0/16" 
        --cluster-dns "10.21.0.5" \
        --token "r0land_home_lab" --server https://10.3.3.50:7500

- name: Install k3 on worker nodes
  gather_facts: true
  hosts: node
  tasks:
    - name: k3 Workers
      shell: |
	    sleep 20 ; curl -sfL https://get.k3s.io \
	    | INSTALL_K3S_VERSION="v1.27.3+k3s1" \
        K3S_URL=https://10.3.3.50:7500 K3S_TOKEN="r0land_home_lab"  sh -

- name: copy config
  gather_facts: true
  hosts: control-plane-001
  tasks:
    - name: export KUBECONFIG
      shell: |
        echo 'export KUBECONFIG=/etc/rancher/k3s/k3s.yaml' >> .bashrc
        cp /etc/rancher/k3s/k3s.yaml ~/.kube/config


```



