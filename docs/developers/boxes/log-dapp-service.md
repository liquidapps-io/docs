
log-dapp-service 
====================




## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)


## Contracts
* `logservice`
* `logconsumer`
## Install
```bash
zeus unbox log-dapp-service
```










### Model Instances
#### dapp-services/log.json
```json
{
  "name": "log",
  "port": 13110,
  "contract": "logservices1",
  "commands": {
    "logevent": {
      "blocking": false,
      "request": {
        "time": "uint64_t",
        "level": "std::string",
        "filename": "std::string",
        "line": "std::string",
        "func": "std::string",
        "message": "std::string"
      },
      "callback": {
        "size": "uint64_t",
        "reciept": "std::string"
      },
      "signal": {
        "size": "uint64_t"
      }
    },
    "logclear": {
      "blocking": false,
      "request": {
        "level": "std::string"
      },
      "callback": {
        "size": "uint64_t",
        "reciept": "std::string"
      },
      "signal": {
        "size": "uint64_t"
      }
    }
  }
}
```
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/log-dapp-service)
