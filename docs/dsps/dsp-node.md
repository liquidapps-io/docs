DSP Node
========

## Prerequisites

- git

### Linux
```bash
sudo su -
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
export NVM_DIR="${XDG_CONFIG_HOME/:-$HOME/.}nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
# latest is 10.17.0 which has issues
nvm install 10.16.3
nvm use 10.16.3
exit
```

#### Ubuntu/Debian
```bash
sudo apt install -y make cmake build-essential python npm git node-typescript
```

#### Centos/Fedora/AWS Linux:
```bash
sudo yum install -y make cmake3 python
```

## Install
```bash
sudo su -
npm install -g pm2
npm install -g @liquidapps/dsp --unsafe-perm=true
exit
```

## Configure Settings
**Any changes to the `config.toml` file will require `setup-dsp` to be run again.**  Link to [`sample-config.toml`](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/dapp-network/dapp-services-deploy/sample-config.toml)

```bash
sudo su -
mkdir ~/.dsp
cp $(readlink -f `which setup-dsp` | xargs dirname)/sample-config.toml ~/.dsp/config.toml
nano ~/.dsp/config.toml
exit
```

## Launch DSP Services
```bash
sudo su -
cd $(readlink -f `which setup-dsp` | xargs dirname)
tsc zeus_boxes/dfuse/
setup-dsp
exit
```

## Logs
There are several log files when it comes to the DAPP Service Providers.  The `logs` folder can be found in the directory the `dsp` software was installed in.  Each service has its own log file, for example: `DSP_NAME_HERE-ipfs-dapp-service-node-2020-03-03.log`.  The service file will log the service related information.  

There is also a demux and dapp service node log `DSP_NAME_HERE-demux-2020-03-03.log`, `DSP_NAME_HERE-dapp-services-node-2020-03-03.log`.  The demux log tracks the interaction with the state history node, listening for relevant information for the DSP to act upon.  The dapp service node log is the first point of contact when sending a transaction to the DSP, this is the gateway that fields requests to the correct service files, to the nodeos RPC API in the case of say pushing a transaction or fetching a table row, or using the `/v1/dsp/version` endpoint for returning the current version of the DSP software on the node.

Finally for each chain a DSP supports with LiquidX there is an additional dapp service node and demux log `DSP_NAME_HERE-CHAIN_NAME_HERE-dapp-services-node-2020-03-03.log` `DSP_NAME_HERE-CHAIN_NAME_HERE-demux-2020-03-03.log`.

```bash
sudo su -
cd $(readlink -f `which setup-dsp` | xargs dirname)
cd logs
exit
```

## Additional Logs
`pm2 logs` can be used to ensure that there are no issues outside of the logging statements used.  For example, if a javascript file was improperly formatted, that error may show up in `pm2 logs`.

```bash
sudo su -
pm2 logs
exit
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

### All logs may be monitored with this script:

```bash
#! /bin/bash

tail -f /root/.pm2/logs/*log* ~/.nvm/versions/node/$(node -v)/lib/node_modules/@liquidapps/dsp/zeus_boxes/dapp-services-deploy/logs/*log*
```