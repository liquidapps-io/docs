# EOS Node

```bash
# install nodeos
VERSION=1.7.0
VERSION_URL=https://github.com/EOSIO/eos/releases/download/v$VERSION/eosio_$VERSION-1-ubuntu-18.04_amd64.deb
wget $VERSION_URL
sudo apt install ./eosio_$VERSION-1-ubuntu-18.04_amd64.deb

#cleanup
rm -rf $HOME/.local/share/eosio/nodeos || true
mkdir $HOME/.local/share/eosio/nodeos/data/blocks -p
mkdir $HOME/.local/share/eosio/nodeos/data/snapshots -p
mkdir $HOME/.local/share/eosio/nodeos/config -p

# generic options
cat <<EOF > $HOME/.local/share/eosio/nodeos/config/config.ini
agent-name = "DSP"
p2p-server-address = addr:8888

http-server-address = 0.0.0.0:8888
p2p-listen-endpoint = 0.0.0.0:9876

blocks-dir = "blocks"
abi-serializer-max-time-ms = 3000
wasm-runtime = wabt
reversible-blocks-db-size-mb = 1024
contracts-console = true
p2p-max-nodes-per-host = 1
allowed-connection = any
max-clients = 100
network-version-match = 1 
sync-fetch-span = 500
connection-cleanup-period = 30
http-validate-host = false
access-control-allow-origin = *
access-control-allow-headers = *
access-control-allow-credentials = false
verbose-http-errors = true
http-threads=8
net-threads=8
plugin = eosio::producer_plugin
plugin = eosio::chain_plugin
plugin = eosio::chain_api_plugin
plugin = eosio::net_plugin
plugin = eosio::state_history_plugin

trace-history = true
state-history-endpoint = 0.0.0.0:8887
EOF
```

## Kylin
[Kylin EOS Node configuration](eos-node-kylin.html)

## Mainnet
[Mainnet EOS Node configuration](eos-node-mainnet.html)
