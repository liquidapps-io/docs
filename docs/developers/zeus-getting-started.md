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
* git

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
* [eosio v2.0.5](https://github.com/EOSIO/eos/releases/tag/v2.0.5)

*note our contracts are not yet compatible with eosio.cdt 1.7.0*

## Install Zeus

```bash
npm install -g @liquidapps/zeus-cmd
```

## Create Zeus box

Before unboxing an existing box or creating a new box, the command `zeus box create` must be run.  This is intended to act like `npm init`.  The command creates a boilerplate `zeus-box.json` file as well as a `package.json` file.  Once this command is run, a box may be unboxed.

```bash
zeus box create
```

## Unbox

The unbox command allows a user to unbox one or multiple boxes, similar to `npm install <MODULE> [MODULE2 ...]`.  A version may also be specified.

```bash
zeus box create
zeus unbox helloworld
zeus unbox helloworld@1.0.1
zeus unbox helloworld ipfs-dapp-service
```

## Update

```bash
npm update -g @liquidapps/zeus-cmd
```

## Test
```bash
mkdir helloworld; cd helloworld
zeus box create
zeus unbox helloworld
zeus test -c
```

## Create your own contract
This box supports all DAPP Services and unit tests and is built to integrate your own DAPP Network logic.  When you run the command a sample unit test and smart contract will be created.
```bash
mkdir mydapp; cd mydapp
zeus box create
zeus unbox all-dapp-services
zeus create contract mycontract
```
*contract is located in /zeus_boxes/contracts, test is located in /zeus_boxes/test*

## Try out LiquidApps's take on Elemental Battles:
[http://elemental.liquidapps.io/](http://elemental.liquidapps.io/) | [code](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/cardgame)

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
mkdir cardgame; cd cardgame
zeus box create
zeus unbox cardgame
zeus migrate
zeus run frontend main
```

## Try out vCPU with our LiquidChess game:
[https://chess.liquidapps.io/](https://chess.liquidapps.io/) | [code](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/chess)

The game incorporates:

* vRAM - light-weight caching solution for EOSIO based RAM
* LiquidAccounts - EOSIO accounts that live in vRAM instead of RAM
* LiquidDNS - DNS service on the blockchain
* vCPU - a solution to scale blockchain processing power horizontally

To launch locally:
```bash
mkdir chess; cd chess
zeus box create
zeus unbox chess
zeus migrate
zeus run frontend main
```

## Try out LiquidPortfolio
[http://portfolio.liquidapps.io/](http://portfolio.liquidapps.io/) | [code](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/portfolio)

LiquidPortfolio is a portfolio tracking tool for BTC, ETH (and tokens), and EOS (and tokens).  The tool displays the total current value of the portfolio while also encrypting all user account info with the LiquidAccount's private key.

The game incorporates:

* vRAM - light-weight caching solution for EOSIO based RAM
* LiquidAccounts - EOSIO accounts that live in vRAM instead of RAM
* LiquidHarmony - oracle service for fetching prices
* LiquidDNS - DNS service on the blockchain
* Encryption/Decryption locally of account data using LiquidAccount private key

To launch locally:
```bash
mkdir portfolio; cd portfolio
zeus box create
zeus unbox portfolio
zeus migrate
zeus run frontend main
```

*note if you compile and run this contract yourself, you will need to update all instances of `uint8[][]` within the abi to `bytes[]`*

## Samples Boxes

```bash
zeus box create
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
* [demux](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/microservices/demux) - install EOSIO's demux backend to capture events for contracts using the state-history plugin

### DAPP Services Boxes
The DAPP Service boxes allow you to isolate the service that you wish to work with.  If you instead would like to use all of the DAPP Services, you may unbox the `all-dapp-services` box.

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
Compile a smart contract.  You can either compile all of the contracts within the contracts directory with `zeus compile` or a specific contract by name, such as `zeus compile dappservices`
```bash
zeus compile <CONTRACT_NAME>

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
--no-compile # do not compile contracts
--no-reset # do not reset local testing environment
```

### Zeus test
Unit test a smart contract.  To run the unit tests for all smart contracts within the unboxed directory, use `zeus test`.  The compile all smart contracts before testing, use `zeus test -c`, `-c` being the alias to compile.  To test and or compile for a specific contract name, add the `<CONTRACT_NAME>` to the command.
```bash
zeus test # run all unit tests
zeus test <CONTRACT_NAME> # run unit tests for contract name
zeus test -c <CONTRACT_NAME> # compile and run unit tests for contract name

# optional flags:

--compile # compile contracts
# default: false
# alias: c
--wallet # keosd wallet to use
# default: zeus
# alias: w
--creator-key # contract creator key
# default: (eosio test key) 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
--creator # account to set contract to
# default: eosio
# alias: a
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
--vaccount # bool whether account is a LiquidAccount
# default: false

zeus key export <ACCOUNT_NAME>

# optional flags:

--encrypted # exports encrypted key
# default: false
--storage # path to where the key is stored
# default: ${home}/.zeus/networks
--network # network to work on (other options, kylin, jungle, mainnet)
# default: development (local)
--password # password to decrypt the keypair
--vaccount # bool whether account is a LiquidAccount
# default: false
```


### Add or remove a box from the local mapping.json file
Zeus uses 2 mapping files to unbox boxes.  The `builtin-mapping.json` file is for boxes that are a part of the official zeus-sdk repo (located: `.../node_modules/@liquidapps/zeus-cmd/lib/resources/builtin-mapping.json`). This file only changes when Zeus is updated. There is also a local zeus box for modifying existing boxes from the `builtin-mapping.json` and adding new boxes.  If a box exists in both the builtin and the local mapping files, the local mapping file will be used.  To use the builtin box instead, you must remove the local version first.

```bash
zeus box add <BOX_NAME> <VERSION> <URI>
# zeus box add liquidx-jungle 1.0.1 https://s3.us-east-2.amazonaws.com/liquidapps.artifacts/boxes/0a98835c75debf2f1d875be8be39591501b15352f7c017799d0ebf3342668d2c.zip

# to deploy helloworld box locally then add
# zeus box deploy
# zeus box add helloworld 1.0.1 file:///home/ubuntu/.zeus/boxes/helloworld/box.zip

# to remove
# zeus list-boxes # will see new box under 'Local Boxes:'
zeus box remove <BOX_NAME> <VERSION>
# zeus box remove liquidx-jungle 1.0.1
# zeus list-boxes, will be gone
```

### Zeus Deploy
Deploy a custom Zeus box to your local working directory.  Once deployed, if the `--update-mapping` flag is used, you may unbox this box like other packages.  The `--type` method can be used to determine what medium to deploy the box to.  The default `local` deploys with the syntax `file://${packagePath}/box.zip`.  The option `ipfs` deploys to the IPFS network with the syntax `ipfs://${hash}`.

```bash
zeus deploy box

# optional flags:

--update-mapping # updates local mapping.json file with an IPFS URI where the package may be accessed at
# default: true
--type # deploy destination (local, ipfs)
# default: local
```

### Zeus RC File
An RC file allows command flags specified in a json file to be used automatically without needing to add them to a command each time it is used. To use an RC file, on can be created in the directory: `~/.zeus/zeusrc.json` to persist between deleting boxes and updating Zeus, or in another directory using the `--rc-file` flag to specify the relative path.

Example RC file: 

```json
{
    "verbose": true,
    "type": "local",
    "update-mapping": true,
    "test": true,
    "compile": true
}
```

Example usage:
```bash
# use rc file in local directory
zeus test --rc-file ./zeusrc.json
# ignore rc file
zeus test -c --rc-ignore
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