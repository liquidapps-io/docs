
sign-dapp-service
====================






## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)



## Contracts
* [`signservice`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/sign-dapp-service/contracts/eos/dappservices/_sign_impl.hpp)
* [`signer`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/sign-dapp-service/contracts/eos/signer)
## Install
```bash
zeus unbox sign-dapp-service
```










### Model Instances
#### [dapp-services/sign.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/sign-dapp-service/models/dapp-services/sign.json)
```json
{
  "name": "sign",
  "port": 13128,
  "contract": "signfndspsvc",
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
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/sign-dapp-service)