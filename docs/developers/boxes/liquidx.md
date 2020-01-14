
liquidx
====================







## Dependencies
### Boxes
* [`local-sidechains`](local-sidechains.md)
* [`all-dapp-services`](all-dapp-services.md)



## Contracts
* [`dappservicex`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/dappservicex)
* [`liquidx`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/liquidx)
* [`ipfsxtest`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/ipfsxtest)
* [`orcxtest`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/orcxtest)
* [`readfnxtest`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/readfnxtest)
* [`vaccountsx`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/vaccountsx)
* [`cronxtest`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/cronxtest)
## Install
```bash
zeus unbox liquidx
```



## Zeus Command Extensions
* ```zeus create-liquidx-mapping  --help```
* ```zeus link-sidechain-dsp  --help```
### Subcommands
* ```zeus start-localenv 20-eos-local-sidechains-dapp-services --help```

* ```zeus start-localenv 21-eos-local-sidechains-services-all-dapp-services --help```




### Model Instances
#### [dapp-network/test1.cron.service.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/models/liquidx-mappings/test1.cron.service.json)
```json
{
  "sidechain_name": "test1",
  "mainnet_account": "cronservices",
  "chain_account": "cronservicex"
}
```,#### [dapp-network/test1.dappservices.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/models/liquidx-mappings/test1.dappservices.json)
```json
{
  "sidechain_name": "test1",
  "mainnet_account": "dappservices",
  "chain_account": "dappservicex"
}
```,#### [dapp-network/test1.ipfs.service.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/models/liquidx-mappings/test1.ipfs.service.json)
```json
{
  "sidechain_name": "test1",
  "mainnet_account": "ipfsservice1",
  "chain_account": "ipfsservice2"
}
```,#### [dapp-network/test1.log.service.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/models/liquidx-mappings/test1.log.service.json)
```json
{
  "sidechain_name": "test1",
  "mainnet_account": "logservices1",
  "chain_account": "logservices2"
}
```,#### [dapp-network/test1.oracle.service.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/models/liquidx-mappings/test1.oracle.service.json)
```json
{
  "sidechain_name": "test1",
  "mainnet_account": "oracleservic",
  "chain_account": "oracleservx1"
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
```,#### [dapp-network/test1.readfn.service.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/models/liquidx-mappings/test1.readfn.service.json)
```json
{
  "sidechain_name": "test1",
  "mainnet_account": "readfndspsvc",
  "chain_account": "readfndspsvx"
}
```,#### [dapp-network/test1.vaccounts.service.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/models/liquidx-mappings/test1.vaccounts.service.json)
```json
{
  "sidechain_name": "test1",
  "mainnet_account": "accountless1",
  "chain_account": "accountless2"
}
```
#### [dapp-network/test1.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/models/local-sidechains/test1.json)
```json
{
  "dsp_port": 12346,
  "nodeos_port": 2424,
  "nodeos_endpoint": "http://localhost:2424",
  "nodeos_host": "localhost",
  "nodeos_state_history_port": 12341,
  "nodeos_p2p_port": 12451,
  "demux_port": 1232,
  "name": "test1"
}
```
## Tests 
* [sidechain-cron.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/test/sidechain-cron.spec.js)
* [sidechain-ipfsconsumer.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/test/sidechain-ipfsconsumer.spec.js)
* [sidechain-oracle.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/test/sidechain-oracle.spec.js)
* [sidechain-readfn.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/test/sidechain-readfn.spec.js)
* [sidechain-vaccount.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/test/sidechain-vaccount.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx)