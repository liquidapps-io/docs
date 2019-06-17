
history-dapp-service
====================






## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)



## Contracts
* [`historyservice`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/history-dapp-service/contracts/eos/dappservices/_history_impl.hpp)
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
#### [dapp-services/history.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/history-dapp-service/models/dapp-services/history.json)
```json
{
  "name": "history",
  "port": 13143,
  "contract": "historyservc",
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
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/history-dapp-service)