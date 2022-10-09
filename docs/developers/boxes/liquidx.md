
liquidx
====================









* [`eosio-chains`](eosio-chains.md)
* [`all-dapp-services`](all-dapp-services.md)



## Contracts
* [`dappservicex`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/dappservicex)
* [`liquidx`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/liquidx)
* [`ipfsxtest`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/ipfsxtest)
* [`orcxtest`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/orcxtest)
* [`readfnxtest`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/readfnxtest)
* [`vaccountsx`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/vaccountsx)
* [`vaccntxrem`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/vaccntxrem)
* [`vaccntxremx`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/vaccntxremx)
* [`cronxtest`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/cronxtest)
* [`storagextest`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/storagextest)
* [`signxtest`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/signxtest)
## Install
```bash
zeus unbox liquidx
```










### Model Instances
#### [dapp-network/test1.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/models/eosio-chains/test1.json)
```json
{
  "dsp_port": 13016,
  "webhook_dapp_port": 8813,
  "nodeos_host": "localhost",
  "nodeos_port": 2424,
  "secured": false,
  "nodeos_state_history_port": 12341,
  "firehose_grpc_address": "localhost",
  "firehose_grpc_secured": false,
  "firehose_grpc_port": 13036,
  "nodeos_p2p_port": 12451,
  "nodeos_endpoint": "http://localhost:2424",
  "demux_port": 1232,
  "name": "test1",
  "local": true
}
```
#### [dapp-network/test1.dappservices.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/models/liquidx-mappings/test1.dappservices.json)
```json
{
  "sidechain_name": "test1",
  "mainnet_account": "dappservices",
  "chain_account": "dappservicex"
}
```,#### [dapp-network/test1.provider1.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/models/liquidx-mappings/test1.provider1.json)
```json
{
  "sidechain_name": "test1",
  "mainnet_account": "pprovider1",
  "chain_account": "xprovider1"
}
```,#### [dapp-network/test1.provider2.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/models/liquidx-mappings/test1.provider2.json)
```json
{
  "sidechain_name": "test1",
  "mainnet_account": "pprovider2",
  "chain_account": "xprovider2"
}
```
## Tests 
* [sidechain-cron.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/test/sidechain-cron.spec.js)
* [sidechain-ipfsconsumer.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/test/sidechain-ipfsconsumer.spec.js)
* [sidechain-oracle.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/test/sidechain-oracle.spec.js)
* [sidechain-readfn.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/test/sidechain-readfn.spec.js)
* [sidechain-sign.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/test/sidechain-sign.spec.js)
* [sidechain-storage.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/test/sidechain-storage.spec.js)
* [sidechain-vaccount.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/test/sidechain-vaccount.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx)