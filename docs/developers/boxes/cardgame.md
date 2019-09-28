
cardgame
====================







## Dependencies
### Boxes
* [`hooks-npm`](hooks-npm.md)
* [`mocha`](mocha.md)
* [`seed-eos`](seed-eos.md)
* [`seed-migrations`](seed-migrations.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
* [`eos-common`](eos-common.md)
* [`all-dapp-services`](all-dapp-services.md)
* [`seed-frontends`](seed-frontends.md)
* [`seed-models`](seed-models.md)



## Contracts
* [`cardgame`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/cardgame/contracts/eos/cardgame)
## Install
```bash
zeus unbox cardgame
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
zeus deploy frontend main --ipfs --register cardgame1111
```








### Model Instances
#### [sample/cardgame.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/cardgame/models/contract-deployments/cardgame.json)
```json
{
  "contract": "cardgame",
  "account": "cardgame1111"
}
```

## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/cardgame)