
history-dapp-service
====================






## Service Documentation
[LiquidArchive](../../services/history-service.md)
## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)




## Install
```bash
zeus unbox history-dapp-service
```



## Zeus Command Extensions
* ```zeus history  --help```
### Subcommands
* ```zeus history get --help```

* ```zeus history subscribe --help```




### Model Instances
#### [services/history.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/history-dapp-service/models/dapp-services/history.json)
```json
{
  "name": "history",
  "port": 13143,
  "contract": "historyservc",
  "prettyName": "LiquidArchive",
  "stage": "WIP",
  "version": "0.0",
  "description": "History API Provisioning",
  "commands": {
    "hststore": {
      "blocking": false,
      "request": {},
      "callback": {
        "size": "uint64_t"
      },
      "signal": {
        "size": "uint64_t"
      }
    },
    "hsthold": {
      "blocking": false,
      "request": {},
      "callback": {
        "size": "uint64_t"
      },
      "signal": {
        "size": "uint64_t"
      }
    },
    "hstserve": {
      "blocking": false,
      "request": {},
      "callback": {
        "size": "uint64_t"
      },
      "signal": {
        "size": "uint64_t"
      }
    },
    "hstreg": {
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
* [history.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/history-dapp-service/test/history.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/history-dapp-service)