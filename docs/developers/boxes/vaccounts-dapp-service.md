
vaccounts-dapp-service
====================






## Service Documentation
[vaccounts](../../services/vaccounts/vaccounts-service.md)
## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
* [`ipfs-dapp-service`](ipfs-dapp-service.md)
### npm packages
* [`node-fetch`](http://npmjs.com/package/node-fetch)

## Contracts
* [`vaccountsservice`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/vaccounts-dapp-service/contracts/eos/dappservices/_vaccounts_impl.hpp)

## Install
```bash
zeus unbox vaccounts-dapp-service
```










### Model Instances
#### [services/vaccounts.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/vaccounts-dapp-service/models/dapp-services/vaccounts.json)
```json
{
  "name": "vaccounts",
  "port": 13129,
  "contract": "accountless1",
  "commands": {
    "vexec": {
      "blocking": false,
      "request": {},
      "callback": {
        "payload": "std::vector<char>",
        "sig": "eosio::signature",
        "pubkey": "eosio::public_key"
      },
      "signal": {
        "sig": "eosio::signature",
        "pubkey": "eosio::public_key"
      }
    }
  }
}
```
## Tests 
* [vaccountsconsumer.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/vaccounts-dapp-service/test/vaccountsconsumer.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/vaccounts-dapp-service)