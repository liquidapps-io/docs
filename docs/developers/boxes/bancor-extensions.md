
bancor-extensions
====================







## Dependencies
### Boxes
* [`seed-eos`](seed-eos.md)
* [`seed-migrations`](seed-migrations.md)
* [`secure-accounts-extensions`](secure-accounts-extensions.md)
* [`seed-extensions`](seed-extensions.md)
* [`seed-models`](seed-models.md)
* [`bancor-relay`](bancor-relay.md)
* [`token`](token.md)




## Install
```bash
zeus unbox bancor-extensions
```
## Examples
### Create Database 
```bash
zeus create converter eosio.token SYS tknarly TKNARLY cnvtkna 0
```
### Run Locally 
```bash
zeus migrate
```

## Zeus Command Extensions

### Subcommands
* ```zeus create bancor-relay --help```

* ```zeus start-localenv 42-eos-local-bnt --help```

* ```zeus start-localenv 42-eth-local-bnt --help```

* ```zeus start-localenv 43-eos-local-bancornetwork --help```

* ```zeus start-localenv 43-eth-local-bancornetwork --help```

* ```zeus start-localenv 44-eos-local-bancorx --help```

* ```zeus start-localenv 44-eth-local-bancorx --help```

* ```zeus start-localenv 45-common-local-bancorx --help```

## Models
### New Model Types
* bancor-relays



## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/economics/bancor-extensions)