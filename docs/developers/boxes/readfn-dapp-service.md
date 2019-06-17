
readfn-dapp-service
====================


undefined



## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)



## Contracts
* [`readfnservice`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/readfn-dapp-service/contracts/eos/dappservices)
* [`readfnconsumer`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/readfn-dapp-service/contracts/eos/readfnconsumer)
## Install
```bash
zeus unbox readfn-dapp-service
```



## Zeus Command Extensions
* ```zeus readfn  --help```





### Model Instances
#### [dapp-services/readfn.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/readfn-dapp-service/models/dapp-services/readfn.json)
```json
{
  "name": "readfn",
  "port": 13141,
  "contract": "readfndspsvc",
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
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/readfn-dapp-service)