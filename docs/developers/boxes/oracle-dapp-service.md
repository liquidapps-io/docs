
oracle-dapp-service 
====================




## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
### npm packages
* `node-fetch`
* `tronweb`
* `ripple-lib`
* `bitcoin-core`
## Contracts
* `oracleservice`
* `oracleconsumer`
## Install
```bash
zeus unbox oracle-dapp-service
```










### Model Instances
#### dapp-services/oracle.json
```json
{
  "name": "oracle",
  "port": 13112,
  "contract": "oracleservic",
  "commands": {
    "geturi": {
      "blocking": true,
      "request": {
        "uri": "std::vector<char>"
      },
      "callback": {
        "size": "uint32_t",
        "uri": "std::vector<char>",
        "data": "std::vector<char>"
      },
      "signal": {
        "size": "uint32_t",
        "uri": "std::vector<char>"
      }
    },
    "orcclean": {
      "blocking": false,
      "request": {
        "uri": "std::vector<char>"
      },
      "callback": {
        "size": "uint32_t",
        "uri": "std::vector<char>"
      },
      "signal": {
        "size": "uint32_t",
        "uri": "std::vector<char>"
      }
    }
  }
}
```
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/oracle-dapp-service)
