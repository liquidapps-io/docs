
oracle-dapp-service
====================






## Service Documentation
[LiquidOracle](../../services/oracle-service.md)
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
  "prettyName": "LiquidOracle",
  "stage": "Beta",
  "description": "Web/IBC/XIBC Oracle Service",
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
## Tests 
* [oracleconsumer.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/oracle-dapp-service/test/oracleconsumer.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/oracle-dapp-service)