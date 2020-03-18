EOSIO Node
==========

A non block / non full history node is required for the DSP API to interact with.  This node may be hosted with the rest of the DSP architecture or standalone.

## Hardware Requirements
## Prerequisites

- jq
- wget
- curl

## Get EOSIO binary

```bash
# nodeos versions 1.8+ and 2.0+ are supported
VERSION=2.0.4
```

### Ubuntu 18.04
```bash
FILENAME=eosio_$VERSION-1-ubuntu-18.04_amd64.deb
INSTALL_TOOL=apt
```

### Ubuntu 16.04
```bash
FILENAME=eosio_$VERSION-1-ubuntu-16.04_amd64.deb
INSTALL_TOOL=apt
```

### Fedora
```bash
FILENAME=eosio_$VERSION-1.fc27.x86_64.rpm
INSTALL_TOOL=yum
```

### Centos
```bash
FILENAME=eosio_$VERSION-1.el7.x86_64.rpm
INSTALL_TOOL=yum
```

## Install
```bash
wget https://github.com/EOSIO/eos/releases/download/v$VERSION/$FILENAME
sudo $INSTALL_TOOL install ./$FILENAME
```

## Prepare Directories
```bash
#cleanup
rm -rf $HOME/.local/share/eosio/nodeos || true

#create dirs
mkdir $HOME/.local/share/eosio/nodeos/data/blocks -p
mkdir $HOME/.local/share/eosio/nodeos/data/snapshots -p
mkdir $HOME/.local/share/eosio/nodeos/config -p
```

### Snapshots
If you would like an up to date snapshot, please visit: [snapshots.eosnation.io](https://snapshots.eosnation.io/) and find the latest snapshot for the chain you are using.  You will want to unpack the file and store it here with the following file name: `$HOME/.local/share/eosio/nodeos/data/snapshots/boot.bin`.  EOS Node tools many also be used for mainnet: [https://eosnode.tools/snapshots](https://eosnode.tools/snapshots)

### Kylin
```bash
URL="http://storage.googleapis.com/eos-kylin-snapshot/snapshot-2019-06-10-09(utc)-0312d3b9843e2efa6831806962d6c219d37200e0b897a0d9243bcab40b2b546b.bin"
P2P_FILE=https://raw.githubusercontent.com/cryptokylin/CryptoKylin-Testnet/master/fullnode/config/config.ini
GENESIS=https://raw.githubusercontent.com/cryptokylin/CryptoKylin-Testnet/master/genesis.json
CHAIN_STATE_SIZE=256000
wget $URL -O $HOME/.local/share/eosio/nodeos/data/snapshots/boot.bin
```  

### Jungle
You can find more Jungle peers here: [https://monitor.jungletestnet.io/#p2p](https://monitor.jungletestnet.io/#p2p)
```bash
export MONTH=01
export DAY=09
wget https://eosn.sfo2.digitaloceanspaces.com/snapshots/snapshot-2020-$MONTH-$DAY-15-jungle.bin.bz2
bzip2 -d ./snapshot-2020-01-09-15-jungle.bin.bz2
mv snapshot-2020-01-09-15-jungle.bin $HOME/.local/share/eosio/nodeos/data/snapshots/boot.bin
P2P_FILE=https://validate.eosnation.io/jungle/reports/config.txt
GENESIS=https://raw.githubusercontent.com/EOS-Jungle-Testnet/Node-Manual-Installation/master/genesis.json
CHAIN_STATE_SIZE=256000
```

### Mainnet
```bash
URL="https://s3.eu-central-1.wasabisys.com/eosnodetools/snapshots/snap_2019-12-15-13-00.tar.gz"
P2P_FILE=https://eosnodes.privex.io/?config=1
GENESIS=https://raw.githubusercontent.com/CryptoLions/EOS-MainNet/master/genesis.json
CHAIN_STATE_SIZE=16384
cd $HOME/.local/share/eosio/nodeos/data
wget $URL -O - | tar xvz
SNAPFILE=`ls snapshots/*.bin | head -n 1 | xargs -n 1 basename`
mv snapshots/$SNAPFILE snapshots/boot.bin
```

## Configuration

```bash
cd $HOME/.local/share/eosio/nodeos/config

# download genesis
wget $GENESIS
# config
cat <<EOF >> $HOME/.local/share/eosio/nodeos/config/config.ini
agent-name = "DSP"
http-server-address = 0.0.0.0:8888
p2p-listen-endpoint = 0.0.0.0:9876
blocks-dir = "blocks"
abi-serializer-max-time-ms = 3000
max-transaction-time = 150000
wasm-runtime = eos-vm
eos-vm-oc-enable = true
reversible-blocks-db-size-mb = 1024
contracts-console = true
p2p-max-nodes-per-host = 1
allowed-connection = any
max-clients = 100
sync-fetch-span = 500
connection-cleanup-period = 30
http-validate-host = false
access-control-allow-origin = *
access-control-allow-headers = *
access-control-allow-credentials = false
verbose-http-errors = true
http-threads=8
net-threads=8
trace-history-debug-mode = true
trace-history = true
plugin = eosio::producer_plugin
plugin = eosio::chain_plugin
plugin = eosio::chain_api_plugin
plugin = eosio::net_plugin
plugin = eosio::state_history_plugin
state-history-endpoint = 0.0.0.0:8887
chain-state-db-size-mb = $CHAIN_STATE_SIZE
EOF

curl $P2P_FILE > p2p-config.ini
cat p2p-config.ini | grep "p2p-peer-address" >> $HOME/.local/share/eosio/nodeos/config/config.ini
```

*Please note the following about some `config.ini` settings:*

- `wasm-runtime = wabt` must be used as the `wavm` engine has bugs
- `read-mode = head` (default is: `read-more = speculative` and does not need to be specified in the `config.ini`) must not be used to prevent duplicate `xwarmup` actions | [read more about read modes here](https://developers.eos.io/eosio-nodeos/docs/read-modes)

## Run 
First run (from snapshot)
```bash
nodeos --disable-replay-opts --snapshot $HOME/.local/share/eosio/nodeos/data/snapshots/boot.bin --delete-all-blocks
```
You will know that the node is fully synced once you see blocks being produced every half second at the head block.  You can match the block number you are seeing in the nodeos logs to what [bloks.io](https://bloks.io/) is indicating as the head block on the chain you are syncing (mainnet, Kylin etc). Once you have confirmed that it is synced press `CTRL+C` once, wait for the node to shutdown and proceed to the next step.

## systemd
```bash
export NODEOS_EXEC=`which nodeos`
export NODEOS_USER=$USER
sudo -E su - -p
cat <<EOF > /lib/systemd/system/nodeos.service
[Unit]
Description=nodeos
After=network.target
[Service]
User=$NODEOS_USER
ExecStart=$NODEOS_EXEC --disable-replay-opts
[Install]
WantedBy=multi-user.target
EOF

systemctl start nodeos
systemctl enable nodeos
exit
sleep 3
systemctl status nodeos
```

## Optimizations

- [atticlab - cpu performance presentation](https://github.com/atticlab/eos-bp-performance/blob/master/cpu_perf_presentation.pdf)
