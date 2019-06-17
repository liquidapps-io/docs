
cron-dapp-service
====================


undefined



## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
### npm packages
* [`node-fetch`](http://npmjs.com/package/node-fetch)

## Contracts
* [`cronservice`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/cron-dapp-service/contracts/eos/dappservices/_cron_impl.hpp)
* [`cronconsumer`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/cron-dapp-service/contracts/eos/cronconsumer)
## Install
```bash
zeus unbox cron-dapp-service
```










### Model Instances
#### [dapp-services/cron.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/cron-dapp-service/models/dapp-services/cron.json)
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