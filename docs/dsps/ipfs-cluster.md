IPFS Cluster
============

## Standalone
https://dist.ipfs.io/#go-ipfs

### Install - Ubuntu
```bash
sudo su -
VERS=0.4.19
DIST="go-ipfs_v${VERS}_linux-amd64.tar.gz"
apt-get update
apt-get install golang-go -y
wget https://dist.ipfs.io/go-ipfs/v$VERS/$DIST
tar xvfz $DIST
rm *.gz
mv go-ipfs/ipfs /usr/local/bin/ipfs
exit
```

### Configure
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
exit
```

## Cluster

https://cluster.ipfs.io/documentation/