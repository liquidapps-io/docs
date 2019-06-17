
cron-dapp-service 
====================




## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
### npm packages
* `node-fetch`
## Contracts
* `cronservice`
* `cronconsumer`
## Install
```bash
zeus unbox cron-dapp-service
```










### Model Instances
#### dapp-services
##### cron
```json
{
  "name": "cron",
  "port": 13131,
  "contract": "cronservices",
  "commands": {
    "schedule": {
      "blocking": false,
      "request": {
        "timer": "name",
        "payload": "std::vector<char>",
        "seconds": "uint32_t"
      },
      "callback": {
        "timer": "name",
        "payload": "std::vector<char>",
        "seconds": "uint32_t"
      },
      "signal": {
        "timer": "name",
        "seconds": "uint32_t"
      }
    }
  }
}
```


## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/cron-dapp-service)
