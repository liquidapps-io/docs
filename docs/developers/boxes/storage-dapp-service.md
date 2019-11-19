
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
### npm packages
* [`tar-stream`](http://npmjs.com/package/tar-stream)
* [`stream-buffers`](http://npmjs.com/package/stream-buffers)

## Contracts
* [`storageservice`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/storage-dapp-service/contracts/eos/dappservices/_storage_impl.hpp)
## Install
```bash
zeus unbox storage-dapp-service
```










### Model Instances
#### [services/storage.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/storage-dapp-service/models/dapp-services/storage.json)
```json
{
  "name": "storage",
  "port": 13142,
  "contract": "liquidstorag",
  "prettyName": "LiquidStorage",
  "stage": "Alpha",
  "version": "0.5",
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
        "contract": "authenticato",
        "permission": "active"
      }
    },
    "upload_private": {
      "authentication": {
        "type": "payer",
        "contract": "authenticato",
        "permission": "active"
      }
    },
    "decrypt": {
      "authentication": {
        "type": "custom",
        "contract": "authenticato"
      }
    },
    "unpin": {
      "authentication": {
        "type": "payer",
        "contract": "authenticato",
        "permission": "active"
      }
    },
    "ipfshttpgw": {}
  }
}
```
## Tests 
* [storage.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/storage-dapp-service/test/storage.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/storage-dapp-service)