
oracle-dapp-service
====================






## Service Documentation
    [oracle](../../services/oracle/oracle-service.md)
## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
### npm packages
* [`node-fetch`](http://npmjs.com/package/node-fetch)
* [`tronweb`](http://npmjs.com/package/tronweb)
* [`ripple-lib`](http://npmjs.com/package/ripple-lib)
* [`bitcoin-core`](http://npmjs.com/package/bitcoin-core)

## Contracts
* [`oracleservice`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/oracle-dapp-service/contracts/eos/dappservices/_oracle_impl.hpp)

## Install
```bash
zeus unbox oracle-dapp-service
```










### Model Instances
#### [services/oracle.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/oracle-dapp-service/models/dapp-services/oracle.json)
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