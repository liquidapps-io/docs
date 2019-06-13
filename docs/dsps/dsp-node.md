DSP Node
========

## Prerequisites
### Linux
```bash
sudo su -
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
export NVM_DIR="${XDG_CONFIG_HOME/:-$HOME/.}nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
nvm install 10
nvm use 10
exit
```

#### Ubuntu/Debian
```bash
sudo apt install -y make cmake build-essential python
```

#### Centos/Fedora/AWS Linux:
```bash
sudo yum install -y make cmake3 python
```

## Install
```bash
sudo su -
nvm use 10
npm install -g pm2
npm install -g @liquidapps/dsp --unsafe-perm=true
exit
```

## Configure Settings
```bash
sudo su -
mkdir ~/.dsp
cp $(readlink -f `which setup-dsp` | xargs dirname)/sample-config.toml ~/.dsp/config.toml
nano ~/.dsp/config.toml
# edit based on config below
exit
```

### sample.config.toml:
```bash
[dsp]
# enter account
account = "<DSP ACCOUNT>"
# enter account private key
private_key = "<DSP PRIVATE KEY>"
port = 3115
# configure available services: 
services_enabled = "ipfs,log,vaccounts,oracle,cron,readfn"

[nodeos]
host = "localhost"
port = 8888
secured = false

# mainnet:
chainid = "aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906"
# kylin: "5fff1dae8dc8e2fc4d5b23b2c7665c97f9e9d8edf2b6485a86ba311c25639191"

# if using zmq_plugin: 
zmq_port = 5557
# if using state_history_plugin: 
websocket_port = 8887

[demux]
backend = "state_history_plugin"
# zmq: "zmq_plugin" only if using nodeos with eosrio's version of the ZMQ plugin: https://github.com/eosrio/eos_zmq_plugin
socket_mode = "sub"

[ipfs]
protocol = "http"
host = "localhost"
port = 5001
```

## Launch DSP Services
```bash
sudo su -
setup-dsp
systemctl stop dsp
systemctl start dsp
systemctl enable dsp
exit
```

## Check logs
```bash
sudo pm2 logs
```

### Output sample:
```
/root/.pm2/logs/readfn-dapp-service-node-error.log last 15 lines:
/root/.pm2/logs/dapp-services-node-out.log last 15 lines:
0|dapp-ser | 2019-06-03T00:46:49: services listening on port 3115!
0|dapp-ser | 2019-06-03T00:46:49: service node webhook listening on port 8812!

/root/.pm2/logs/demux-out.log last 15 lines:
1|demux    | 2019-06-05T14:41:12: count 1

/root/.pm2/logs/ipfs-dapp-service-node-out.log last 15 lines:
2|ipfs-dap | 2019-06-04T19:03:04: commited to: ipfs://zb2rhXKc8zSVppFhKm8pHLBuyGb7vPeCnpZqcmjFnDLA9LLBb

/root/.pm2/logs/log-dapp-service-node-out.log last 15 lines:
3|log-dapp | 2019-06-03T00:46:49: log listening on port 13110!
3|log-dapp | 2019-06-03T00:46:52: LOG SVC NODE 2019-06-03T00:46:52.413Z INFO  index.js:global:0             Started Service

/root/.pm2/logs/vaccounts-dapp-service-node-out.log last 15 lines:
4|vaccount | 2019-06-03T00:46:50: vaccounts listening on port 13129!

/root/.pm2/logs/oracle-dapp-service-node-out.log last 15 lines:
5|oracle-d | 2019-06-03T00:46:50: oracle listening on port 13112!

/root/.pm2/logs/cron-dapp-service-node-out.log last 15 lines:
6|cron-dap | 2019-06-03T00:46:50: cron listening on port 13131!

/root/.pm2/logs/readfn-dapp-service-node-out.log last 15 lines:
7|readfn-d | 2019-06-03T00:46:50: readfn listening on port 13141!

```