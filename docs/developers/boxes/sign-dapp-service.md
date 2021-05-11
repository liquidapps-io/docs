
sign-dapp-service
====================






## Service Documentation
[LiquidLink](../../services/sign-service.md)


* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
### npm packages
* [`web3`](http://npmjs.com/package/web3)
* [`ganache-cli`](http://npmjs.com/package/ganache-cli)
* [`@truffle/contract`](http://npmjs.com/package/@truffle/contract)

## Contracts
* [`signer`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/sign-dapp-service/contracts/eos/signer)
## Install
```bash
zeus unbox sign-dapp-service
```










### Model Instances
#### [services/sign.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/sign-dapp-service/models/dapp-services/sign.json)
```json
{
  "name": "sign",
  "port": 13128,
  "alt": 26256,
  "contract": "signfndspsvc",
  "prettyName": "LiquidLink",
  "stage": "Beta",
  "version": "0.9",
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
        "account": "std::string",
        "sigs_required": "uint16_t"
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