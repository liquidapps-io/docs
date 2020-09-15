
kms-dapp-service
====================






## Service Documentation
[LiquidKMS](../../services/kms-service.md)


* [`dapp-services`](dapp-services.md)




## Install
```bash
zeus unbox kms-dapp-service
```










### Model Instances
#### [services/kms.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/kms-dapp-service/models/dapp-services/kms.json)
```json
{
  "name": "kms",
  "port": 13214,
  "prettyName": "LiquidKMS",
  "description": "Key Management Service",
  "stage": "WIP",
  "version": "0.0",
  "contract": "kmsservices1",
  "generateStubs": true,
  "commands": {
    "sdummy3": {
      "blocking": false,
      "request": {
        "data": "std::vector<char>"
      },
      "callback": {
        "size": "uint32_t",
        "uri": "std::string"
      },
      "signal": {
        "size": "uint32_t",
        "uri": "std::string"
      }
    }
  },
  "api": {
    "create_key": {
      "authentication": {
        "type": "payer",
        "contract": "authentikeos"
      }
    }
  }
}
```
## Tests 
* [kmsconsumer.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/kms-dapp-service/test/kmsconsumer.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/kms-dapp-service)