# Deploy mainnet DAPP-DSP using K8S
This helm chart will install an full mainnet DSP cluster (Syncd Mainnet API Node, DAPP DSP Services, IPFS Cluster)

## Minimum cluster requirements

* CPU: 4x2.2GHz Cores
* Memory: 64GB memory
* Network: 1 GigE
* Disk: 1TB

## Getting started
### AWS
[EKS](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html)

### GCP 
[GKE](https://cloud.google.com/kubernetes-engine/docs/quickstart)

## Deployment
### Make sure kubectl is installed and configured and nodes are running
```
kubectl get nodes
```
### Run boostrap container 
For GCP:
```bash
docker run -v /google/google-cloud-sdk:/google/google-cloud-sdk \
    --entrypoint /bin/bash --rm -it -v $HOME/.kube/config:/root/.kube/config \
    liquidapps/zeus-dsp-bootstrap 
```

Others:
```bash
docker run --entrypoint /bin/bash --rm -it -v $HOME/.kube/config:/root/.kube/config \
    liquidapps/zeus-dsp-bootstrap 
```

Inside the container shell:
```bash

# create tiller service account
kubectl -n kube-system create serviceaccount tiller
kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller

# install helm on cluster
helm init --service-account tiller
helm repo update
```

### Edit values.yaml (optional)
### Deploy
Restore from snapshot:
```bash
zeus deploy dapp-cluster dspaccount --key yourdspprivatekey
```
Or restore from full backup and replay:
```bash
zeus deploy dapp-cluster dspaccount --key yourdspprivatekey --full-replay=true 
```
Or resume after first restore:
```bash
zeus deploy dapp-cluster dspaccount --key yourdspprivatekey --snapshot=false
```

### Monitor restore and sync progress 
```bash
kubectl logs -f dsp-nodeos-0 --all-containers
```
*It takes a couple of hours to restore from the blockchain backups depending on internet connection and hardware performance*


### Get your API endpoint 
AWS:
```bash
DSP_ENDPOINT=$(kubectl get service dsp-dspnode -o jsonpath="{.status.loadBalancer.ingress[?(@.hostname)].hostname}"):3115
echo $DSP_ENDPOINT
```

GCP:
```bash
DSP_ENDPOINT=$(kubectl get service dsp-dspnode -o jsonpath="{.status.loadBalancer.ingress[0].ip}"):3115
echo $DSP_ENDPOINT
```