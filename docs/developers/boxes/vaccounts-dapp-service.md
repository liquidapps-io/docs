
vaccounts-dapp-service
====================






## Service Documentation
[LiquidAccounts](../../services/vaccounts-service.md)
## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
* [`ipfs-dapp-service`](ipfs-dapp-service.md)
* [`log-dapp-service`](log-dapp-service.md)
### npm packages
* [`node-fetch`](http://npmjs.com/package/node-fetch)
* [`eosjs-ecc`](http://npmjs.com/package/eosjs-ecc)

## Contracts
* [`vaccountsservice`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/vaccounts-dapp-service/contracts/eos/dappservices/_vaccounts_impl.hpp)

## Install
```bash
zeus unbox vaccounts-dapp-service
```



## Zeus Command Extensions
* ```zeus vaccounts  --help```
### Subcommands
* ```zeus vaccounts push-action --help```




### Model Instances
#### [services/vaccounts.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/vaccounts-dapp-service/models/dapp-services/vaccounts.json)
```json
{
  "name": "vaccounts",
  "port": 13129,
  "prettyName": "LiquidAccounts",
  "stage": "Beta",
  "version": "0.9",
  "description": "Allows interaction with contract without a native EOS Account",
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