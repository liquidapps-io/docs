
readfn-dapp-service
====================






## Service Documentation
[LiquidLens](../../services/readfn-service.md)


* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)



## Contracts

## Install
```bash
zeus unbox readfn-dapp-service
```










### Model Instances
#### [services/readfn.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/readfn-dapp-service/models/dapp-services/readfn.json)
```json
{
  "name": "readfn",
  "port": 13141,
  "contract": "readfndspsvc",
  "prettyName": "LiquidLens",
  "stage": "Alpha",
  "version": "0.9",
  "description": "Read Functions Service",
  "commands": {
    "rfnuse": {
      "blocking": false,
      "request": {},
      "callback": {
        "size": "uint64_t"
      },
      "signal": {
        "size": "uint64_t"
      }
    }
  }
}
```
## Tests 
* [readfnconsumer.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/readfn-dapp-service/test/readfnconsumer.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/readfn-dapp-service)