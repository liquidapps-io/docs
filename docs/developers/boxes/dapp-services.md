
dapp-services 
====================




## Dependencies
### Boxes
* [`seed`](seed.md)
* [`seed-microservices`](seed-microservices.md)
* [`core-extensions`](core-extensions.md)
* [`demux`](demux.md)
* [`seed-eos`](seed-eos.md)
* [`events`](events.md)
* [`seed-models`](seed-models.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
* [`mocha`](mocha.md)
### npm packages
* `big-integer`
* `bytebuffer`
* `cors`
* `eosjs@16.0.9`
* `express`
* `ipfs-api`
* `http-proxy`
* `raw-body`
* `content-type`
* `js-sha256`
* `body-parser`
* `mkdirp`
## Contracts
* `dappservices`
## Install
```bash
zeus unbox dapp-services
```


## Zeus Command Extensions

### Subcommands
* ```zeus compile dapp-services-eos --help```

* ```zeus dapp-service-provider claim --help```

* ```zeus register dapp-service-provider-package --help```

* ```zeus run dapp-services-node --help```

* ```zeus start-localenv 20-eos-local-dapp-services --help```

* ```zeus start-localenv 21-eos-local-services-all-dapp-services --help```


## Models
### New Model Types
* dapp-services
* deploy-dapp-services
### Model Instances
#### captured-events/dappservice.request.1.json
```json
{
  "eventType": "service_request",
  "webhook": "http://localhost:17624",
  "testOnly": true
}
```,#### captured-events/dappservice.request.json
```json
{
  "eventType": "service_request",
  "webhook": "http://localhost:8812"
}
```,#### captured-events/dappservice.signal.1.json
```json
{
  "eventType": "service_signal",
  "webhook": "http://localhost:17624",
  "testOnly": true
}
```,#### captured-events/dappservice.signal.json
```json
{
  "eventType": "service_signal",
  "webhook": "http://localhost:8812"
}
```,#### captured-events/dappservice.usage.1.json
```json
{
  "eventType": "usage_report",
  "webhook": "http://localhost:17624",
  "testOnly": true
}
```,#### captured-events/dappservice.usage.json
```json
{
  "eventType": "usage_report",
  "webhook": "http://localhost:8812"
}
```
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/dapp-services)
