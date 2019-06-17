
airdrop 
====================




## Dependencies
### Boxes
* [`oracle-dapp-service`](oracle-dapp-service.md)
* [`readfn-dapp-service`](readfn-dapp-service.md)
* [`token`](token.md)
### npm packages
* `aws-sdk`
* `node-fetch`
* `async-csv`
## Contracts
* `airdrop`
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


## (Source)[https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups//opt/atlassian/pipelines/agent/build/zeus/boxes/groups/economics/airdrop]
