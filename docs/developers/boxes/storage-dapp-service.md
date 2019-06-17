
storage-dapp-service 
====================




## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)


## Contracts
* [`storageservice`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/storage-dapp-service/contracts/eos/storageservice)
## Install
```bash
zeus unbox storage-dapp-service
```


## Zeus Command Extensions
* ```zeus storage  --help```

### Subcommands
* ```zeus storage get --help```

* ```zeus storage migrate-provider --help```

* ```zeus storage put --help```





### Model Instances
#### [dapp-services/storage.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/storage-dapp-service/models/dapp-services/storage.json)
```json
{
  "name": "storage",
  "port": 13142,
  "contract": "liquidstorag",
  "commands": {
    "strstore": {
      "blocking": false,
      "request": {},
      "callback": {
        "size": "uint64_t"
      },
      "signal": {
        "size": "uint64_t"
      }
    },
    "strhold": {
      "blocking": false,
      "request": {},
      "callback": {
        "size": "uint64_t"
      },
      "signal": {
        "size": "uint64_t"
      }
    },
    "strserve": {
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
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/storage-dapp-service)
