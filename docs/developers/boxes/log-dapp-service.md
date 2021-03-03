
log-dapp-service
====================






## Service Documentation
[LiquidLog](../../services/log-service.md)


* [`dapp-services`](dapp-services.md)



## Contracts

## Install
```bash
zeus unbox log-dapp-service
```










### Model Instances
#### [services/log.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/log-dapp-service/models/dapp-services/log.json)
```json
{
  "name": "log",
  "port": 13110,
  "alt": 26220,
  "contract": "logservices1",
  "prettyName": "LiquidLog",
  "stage": "Beta",
  "version": "0.9",
  "description": "Log Service",
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
## Tests 
* [logconsumer.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/log-dapp-service/test/logconsumer.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/log-dapp-service)