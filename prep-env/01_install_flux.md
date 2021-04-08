
# Install Flux on a k8s cluster

### Create a namespace

```bash
kubectl create namespace flux 
```

### Install fluxcd repo 

```bash
helm repo add fluxcd https://charts.fluxcd.io
```

### Install helm package for flux 

```bash
helm upgrade -i flux fluxcd/flux \
--set git.url=git@github.com:papaispicolo/eit_gitops.git \
--namespace flux 

helm upgrade -i helm-operator fluxcd/helm-operator --wait \
--namespace flux \
--set git.ssh.secretName=flux-git-deploy \
--set git.pollInterval=1m \
--set chartsSyncInterval=1m \
--set helm.versions=v3 
```

### Check flux installation, 3 pods should be running 

```bash
kubectl get pods -n flux
```

# Install fluxctl in order to get the SSH key to allow GitHub write access. This allows Flux to keep the configuration in GitHub in sync with the configuration deployed in the cluster.

```bash
sudo wget -O /usr/local/bin/fluxctl https://github.com/fluxcd/flux/releases/download/1.19.0/fluxctl_linux_amd64
sudo chmod 755 /usr/local/bin/fluxctlAE
```

```bash
fluxctl version
fluxctl identity --k8s-fwd-ns flux
```

# Add the public key to your repo 


