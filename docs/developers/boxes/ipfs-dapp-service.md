
ipfs-dapp-service 
====================




## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
### npm packages
* `ipfs-api`
* `lru-cache`
* `dfuse-eoshttp-js`
* `node-fetch`
## Contracts
* `ipfsservice`
* `ipfsconsumer`
## Install
```bash
zeus unbox ipfs-dapp-service
```


## Zeus Command Extensions
* ```zeus get-table.row  --help```







### Model Instances
#### dapp-services/ipfs.json
```json
{
  "name": "ipfs",
  "port": 13115,
  "contract": "ipfsservice1",
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
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service)
