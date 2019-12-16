
ipfs-dapp-service
====================






## Service Documentation
[LiquidVRAM](../../services/ipfs-service.md)
## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`log-dapp-service`](log-dapp-service.md)
* [`seed-utils-cleanup`](seed-utils-cleanup.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
### npm packages
* [`ipfs-api`](http://npmjs.com/package/ipfs-api)
* [`lru-cache`](http://npmjs.com/package/lru-cache)
* [`dfuse-eoshttp-js`](http://npmjs.com/package/dfuse-eoshttp-js)
* [`node-fetch`](http://npmjs.com/package/node-fetch)

## Contracts
* [`ipfsservice`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service/contracts/eos/dappservices/_ipfs_impl.hpp)

## Install
```bash
zeus unbox ipfs-dapp-service
```



## Zeus Command Extensions
* ```zeus get-table.row  --help```





### Model Instances
#### [services/ipfs.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service/models/dapp-services/ipfs.json)
```json
{
  "name": "ipfs",
  "port": 13115,
  "contract": "ipfsservice1",
  "prettyName": "LiquidVRAM",
  "stage": "Stable",
  "description": "Virtual Memory Service",
  "version": "1.1",
  "commands": {
    "commit": {
      "blocking": false,
      "request": {
        "data": "std::vector<char>"
      },
      "callback": {
        "size": "uint32_t",
        "uri": "std::string"
      },
      "signal": {
        "size": "uint32_t",
        "uri": "std::string"
      }
    },
    "cleanup": {
      "blocking": false,
      "request": {
        "uri": "std::string"
      },
      "callback": {
        "size": "uint32_t",
        "uri": "std::string"
      },
      "signal": {
        "size": "uint32_t",
        "uri": "std::string"
      }
    },
    "warmup": {
      "blocking": true,
      "request": {
        "uri": "std::string"
      },
      "callback": {
        "size": "uint32_t",
        "uri": "std::string",
        "data": "std::vector<char>"
      },
      "signal": {
        "size": "uint32_t",
        "uri": "std::string"
      }
    }
  }
}
```
## Tests 
* [dappservices.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service/test/dappservices.spec.js)
* [ipfsconsumer.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service/test/ipfsconsumer.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service)