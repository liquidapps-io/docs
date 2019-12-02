
oracle-dapp-service
====================






## Service Documentation
[LiquidHarmony](../../services/oracle-service.md)
## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`mocha`](mocha.md)
* [`seed-utils-cleanup`](seed-utils-cleanup.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
* [`oracle-web`](oracle-web.md)
* [`oracle-self-history`](oracle-self-history.md)
* [`oracle-random`](oracle-random.md)
* [`oracle-sister-chain`](oracle-sister-chain.md)
### npm packages
* [`node-fetch`](http://npmjs.com/package/node-fetch)

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
  "prettyName": "LiquidHarmony",
  "stage": "Beta",
  "version": "0.9",
  "description": "Web/IBC/XIBC/VCPU/SQL Services",
  "commands": {
    "geturi": {
      "blocking": true,
      "broadcast": true,
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