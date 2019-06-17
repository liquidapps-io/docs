
airdrop
====================







## Dependencies
### Boxes
* [`oracle-dapp-service`](oracle-dapp-service.md)
* [`readfn-dapp-service`](readfn-dapp-service.md)
* [`token`](token.md)
### npm packages
* [`aws-sdk`](http://npmjs.com/package/aws-sdk)
* [`node-fetch`](http://npmjs.com/package/node-fetch)
* [`async-csv`](http://npmjs.com/package/async-csv)

## Contracts
* [`airdrop`](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/economics/airdrop/contracts/eos/airdrop)
## Install
```bash
zeus unbox airdrop
```
## Examples
### Create airdrop 
```bash
zeus airdrop create myairdrop
```
### Download Snapshot 
```bash
zeus airdrop download-snapshot myairdrop
```
### Transform 
```bash
zeus airdrop transform-entries myairdrop
```
### Populate Entries 
```bash
zeus airdrop populate-entries myairdrop
```
### Init 
```bash
zeus airdrop init myairdrop
```
### Periodic Cleanup 
```bash
zeus airdrop cleanup myairdrop
```

## Zeus Command Extensions
* ```zeus airdrop  --help```
### Subcommands
* ```zeus airdrop cleanup --help```

* ```zeus airdrop create --help```

* ```zeus airdrop download-snapshot --help```

* ```zeus airdrop init --help```

* ```zeus airdrop notify --help```

* ```zeus airdrop populate-entries --help```

* ```zeus airdrop transform-snapshot --help```

## Models
### New Model Types
* airdrops


## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/economics/airdrop)