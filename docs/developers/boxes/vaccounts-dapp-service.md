
vaccounts-dapp-service
====================






## Service Documentation
[LiquidAccounts](../../services/vaccounts-service.md)


* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
* [`ipfs-dapp-service`](ipfs-dapp-service.md)
* [`log-dapp-service`](log-dapp-service.md)
### npm packages
* [`node-fetch`](http://npmjs.com/package/node-fetch)
* [`eosjs-ecc`](http://npmjs.com/package/eosjs-ecc)
* [`isomorphic-fetch`](http://npmjs.com/package/isomorphic-fetch)

## Contracts

* [`vaccountsremote`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/vaccounts-dapp-service/contracts/eos/vaccountsremote)
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
  "alt": 26258,
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