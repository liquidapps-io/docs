IPFS
====

The InterPlanetary File System is a protocol and peer-to-peer network for storing and sharing data in a distributed file system. IPFS uses content-addressing to uniquely identify each file in a global namespace connecting all computing devices.  The DSPs utilize this as the storage layer to request and serve information to and from vRAM <> RAM as a caching solution.

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
ipfs config Addresses.API /ip4/0.0.0.0/tcp/5001
ipfs config Addresses.Gateway /ip4/0.0.0.0/tcp/8080
cat <<EOF > /lib/systemd/system/ipfs.service
[Unit]
Description=IPFS daemon
After=network.target
[Service]
ExecStart=/usr/local/bin/ipfs daemon
Restart=always
[Install]
WantedBy=multi-user.target
EOF

systemctl start ipfs
systemctl enable ipfs

exit
```

### Adding Peers

#### Bootstrap Peers [Documentation](https://docs.ipfs.io/guides/examples/bootstrap/)

Bootstrap peers default to IPFS nodes provided by the core development team.  They are scattered across the world.  Bootstrap peers are what your IPFS node will monitor for new IPFS nodes on the network, for this reason, it is important this is a trusted source.  To facilitate the process of connecting to other IPFS nodes that are EOS related, you may add our mainnet and kylin testnet IPFS nodes with the following command:

```sh
# kylin

ipfs bootstrap add /ip4/18.212.96.94/tcp/4001/ipfs/QmZ5gLTZwvfD5DkbbaFFX4YJCi7f4C5oQAgq8qpjL8S1ur
ipfs bootstrap add /ip4/18.212.76.109/tcp/4001/ipfs/QmcCX4b3EF3eXaDe5dgxTL9mXbyci4FwcJAjWqpub5vCXM

# mainnet

ipfs bootstrap add /ip4/35.170.64.183/tcp/4001/ipfs/QmZpyMnBJKwyPJNBUYVuCZEJuKQBEwM6qVHsSp179B3yao
```

#### Swarm Peers [Documentation](https://docs.ipfs.io/reference/api/cli/#ipfs-swarm-connect)

Swarm peers are what your IPFS node will look to first when requesting a file that is not stored locally.  To reduce the latency of requesting files, you may add our DSPs to your swarm.  You may also add other DSP IPFS nodes.

```sh
# kylin

ipfs swarm connect /ip4/18.212.96.94/tcp/4001/ipfs/QmZ5gLTZwvfD5DkbbaFFX4YJCi7f4C5oQAgq8qpjL8S1ur
ipfs swarm connect /ip4/18.212.76.109/tcp/4001/ipfs/QmcCX4b3EF3eXaDe5dgxTL9mXbyci4FwcJAjWqpub5vCXM

# mainnet

ipfs swarm connect /ip4/35.170.64.183/tcp/4001/ipfs/QmZpyMnBJKwyPJNBUYVuCZEJuKQBEwM6qVHsSp179B3yao
```

## Cluster

### IPFS-Cluster

[IPFS-Cluster Documentation](https://cluster.ipfs.io/documentation/)

### Boostrapping from an existing IPFS Cluster

[Documentation](https://cluster.ipfs.io/documentation/quickstart/#quickstart-starting-enlarging-and-shrinking-a-cluster)

IPFS is designed so that a node only stores files locally that are specifically requested.  The following is one way of populating a new IPFS node with all existing files from a pre-existing node.

To do so, first create a secret from `node0`, the original node, then share that secret with `node1`, the node you want to bootstrap from `node0`.  Then `node1` runs the bootstrap command specifying the cluster's address and setting the `CLUSTER_SECRET` as an env variable.

#### node0

* `export CLUSTER_SECRET=$(od  -vN 32 -An -tx1 /dev/urandom | tr -d ' \n')`
* `echo $CLUSTER_SECRET`
* `ipfs-cluster-service init`
* `ipfs-cluster-service daemon`

#### node1 (bootstrapping from node0)

* `export CLUSTER_SECRET=<copy from node0>`
* `ipfs-cluster-service init`
* `ipfs-cluster-service daemon --bootstrap /ip4/192.168.1.2/tcp/9096/ipfs/QmZjSoXUQgJ9tutP1rXjjNYwTrRM9QPhmD9GHVjbtgWxEn` // replace with what you see from running node0's daemon
* `ipfs-cluster-ctl peers ls` check your peers to see you've added node0 correctly

#### node0
* if you want to remove a peer after the bootstrapping is complete, the following command will do that and shut down the IPFS cluster
* `ipfs-cluster-ctl peers rm QmYFYwnFUkjFhJcSJJGN72wwedZnpQQ4aNpAtPZt8g5fCd`