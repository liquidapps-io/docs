
auth-dapp-service
====================






## Service Documentation
[LiquidAuthenticator](../../services/auth-service.md)


* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)



## Contracts
* [`authenticator`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/auth-dapp-service/contracts/eos/authenticator)
## Install
```bash
zeus unbox auth-dapp-service
```










### Model Instances
#### [services/auth.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/auth-dapp-service/models/dapp-services/auth.json)
```json
{
  "name": "auth",
  "port": 13127,
  "contract": "authfndspsvc",
  "prettyName": "LiquidAuthenticator",
  "stage": "Alpha",
  "version": "0.5",
  "description": "Authentication of offchain APIs and services using EOSIO permissions and contract",
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
## Tests 
* [auth-client.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/auth-dapp-service/test/auth-client.spec.js)
* [authenticator.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/auth-dapp-service/test/authenticator.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/auth-dapp-service)