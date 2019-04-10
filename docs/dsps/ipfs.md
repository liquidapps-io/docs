IPFS
====

## Standalone

[go-ipfs](https://dist.ipfs.io/#go-ipfs)
### Hardware Requirements

### Prerequisites 

- golang
- systemd

#### Ubuntu/Debian
```bash
sudo apt-get update
sudo apt-get install golang-go -y
```

#### Centos/Fedora/AWS Linux v2
```bash
sudo yum install golang -y
```

### Install 
```bash
sudo su -
VERS=0.4.19
DIST="go-ipfs_v${VERS}_linux-amd64.tar.gz"
wget https://dist.ipfs.io/go-ipfs/v$VERS/$DIST
tar xvfz $DIST
rm *.gz
mv go-ipfs/ipfs /usr/local/bin/ipfs
exit
```

### Configure systemd
```bash
sudo su -
ipfs init
cat <<EOF > /lib/systemd/system/ipfs.service
[Unit]
Description=IPFS daemon
After=network.target
[Service]
ExecStart=/usr/local/bin/ipfs daemon
[Install]
WantedBy=multiuser.target
EOF

systemctl start ipfs
systemctl enable ipfs

exit
```

## Cluster

### IPFS-Cluster

[IPFS-Cluster Documentation](https://cluster.ipfs.io/documentation/)

### Kubernetes

[IPFS Helm Chart](https://github.com/helm/charts/tree/master/stable/ipfs)