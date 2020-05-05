
cron-dapp-service
====================






## Service Documentation
[LiquidScheduler](../../services/cron-service.md)


* [`dapp-services`](dapp-services.md)
* [`mocha`](mocha.md)
### npm packages
* [`node-fetch`](http://npmjs.com/package/node-fetch)

## Contracts

## Install
```bash
zeus unbox cron-dapp-service
```










### Model Instances
#### [services/cron.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/cron-dapp-service/models/dapp-services/cron.json)
```json
{
  "name": "cron",
  "port": 13131,
  "contract": "cronservices",
  "prettyName": "LiquidScheduler",
  "stage": "beta",
  "version": "0.9",
  "description": "Scheduled Transactions",
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
## Tests 
* [cronconsumer.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/cron-dapp-service/test/cronconsumer.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/cron-dapp-service)