
storage-dapp-service
====================






## Service Documentation
[LiquidStorage](../../services/storage-service.md)
## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
* [`seed-utils`](seed-utils.md)
* [`mocha`](mocha.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
* [`auth-dapp-service`](auth-dapp-service.md)
* [`ipfs-dapp-service`](ipfs-dapp-service.md)
* [`vaccounts-dapp-service`](vaccounts-dapp-service.md)
### npm packages
* [`tar-stream`](http://npmjs.com/package/tar-stream)
* [`stream-buffers`](http://npmjs.com/package/stream-buffers)

## Contracts

## Install
```bash
zeus unbox storage-dapp-service
```



## Zeus Command Extensions
* ```zeus storage  --help```
### Subcommands
* ```zeus storage unpin --help```

* ```zeus storage upload --help```




### Model Instances
#### [services/storage.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/storage-dapp-service/models/dapp-services/storage.json)
```json
{
  "name": "storage",
  "port": 13142,
  "contract": "liquidstorag",
  "prettyName": "LiquidStorage",
  "stage": "Beta",
  "version": "0.9",
  "description": "Distributed storage and hosting",
  "generateStubs": true,
  "commands": {
    "sdummy": {
      "blocking": false,
      "request": {
        "data": "std::vector<char>"
      },
      "callback": {
        "size": "uint32_t",
        "uri": "std::string"
      },
      "signal": {
        "size": "uint32_t",
        "uri": "std::string"
      }
    }
  },
  "api": {
    "upload_public": {
      "authentication": {
        "type": "payer",
        "contract": "authentikeos",
        "permission": "active"
      }
    },
    "upload_public_vaccount": {},
    "upload_private": {
      "authentication": {
        "type": "payer",
        "contract": "authentikeos",
        "permission": "active"
      }
    },
    "decrypt": {
      "authentication": {
        "type": "custom",
        "contract": "authentikeos"
      }
    },
    "unpin": {
      "authentication": {
        "type": "payer",
        "contract": "authentikeos",
        "permission": "active"
      }
    },
    "get_uri": {}
  }
}
```
## Tests 
* [storage.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/storage-dapp-service/test/storage.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/storage-dapp-service)