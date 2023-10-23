# 10. Kubernetes installation (WS)

## K8s Installation

### Localhost

**Installed  kubectl for local run:** (in HW 09)
```bash
  56  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  57  sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

```bash
kubectl version
Client Version: v1.28.3
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
Server Version: v1.27.6+k3s1
```
I already installed Go on the previous Homework 09. Kubernetes during the installation of KinD
```bash
 wget https://go.dev/dl/go1.21.3.linux-amd64.tar.gz
    4  mkdir -p $HOME/go/{bin,src}
    5  cd
    6  ls -la
    7  nano .profile
    8  cd /home
    9  tar -xvf go1.21.3.linux-amd64.tar.gz
   10  mv go /usr/local/
   11  cd
   12  . ~/.profile
   13  go version
```
```
cat ~/.profile
export PATH=$PATH:$GOPATH/bin
export GOPATH=$HOME/go
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
```

```bsah
  go version
  go version go1.21.3 linux/amd64
```

**Install k9s to maintain cluster**
```bash
  214* wget https://github.com/derailed/k9s/releases/download/v0.27.4/k9s_Linux_amd64.tar.gz
  215* sudo tar -C /usr/local/bin/ -xzf k9s_Linux_amd64.tar.gz
  216* k9s
```
---
![using_k9s_on_local_host](https://github.com/IPaul32/sa2-ma25-23-Ivanchuk-04.GitOps/assets/145698867/48edee75-bfec-41bb-a27a-9efdfa430c2e)
---

### Add test pod deployment 

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu
  labels:
    app: ubuntu
spec:
  containers:
  - image: ghcr.io/pluhin/busy-box:latest
    command:
      - "sleep"
      - "604800"
    imagePullPolicy: IfNotPresent
    name: ubuntu
  restartPolicy: Always
```

```bash
   251  kubectl apply -f test.yaml
```
---
![add_pod](https://github.com/IPaul32/sa2-ma25-23-Ivanchuk-04.GitOps/assets/145698867/71c670fd-6d7f-486c-9d60-d8349aefe1a7)
---


### Created GitHub action to check status of pods and create slack notification if I have crashed/failed pods

