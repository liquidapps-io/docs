
dns-dapp-service
====================






## Service Documentation
[LiquidDNS](../../services/dns-service.md)
## Dependencies
### Boxes
* [`dapp-services`](dapp-services.md)
### npm packages
* [`dnsd`](http://npmjs.com/package/dnsd)

## Contracts
* [`dnsservice`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/dns-dapp-service/contracts/eos/dappservices/_dns_impl.hpp)

## Install
```bash
zeus unbox dns-dapp-service
```










### Model Instances
#### [services/dns.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/dns-dapp-service/models/dapp-services/dns.json)
```json
{
  "name": "dns",
  "port": 13284,
  "prettyName": "LiquidDNS",
  "description": "DSP Hosted DNS Service",
  "stage": "WIP",
  "contract": "dnsservices1",
  "generateStubs": true,
  "commands": {
    "dnsq": {
      "blocking": true,
      "request": {
        "payload": "std::vector<char>"
      },
      "callback": {
        "payload": "std::vector<char>"
      },
      "signal": {
        "payload": "std::vector<char>"
      }
    }
  }
}
```
## Tests 
* [dnsconsumer.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/dns-dapp-service/test/dnsconsumer.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/dns-dapp-service)