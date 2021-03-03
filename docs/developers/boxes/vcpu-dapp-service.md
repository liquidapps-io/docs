
vcpu-dapp-service
====================






## Service Documentation
[VCPU](../../services/vcpu-service.md)


* [`dapp-services`](dapp-services.md)
### npm packages
* [`assemblyscript`](http://npmjs.com/package/assemblyscript)
* [`esm`](http://npmjs.com/package/esm)
* [`node-fetch`](http://npmjs.com/package/node-fetch)

## Contracts

## Install
```bash
zeus unbox vcpu-dapp-service
```










### Model Instances
#### [services/vcpu.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/vcpu-dapp-service/models/dapp-services/vcpu.json)
```json
{
  "name": "vcpu",
  "port": 13386,
  "alt": 26772,
  "prettyName": "VCPU",
  "description": "DSP Hosted Computation Service",
  "stage": "PoC",
  "version": "0.1",
  "contract": "vcpuservices",
  "generateStubs": true,
  "commands": {
    "vrun": {
      "blocking": true,
      "broadcast": true,
      "request": {
        "uri": "std::vector<char>",
        "payload": "std::vector<char>"
      },
      "callback": {
        "size": "uint32_t",
        "uri": "std::vector<char>",
        "data": "std::vector<char>"
      },
      "signal": {
        "size": "uint32_t",
        "uri": "std::vector<char>"
      }
    },
    "vrunclean": {
      "blocking": false,
      "request": {
        "uri": "std::vector<char>"
      },
      "callback": {
        "size": "uint32_t",
        "uri": "std::vector<char>"
      },
      "signal": {
        "size": "uint32_t",
        "uri": "std::vector<char>"
      }
    }
  }
}
```
## Tests 
* [vcpuconsumer.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/vcpu-dapp-service/test/vcpuconsumer.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/vcpu-dapp-service)