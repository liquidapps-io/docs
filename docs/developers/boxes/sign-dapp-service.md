
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
* [`web3@1.0.0-beta.55`](http://npmjs.com/package/web3@1.0.0-beta.55)
* [`ganache-cli`](http://npmjs.com/package/ganache-cli)

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
  "stage": "WIP",
  "description": "IBC MultiSig Service",
  "commands": {
    "signtrx": {
      "blocking": true,
      "request": {
        "account": "name",
        "permission": "name",
        "client_code": "std::string",
        "payload_hash": "checksum256",
        "signature": "std::vector<char>"
      },
      "callback": {
        "account": "name",
        "permission": "name",
        "client_code": "std::string",
        "payload_hash": "checksum256",
        "signature": "std::vector<char>"
      },
      "signal": {
        "account": "name",
        "permission": "name",
        "client_code": "std::string",
        "payload_hash": "checksum256",
        "signature": "std::vector<char>"
      }
    },
    "sgcleanup": {
      "blocking": false,
      "request": {
        "account": "name",
        "permission": "name",
        "client_code": "std::string",
        "payload_hash": "checksum256",
        "signature": "std::vector<char>"
      },
      "callback": {
        "account": "name",
        "permission": "name",
        "client_code": "std::string",
        "payload_hash": "checksum256",
        "signature": "std::vector<char>"
      },
      "signal": {
        "account": "name",
        "permission": "name",
        "client_code": "std::string",
        "payload_hash": "checksum256",
        "signature": "std::vector<char>"
      }
    }
  }
}
```
## Tests 
* [sign.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/sign-dapp-service/test/sign.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/sign-dapp-service)