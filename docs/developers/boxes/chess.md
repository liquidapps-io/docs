
chess
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
* [`oracle-stockfish`](oracle-stockfish.md)
* [`seed-models`](seed-models.md)



## Contracts
* [`chess`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/chess/contracts/eos/chess)
## Install
```bash
zeus unbox chess
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
zeus deploy frontend main --ipfs --register chess
```








### Model Instances
#### [sample/chess.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/chess/models/contract-deployments/chess.json)
```json
{
  "contract": "cardgame",
  "account": "cardgame1111"
}
```
## Tests 
* [chess.spec.js](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/chess/test/chess.spec.js)
## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/chess)