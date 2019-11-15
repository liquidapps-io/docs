Zeus Getting Started
====================

## Overview
Zeus-cmd is an extensible command line tool. SDK extensions come packaged in "boxes" and are served through IPFS.  Zeus is currently in alpha.  *As a note, all Zeus commands must be run within the root directory of the package that was unboxed.*

* [zeus-sdk](https://github.com/liquidapps-io/zeus-sdk)
* [overview of boxes](boxes/zeus-boxes.md)

## Features:

* Smart contract templating with a single command
* Install nodeos, keosd, cleos, and eosio.cdt with a single command
* Simulate a blockchain node with a single command
* Test, compile, and deploy smart contracts
* Easily migrate a contract to a different EOSIO chain such as the Kylin and Jungle testnets or the mainnet
* Design fluid dApp frontends
* Cross-platform (Windows, OS X, Linux)
* Easily install necessary libraries with a package manager
* Truffle-like interface
* Manage development lifecycle with version control
* Open source (BSD License)
* And more...

## [Gitpod Zeus-IDE](https://gitpod.io/#https://github.com/liquidapps-io/zeus-ide)

If you want to be up and running with Zeus quickly, you can use our cloud based Zeus-IDE, all you need is a Github account! [Try it here!](zeus-ide)

## [dapp-client library](https://www.npmjs.com/package/@liquidapps/dapp-client)

The dapp-client library makes it easier to interact with the DAPP Network's core smart contracts and services, [read more here](dapp-client).

## Hardware Requirements
* 16GB RAM
* 2 CPU Cores

## Prerequisites

* *node version 10.16.3* is recommended (nvm recommended, install at bottom of doc)
* curl
* cmake
* make

### Use node version manager to install node

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
# use install instructions provided to set PATH
nvm install 10.16.3
nvm use 10.16.3
```

## Recommended eosio.cdt and eosio versions
Automatically installed with `zeus unbox helloworld`

* [eosio.cdt v1.6.3](https://github.com/EOSIO/eosio.cdt/releases/tag/v1.6.3)
* [eosio v1.8.6](https://github.com/EOSIO/eos/releases/tag/v1.8.6)


## Install Zeus

```bash
npm install -g @liquidapps/zeus-cmd
```

## Update

```bash
npm update -g @liquidapps/zeus-cmd
```

## Test
```bash
zeus unbox helloworld
cd helloworld
zeus test
```

## Create your own contract
This box supports all DAPP Services and unit tests and is built to integrate your own DAPP Network logic.  When you run the command a sample unit test and smart contract will be created.
```bash
mkdir mydapp; cd mydapp
zeus unbox dapp --no-create-dir
zeus create contract mycontract
```

## Try out LiquidApps's take on Elemental Battles:
[https://cardgame1112.dnsregistry1.com/](https://cardgame1112.dnsregistry1.com/) | [code](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/cardgame)

The game incorporates:

* vRAM - light-weight caching solution for EOSIO based RAM
* LiquidAccounts - EOSIO accounts that live in vRAM instead of RAM
* LiquidDNS - DNS service on the blockchain | [contract table](https://kylin.bloks.io/account/dnsregistry1?loadContract=true&tab=Tables&table=dnsentry&account=dnsregistry1&scope=cardgame1112&limit=100)
* Frontend stored on IPFS
* user data is stored in the vRAM `dapp::multi_index` table (vRAM) | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/sample/cardgame/contracts/eos/cardgame/cardgame.hpp#L94)
* keys stored in `dapp::multi_index` table | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/sample/cardgame/contracts/eos/cardgame/cardgame.hpp#L94)
* keys created using the account name and password as seed phrases | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/sample/cardgame/frontends/main/src/components/Login/Login.jsx#L35)
* eosjs-ecc's `seedPrivate` method is used to create the keypair | [code](https://github.com/EOSIO/eosjs-ecc#seedprivate)
* logic to create LiquidAccount transactions | [code](https://github.com/liquidapps-io/zeus-sdk/blob/master/boxes/groups/sample/cardgame/frontends/main/src/services/ApiService.js#L12)

To launch locally:
```bash
zeus unbox cardgame
cd cardgame
zeus migrate
zeus run frontend main
```

## Or try out vCPU with our LiquidChess game:
[https://chess.liquidapps.io/](https://chess.liquidapps.io/) | [code](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/chess)

The game incorporates:

* vRAM - light-weight caching solution for EOSIO based RAM
* LiquidAccounts - EOSIO accounts that live in vRAM instead of RAM
* LiquidDNS - DNS service on the blockchain
* vCPU - a solution to scale blockchain processing power horizontally

To launch locally:
```bash
zeus unbox chess
cd chess
zeus migrate
zeus run frontend main
```

## Samples Boxes

```bash
zeus unbox <INSERT_BOX>
```

### vRAM Boxes
* [coldtoken](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/coldtoken) - vRAM based eosio.token
* [deepfreeze](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/deepfreeze) - vRAM based cold storage contract
* [vgrab](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/vgrab) - vRAM based airgrab for eosio.token
* [registry](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/eos-framework/registry) - vRAM based item registration

### Zeus Extension Boxes
* [contract-migrations-extensions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/core/contract-migrations-extensions) - contract create/deployment command template, deploy contract and allocate DAPP tokens
* [test-extensions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/core/test-extensions) - provides logic to test smart contract with unit tests
* [eos-extensions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/eos-sdk/eos-extensions) - install eos/eosio.cdt, launch local nodeos, launch system contracts
* [unbox-extensions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/repos/unbox-extensions) - logic to unbox zeus boxes, list all boxes, and deploy a new box
* [demux](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/microservices/demux) - install EOSIO's demux backend to capture events for contracts using the state-history plugin

### DAPP Services Boxes
* [ipfs-dapp-service](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service) - utilize the dapp::multi_index table to store data in IPFS (vRAM) instead of RAM
* [cron-dapp-service](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/cron-dapp-service) - schedule CRON tasks on-chain
* [oracle-dapp-service](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/oracle-dapp-service) - provide oracle services
* [readfn-dapp-service](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/readfn-dapp-service) - read a contract function without the need to submit a trx to the chain
* [vaccounts-dapp-service](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/vaccounts-dapp-service) - EOSIO accounts that live in vRAM instead of RAM
* [vcpu-dapp-service](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/vcpu-dapp-service) - scale blockchain processing power horizontally

### Miscellaneous Boxes
* [microauctions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/economics/microauctions) - twin reverse dutch auctions used in DAPP's generation event
* [eos-detective-reports](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/economics/eos-detective-reports) - EOS Detective Reports - by EOSNation - [https://eosdetective.io/](https://eosdetective.io/)
* [helloworld](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/eos-sdk/sample-eos-cpp) - Hello World
* [token](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/eos-framework/token) - Standard eosio.token
* [airhodl](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/economics/airhodl) - First ever Air-HODL

## Zeus Options
*please note: zeus commands are directory sensitive, all commands should be performed in root of box*

### Zeus compile
Compile a smart contract
```bash
zeus compile

# optional flags:

--all # compile all contracts
# default: true
--chain # chain to work on
# default: eos
```

### Zeus migrate
Compile and migrate a smart contract to another network such as the [Kylin Testnet](https://www.cryptokylin.io/), [Jungle Testnet](https://monitor.jungletestnet.io/#home), or [Mainnet](https://eosnetworkmonitor.io/).

Be sure to run the following commands from inside the directory you unboxed, e.g., if you unboxed coldtoken, be in `/coldtoken`.  Also be sure to set the network in the import and migrate commands so Zeus knows what chain the keys / contract is operating on (mainnet, kylin, or jungle).
```bash
# keys are stored in ~/.zeus/networks/<NETWORK>/accounts/
zeus key import <CONTRACT_ACCOUNT_NAME> --owner-private-key <KEY> --active-private-key <KEY> --network=kylin
# contract deployment files are stored in ~/<BOX>/models/contract-deployments
zeus create contract-deployment <CONTRACT_FILE_NAME> <CONTRACT_ACCOUNT_NAME>
zeus migrate --network=kylin --creator=<CONTRACT_ACCOUNT_NAME> --creator-key=<ACTIVE_PRIVATE_KEY>

# optional flags:

--compile-all # compile all contracts
# default: true
--wallet # keosd wallet to use
# default: zeus
--creator-key # contract creator private key
# default: (eosio test key) 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
--creator # account to set contract to
# default: eosio
--reset # reset testing environment
# default: true
--chain # chain to work on
# default: eos
--network # network to work on (other options, kylin, jungle, mainnet)
# default: development (local)
--verbose-rpc # verbose logs for blockchain communication
# default: false
--storage-path # path for persistent storage',
# default: path.join(require('os').homedir(), '.zeus')
--stake # account EOSIO staking amount
# default: '30.0000'
--no-compile-all # do not compile contracts
--no-reset # do not reset local testing environment
```

### Zeus test
Compile and unit test a smart contract
```bash
zeus test

# optional flags:

--compile-all # compile all contracts
# default: true
--wallet # keosd wallet to use
# default: zeus
--creator-key # contract creator key
# default: (eosio test key) 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
--creator # account to set contract to
# default: eosio
--reset # reset testing environment
# default: true
--chain # chain to work on
# default: eos
--network # network to work on (other options, kylin, jungle, mainnet)
# default: development (local)
--verbose-rpc # verbose logs for blockchain communication
# default: false
--storage-path # path for persistent storage',
# default: path.join(require('os').homedir(), '.zeus')
--stake # account EOSIO staking amount
# default: '30.0000'
--no-compile-all # do not compile contracts
--no-reset # do not reset local testing environment
```

### Zeus Import/Export Keys
Import and export keys to your Zeus wallet.  **Please note by default keys are imported without encryption.**

```bash
zeus key import <ACCOUNT_NAME> --owner-private-key <KEY> --active-private-key <KEY>

# optional flags:

--encrypted # encrypt the account keys with a password
# default: false
--storage # path to the wallet which will store the key
# default: ${home}/.zeus/networks
--network # network to work on (other options, kylin, jungle, mainnet)
# development (local)
--password # password to encrypt the keys with

zeus key export <ACCOUNT_NAME>

# optional flags:

--encrypted # exports encrypted key
# default: false
--storage # path to where the key is stored
# default: ${home}/.zeus/networks
--network # network to work on (other options, kylin, jungle, mainnet)
# default: development (local)
--password # password to decrypt the keypair
```

### Zeus Deploy
Deploy a custom Zeus box to your local IPFS node.  Once deployed, if the `--update-mapping` flag is used, you may unbox this box like other packages.

```bash
zeus deploy box

# optional flags:

--update-mapping # updates local mapping.js file with an IPFS URI where the package may be accessed at
# default: false
```

### Help
```bash
zeus --help 
```

### List Boxes
Lists all available zeus boxes that can be unboxed.

```bash
zeus list-boxes
```

### Update Boxes
Updates zeus boxes for currently unboxed project

```bash
zeus update --boxes
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
Add commands, NPM intalls, ignores, and command hooks
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
        "post-unpack": "echo hello",
        "post-install": "git clone ..."
      }
    }
```

### zeus-config.js
Configure zeus environments available to interact with.  The `zeus-config.js` file is located in the root of an unboxed directory.
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
                        host: "jungle2.cryptolions.io",
                        port: 80,
                        network_id: "*", // Match any network id
                        secured: false
                    },
                    kylin: {
                        host: "api.kylin.eosbeijing.one",
                        port: 80,
                        network_id: "*",
                        secured: false
                    },
                    mainnet:{
                        host: "bp.cryptolions.io",
                        port: 8888,
                        network_id: "*", // Match any network id
                        secured: false
                    }
                }
            }
        }
    };
```