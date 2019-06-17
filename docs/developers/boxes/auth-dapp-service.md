
auth-dapp-service
====================


undefined



## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)



## Contracts
* [`authservice`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/auth-dapp-service/contracts/eos/dappservices)
* [`authenticator`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/auth-dapp-service/contracts/eos/authenticator)
## Install
```bash
zeus unbox auth-dapp-service
```










### Model Instances
#### [dapp-services/auth.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/auth-dapp-service/models/dapp-services/auth.json)
```json
{
  "name": "auth",
  "port": 13127,
  "contract": "authfndspsvc",
  "commands": {
    "authusage": {
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
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/auth-dapp-service)