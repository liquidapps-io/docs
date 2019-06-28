
templates-dsp-service
====================







## Dependencies
### Boxes
* [`hooks-npm`](hooks-npm.md)
* [`mocha`](mocha.md)
* [`seed-eos`](seed-eos.md)
* [`seed-migrations`](seed-migrations.md)
* [`hooks-cpp-contracts`](hooks-cpp-contracts.md)
* [`eos-common`](eos-common.md)
* [`dapp-services`](dapp-services.md)
* [`seed-models`](seed-models.md)
* [`templates-extensions`](templates-extensions.md)




## Install
```bash
zeus unbox templates-dsp-service
```
## Examples
### Create contract from template 
```bash
zeus create dsp-service someservice
```

## Zeus Command Extensions

### Subcommands
* ```zeus create dsp-service --help```




### Model Instances
#### [templates/dsp-service.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/templates/templates-dsp-service/models/templates/dsp-service.json)
```json
{
  "name": "dsp-service",
  "args": [
    "servicename"
  ],
  "optionals": [
    "servicename"
  ]
}
```

## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/templates/templates-dsp-service)