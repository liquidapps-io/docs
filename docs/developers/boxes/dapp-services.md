
dapp-services
====================









* [`seed-zeus-support`](seed-zeus-support.md)
* [`seed-microservices`](seed-microservices.md)
* [`core-extensions`](core-extensions.md)
* [`demux`](demux.md)
* [`dfuse`](dfuse.md)
* [`seed-eos`](seed-eos.md)
* [`events`](events.md)
* [`seed-models`](seed-models.md)
* [`mocha`](mocha.md)
* [`client-lib-base`](client-lib-base.md)
* [`ipfs-daemon`](ipfs-daemon.md)
### npm packages
* [`@dfuse/client`](http://npmjs.com/package/@dfuse/client)
* [`big-integer`](http://npmjs.com/package/big-integer)
* [`bytebuffer`](http://npmjs.com/package/bytebuffer)
* [`cors`](http://npmjs.com/package/cors)
* [`eosjs@20.0.0`](http://npmjs.com/package/eosjs@20.0.0)
* [`express`](http://npmjs.com/package/express)
* [`ipfs-api`](http://npmjs.com/package/ipfs-api)
* [`http-proxy`](http://npmjs.com/package/http-proxy)
* [`raw-body`](http://npmjs.com/package/raw-body)
* [`content-type`](http://npmjs.com/package/content-type)
* [`js-sha256`](http://npmjs.com/package/js-sha256)
* [`body-parser`](http://npmjs.com/package/body-parser)
* [`mkdirp`](http://npmjs.com/package/mkdirp)
* [`sequelize`](http://npmjs.com/package/sequelize)
* [`pg`](http://npmjs.com/package/pg)
* [`minipass@2.7.0`](http://npmjs.com/package/minipass@2.7.0)
* [`sqlite3`](http://npmjs.com/package/sqlite3)
* [`promise.allsettled`](http://npmjs.com/package/promise.allsettled)

## Contracts
* [`dappservices`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/dapp-services/contracts/eos/dappservices)
* [`allservices`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/dapp-services/contracts/eos/allservices)
## Install
```bash
zeus unbox dapp-services
```







## Models
### New Model Types
* dapp-services
* deploy-dapp-services
### Model Instances
#### [dapp-network/dappservice.request.1.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/dapp-services/models/captured-events/dappservice.request.1.json)
```json
{
  "eventType": "service_request",
  "webhook": "http://localhost:17624",
  "testOnly": true
}
```,#### [dapp-network/dappservice.request.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/dapp-services/models/captured-events/dappservice.request.json)
```json
{
  "eventType": "service_request",
  "webhook": "http://localhost:8812"
}
```,#### [dapp-network/dappservice.signal.1.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/dapp-services/models/captured-events/dappservice.signal.1.json)
```json
{
  "eventType": "service_signal",
  "webhook": "http://localhost:17624",
  "testOnly": true
}
```,#### [dapp-network/dappservice.signal.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/dapp-services/models/captured-events/dappservice.signal.json)
```json
{
  "eventType": "service_signal",
  "webhook": "http://localhost:8812"
}
```,#### [dapp-network/dappservice.usage.1.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/dapp-services/models/captured-events/dappservice.usage.1.json)
```json
{
  "eventType": "usage_report",
  "webhook": "http://localhost:17624",
  "testOnly": true
}
```,#### [dapp-network/dappservice.usage.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/dapp-services/models/captured-events/dappservice.usage.json)
```json
{
  "eventType": "usage_report",
  "webhook": "http://localhost:8812"
}
```

## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/dapp-network/dapp-services)