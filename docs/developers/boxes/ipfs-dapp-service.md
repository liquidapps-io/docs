
ipfs-dapp-service
====================






## Service Documentation
[LiquidVRAM](../../services/ipfs-service.md)


* [`dapp-services`](dapp-services.md)
* [`log-dapp-service`](log-dapp-service.md)
* [`seed-utils-cleanup`](seed-utils-cleanup.md)
* [`mocha`](mocha.md)
### npm packages
* [`ipfs-api`](http://npmjs.com/package/ipfs-api)
* [`bignumber.js`](http://npmjs.com/package/bignumber.js)
* [`lru-cache`](http://npmjs.com/package/lru-cache)
* [`dfuse-eoshttp-js`](http://npmjs.com/package/dfuse-eoshttp-js)
* [`node-fetch`](http://npmjs.com/package/node-fetch)

## Contracts

* [`oldipfscons`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service/contracts/eos/oldipfscons)
## Install
```bash
zeus unbox ipfs-dapp-service
```










### Model Instances
#### [services/ipfs.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service/models/dapp-services/ipfs.json)
```json
{
  "name": "ipfs",
  "port": 13115,
  "contract": "ipfsservice1",
  "prettyName": "LiquidVRAM",
  "stage": "Stable",
  "description": "Virtual Memory Service",
  "version": "1.5",
  "commands": {
    "commit": {
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
    },
    "cleanup": {
      "blocking": false,
      "request": {
        "uri": "std::string"
      },
      "callback": {
        "size": "uint32_t",
        "uri": "std::string"
      },
      "signal": {
        "size": "uint32_t",
        "uri": "std::string"
      }
    },
    "warmup": {
      "blocking": true,
      "request": {
        "uri": "std::string"
      },
      "callback": {
        "size": "uint32_t",
        "uri": "std::string",
        "data": "std::vector<char>"
      },
      "signal": {
        "size": "uint32_t",
        "uri": "std::string"
      }
    },
    "warmupcode": {
      "blocking": true,
      "request": {
        "uri": "std::string",
        "code": "name"
      },
      "callback": {
        "size": "uint32_t",
        "uri": "std::string",
        "data": "std::vector<char>"
      },
      "signal": {
        "size": "uint32_t",
        "uri": "std::string"
      }
    },
    "warmuprow": {
      "blocking": true,
      "request": {
        "uri": "std::string",
        "code": "name",
        "table": "name",
        "scope": "uint64_t",
        "index_position": "uint8_t",
        "key": "checksum256",
        "keysize": "uint8_t"
      },
      "callback": {
        "size": "uint32_t",
        "uris": "vector<std::string>",
        "data": "vector<vector<char>>"
      },
      "signal": {
        "size": "uint32_t",
        "uris": "vector<std::string>"
      }
    },
    "warmupchain": {
      "blocking": true,
      "request": {
        "shard": "uint32_t",
        "code": "name",
        "table": "name",
        "chain": "name",
        "scope": "uint64_t",
        "index_position": "uint8_t",
        "key": "checksum256",
        "keysize": "uint8_t"
      },
      "callback": {
        "shard": "uint32_t",
        "code": "name",
        "table": "name",
        "chain": "name",
        "size": "uint32_t",
        "uris": "vector<std::string>",
        "data": "vector<vector<char>>"
      },
      "signal": {
        "shard": "uint32_t",
        "code": "name",
        "table": "name",
        "chain": "name",
        "size": "uint32_t",
        "uris": "vector<std::string>"
      }
    },
    "cleanuprow": {
      "blocking": false,
      "request": {
        "uris": "vector<string>"
      },
      "callback": {
        "size": "uint32_t",
        "uris": "vector<string>"
      },
      "signal": {
        "size": "uint32_t",
        "uris": "vector<string>"
      }
    },
    "cleanchain": {
      "blocking": false,
      "request": {
        "uris": "vector<string>"
      },
      "callback": {
        "shard": "uint32_t",
        "code": "name",
        "table": "name",
        "chain": "name",
        "size": "uint32_t",
        "uris": "vector<string>"
      },
      "signal": {
        "shard": "uint32_t",
        "code": "name",
        "table": "name",
        "chain": "name",
        "size": "uint32_t",
        "uris": "vector<string>"
      }
    }
  }
}
```
## Tests 
* [dappservices.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service/test/dappservices.spec.js)
* [ipfsconsumer.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service/test/ipfsconsumer.spec.js)
* [oldipfscons.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service/test/oldipfscons.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service)