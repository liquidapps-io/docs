Zeus Create Service
=====================

# Create service boilerplate

The `templates-dsp-service` box contains a basic version of:

- consumer contract - contract showcasing simple examples of how to use the service
- unit test - file that shows examples of the consumer contract being used
- dsp-service.json - this goes in the `/models/dapp-service` directory of the service
- myservice-dapp-service-node.js file for running the DSPs node logic

```bash
zeus unbox templates-dsp-service
zeus create dsp-service myservice
# compile
zeus compile
# zeus test
zeus test
```

*The gitignore ignores all extensions at the moment, in the future you will be able to install those extensions with `zeus install`, but until that time just ignore the `node_modules` folder*

### dsp-service.json

- name - define the simplest version of your DSP service, this is used for DSPs to register the service with the `zeus register dapp-service-provider-package`.  It is the `PACKAGE` env variable
- port - select a port for your service to run on, ensure the other services don't overlap as each service must have a unique port
- prettyName - what the service will be officially referred to (LiquidAccounts, vRAM, etc..)
- stage - Work in Progress (WIP), Alpha, Beta, Stable
- version - current version
- description - short description of service
- commands - list of DSP commands
    - blocking - if true, blocking will treat the command as a synchronous event, if false it will be treated as an asynchonous event

```bash
{
    "name": "dsp-service",
    "port": 13150,
    "prettyName": "Liquid...",
    "stage":"WIP or Alpha, or BETA, or Stable",
    "version": "0.9",
    "description": "Allows interaction with contract without a native EOS Account",
    "contract": "account_service_contract_is_deployed_on",
    "commands": {
        "example_command": {
            "blocking (true  = syncronous, false = async)": false,
            "request": {
            },
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