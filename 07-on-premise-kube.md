# update

```bash
sudo apt update
sudo apt upgrade -y
```

# disable firewall

```bash
sudo ufw disable
```

# disable swap

```bash
sed -i '/swap/d' /etc/fstab
swapoff -a
```

# set systemd container runner

```bash
sudo mkdir /etc/docker
```

```bash
cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF
```

# restart docker

```bash
sudo systemctl enable docker
sudo systemctl daemon-reload
sudo systemctl restart docker
```

# Letting iptables see bridged traffic 

```bash
lsmod | grep br_netfilter
sudo modprobe br_netfilter

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sudo sysctl --system
```

# install kubeadm

```bash
sudo apt-get install -y apt-transport-https ca-certificates curl

sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

# spin up cluster

```bash
kubeadm init
```

# set regular user

```bash
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf
```

# install network pod

```bash
https://docs.projectcalico.org/getting-started/kubernetes/self-managed-onprem/onpremises

curl https://docs.projectcalico.org/manifests/calico.yaml -O
kubectl apply -f calico.yaml

kubectl apply -f calico.yaml
```


# verify core dns running

```bash
kubectl get pods --all-namespaces
```

# join worker node

```bash
kubeadm join 139.59.110.144:6443 --token <token> \
	--discovery-token-ca-cert-hash <hash>
```
