
vaccounts-dapp-service 
====================




## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
* [`ipfs-dapp-service`](ipfs-dapp-service.md)
### npm packages
* `node-fetch`
## Contracts
* `vaccountsservice`
* `vaccountsconsumer`
## Install
```bash
zeus unbox vaccounts-dapp-service
```










### Model Instances
#### dapp-services/vaccounts.json
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
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/vaccounts-dapp-service)
