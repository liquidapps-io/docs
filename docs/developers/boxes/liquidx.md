
liquidx
====================







## Dependencies
### Boxes
* [`local-sidechains`](local-sidechains.md)
* [`ipfs-dapp-service`](ipfs-dapp-service.md)



## Contracts
* [`dappservicex`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/dappservicex)
* [`liquidx`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/contracts/eos/liquidx)
## Install
```bash
zeus unbox liquidx
```



## Zeus Command Extensions

### Subcommands
* ```zeus start-localenv 20-eos-local-sidechains-dapp-services --help```

* ```zeus start-localenv 21-eos-local-sidechains-services-all-dapp-services --help```




### Model Instances
#### [dapp-network/test1.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/models/local-sidechains/test1.json)
```json
{
  "dsp_port": 12346,
  "nodeos_port": 42345,
  "nodeos_endpoint": "http://localhost:42345",
  "nodeos_host": "localhost",
  "nodeos_state_history_port": 12341,
  "nodeos_p2p_port": 12451,
  "demux_port": 1232,
  "name": "test1"
}
```
## Tests 
* [sidechain-ipfsconsumer.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx/test/sidechain-ipfsconsumer.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/liquidx)