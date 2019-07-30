
sign-dapp-service
====================






## Service Documentation
[LiquidLink](../../services/sign-service.md)
## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
### npm packages
* [`web3@1.0.0-beta.36`](http://npmjs.com/package/web3@1.0.0-beta.36)
* [`ganache-cli`](http://npmjs.com/package/ganache-cli)
* [`truffle-contract`](http://npmjs.com/package/truffle-contract)
* [`ethereumjs-tx`](http://npmjs.com/package/ethereumjs-tx)

## Contracts
* [`signservice`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/sign-dapp-service/contracts/eos/dappservices/_sign_impl.hpp)
* [`signer`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/sign-dapp-service/contracts/eos/signer)
## Install
```bash
zeus unbox sign-dapp-service
```



## Zeus Command Extensions

### Subcommands
* ```zeus start-localenv 03-ganache-cli --help```




### Model Instances
#### [services/sign.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/sign-dapp-service/models/dapp-services/sign.json)
```json
{
  "name": "sign",
  "port": 13128,
  "contract": "signfndspsvc",
  "prettyName": "LiquidLink",
  "stage": "Alpha",
  "description": "IBC MultiSig Service",
  "commands": {
    "signtrx": {
      "blocking": false,
      "request": {
        "id": "std::string",
        "destination": "std::string",
        "trx_data": "std::string",
        "chain": "std::string",
        "chain_type": "std::string",
        "account": "std::string"
      },
      "callback": {
        "id": "std::string",
        "destination": "std::string",
        "trx_data": "std::string",
        "chain": "std::string",
        "chain_type": "std::string",
        "account": "std::string",
        "trx_id": "std::string"
      },
      "signal": {
        "id": "std::string",
        "destination": "std::string",
        "trx_data": "std::string",
        "chain": "std::string",
        "chain_type": "std::string",
        "account": "std::string"
      }
    },
    "sgcleanup": {
      "blocking": false,
      "request": {
        "id": "uint64_t"
      },
      "callback": {
        "id": "uint64_t"
      },
      "signal": {
        "id": "uint64_t"
      }
    }
  }
}
```
## Tests 
* [sign.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/sign-dapp-service/test/sign.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/sign-dapp-service)