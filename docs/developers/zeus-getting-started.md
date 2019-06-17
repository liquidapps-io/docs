Zeus Getting Started
====================

## Overview
zeus-cmd is an Extensible command line tool. SDK extensions come packaged in "boxes".

## Boxes

* EOSIO dApp development support
* DAPP Services support

[Boxes overview](boxes/zeus-boxes.md)

## Hardware Requirements
* 16GB RAM
* 2 CPU Cores

## Prerequisites

* nodejs == 10.x (nvm recommended)
* curl

Recommended (otherwise falling back to docker)
* [eosio.cdt v1.6.1](https://github.com/EOSIO/eosio.cdt/releases/tag/v1.6.1)
* [eosio v1.7.2](https://github.com/EOSIO/eos/releases/tag/v1.7.2)


## Install Zeus

```bash
npm install -g @liquidapps/zeus-cmd
```

### Notes regarding docker on mac:
Recommended version: 18.06.1-ce-mac73

## Upgrade

```bash
npm update -g @liquidapps/zeus-cmd
```

## Test
```bash
zeus unbox helloworld
cd helloworld
zeus test
```

## Samples Boxes
### vRAM
* [coldtoken](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/coldtoken) - vRAM based eosio.token
* [deepfreeze](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/deepfreeze) - vRAM based cold storage contract
* [vgrab](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/vgrab) - vRAM based airgrab for eosio.token
* [cardgame](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/cardgame) - vRAM supported elemental battles
* [registry](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/eos-framework/registry) - Generic Registry - the1registry

### Zeus Extensions
* [contract-migrations-extensions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/core/contract-migrations-extensions)
* [build-extensions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/core/build-extensions)
* [test-extensions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/core/test-extensions)
* [eos-extensions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/eos-sdk/eos-extensions)
* [unbox-extensions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/repos/unbox-extensions)
* [demux](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/microservices/demux)

### DAPP Services
* [ipfs-dapp-service](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service)
* [log-dapp-service](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/log-dapp-service)
* [cron-dapp-service](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/cron-dapp-service)
* [oracle-dapp-service](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/oracle-dapp-service)

### Misc.
* [microauctions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/economics/microauctions) - Micro Auctions
* [eos-detective-reports](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/economics/eos-detective-reports) - EOS Detective Reports - by EOSNation
* [helloworld](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/eos-sdk/sample-eos-cpp) - Hello World
* [token](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/eos-framework/token) - Standard eosio.token

## Other Options
```bash
zeus compile #compile contracts
zeus migrate #migrate contracts (deploy to local eos.node)
```

## Usage inside a project
```bash
zeus --help 
```

### List Boxes
```bash
zeus list-boxes
```

## Project structure
### Directory structure
```
    extensions/
    contracts/
    frontends/
    models/
    test/
    migrations/
    utils/
    services/
    zeus-box.json
    zeus-config.js
```
### zeus-box.json
```
    {
      "ignore": [
        "README.md"
      ],
      "commands": {
        "Compile contracts": "zeus compile",
        "Migrate contracts": "zeus migrate",
        "Test contracts": "zeus test"
      },
      "install":{
          "npm": {
              
          }
      },
      "hooks": {
        "post-unpack": "echo hello"
      }
    }
```

### zeus-config.js
```
    module.exports = {
        defaultArgs:{
          chain:"eos",
          network:"development"
        },
        chains:{
            eos:{
                networks: {
                    development: {
                        host: "localhost",
                        port: 7545,
                        network_id: "*", // Match any network id
                        secured: false
                    },
                    jungle: {
                        host: "localhost",
                        port: 7545,
                        network_id: "*", // Match any network id
                        secured: false
                    },
                    mainnet:{
                        host: "localhost",
                        port: 7545,
                        network_id: "*", // Match any network id
                        secured: false
                    }
                }
            }
        }
    };
```

## Notes regarding permissions errors:
Recommend using Node Version Manager (nvm)
```bash
sudo apt install curl
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
exec bash
nvm install 10
nvm use 10
```
Or you can try the following:
```bash
sudo groupadd docker
sudo usermod -aG docker $USER

#If still getting error:
sudo chmod 666 /var/run/docker.sock
```