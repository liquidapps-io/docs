
portfolio
====================







## Dependencies
### Boxes
* [`hooks-npm`](hooks-npm.md)
* [`mocha`](mocha.md)
* [`seed-eos`](seed-eos.md)
* [`seed-migrations`](seed-migrations.md)
* [`ipfs-dapp-service`](ipfs-dapp-service.md)
* [`vaccounts-dapp-service`](vaccounts-dapp-service.md)
* [`oracle-dapp-service`](oracle-dapp-service.md)
* [`cron-dapp-service`](cron-dapp-service.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
* [`eos-common`](eos-common.md)
* [`seed-frontends`](seed-frontends.md)
* [`seed-models`](seed-models.md)



## Contracts
* [`liquidportfolio`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/portfolio/contracts/eos/liquidportfolio)
## Install
```bash
zeus unbox portfolio
```
## Examples
### Deploy contract
```bash
zeus migrate
```
### Run frontend locally
```bash
zeus run frontend main
```
### Build frontend
```bash
zeus build frontend main
```
### Deploy frontend
```bash
zeus deploy frontend main
```
### Deploy and register frontend
```bash
zeus deploy frontend main --ipfs --register portfolio
```











## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/portfolio)