
templates-dsp-service
====================









* [`mocha`](mocha.md)
* [`seed-eos`](seed-eos.md)
* [`seed-migrations`](seed-migrations.md)
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








### Model Instances
#### [templates/dsp-service.json](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/templates/templates-dsp-service/models/templates/dsp-service.json)
```json
{
  "name": "dsp-service",
  "port": 13150,
  "prettyName": "Liquid...",
  "stage": "WIP or Alpha, or BETA, or Stable",
  "version": "0.9",
  "description": "Allows interaction with contract without a native EOS Account",
  "contract": "account_service_contract_is_deployed_on",
  "commands": {
    "example_command": {
      "blocking (true  = syncronous, false = async)": false,
      "request": {},
      "callback": {
        "payload": "std::vector<char>",
        "sig": "eosio::signature",
        "pubkey": "eosio::public_key"
      },
      "signal": {
        "sig": "eosio::signature",
        "pubkey": "eosio::public_key"
      }
    }
  }
}
```

## [Source](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/templates/templates-dsp-service)