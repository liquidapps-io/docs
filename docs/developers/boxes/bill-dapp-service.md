
bill-dapp-service
====================






## Service Documentation
[LiquidBilling](../../services/bill-service.md)
## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)



## Contracts
* [`billservice`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/bill-dapp-service/contracts/eos/dappservices/_bill_impl.hpp)
## Install
```bash
zeus unbox bill-dapp-service
```










### Model Instances
#### [services/bill.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/bill-dapp-service/models/dapp-services/bill.json)
```json
{
  "name": "bill",
  "port": 13145,
  "contract": "liquidbillin",
  "prettyName": "LiquidBilling",
  "stage": "WIP",
  "version": "0.0",
  "description": "Transaction signing service for resource payment",
  "generateStubs": true,
  "commands": {
    "sdummy2": {
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
    }
  },
  "api": {
    "push_transaction": {},
    "sign": {}
  }
}
```
## Tests 
* [bill.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/bill-dapp-service/test/bill.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/bill-dapp-service)