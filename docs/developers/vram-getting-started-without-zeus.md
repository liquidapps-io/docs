vRAM Getting Started - without zeus
===================================

```            
       _____            __  __ 
      |  __ \     /\   |  \/  |
__   _| |__) |   /  \  | \  / |
\ \ / /  _  /   / /\ \ | |\/| |
 \ V /| | \ \  / ____ \| |  | |
  \_/ |_|  \_\/_/    \_\_|  |_|
            
```
## Prerequisites

* eosio.cdt - recommend: v1.6.1 https://github.com/EOSIO/eosio.cdt/releases/tag/v1.6.1
* CryptoKylin Testnet Account - https://www.cryptokylin.io/
  * Account: `http://faucet.cryptokylin.io/create_account?**ACCOUNT_NAME**`
  * Faucet: `http://faucet.cryptokylin.io/get_token?**ACCOUNT_NAME**`

## Install

Clone into your `/contracts` directory:
```sh
$ git clone --recursive https://github.com/liquidapps-io/****UPDATE_THIS****
```

vRAM provides a drop in replacement for the multi_index table that is also interacted with in the same way as the traditional multi_index table making it very easy and familiar to use.  

To access the vRAM table, add the following lines to your smart contract:

```sh
#include "../dappservices/log.hpp"
#include "../dappservices/multi_index.hpp"

#define DAPPSERVICES_ACTIONS()
    XSIGNAL_DAPPSERVICE_ACTION
    LOG_DAPPSERVICE_ACTIONS
    IPFS_DAPPSERVICE_ACTIONS

#define DAPPSERVICE_ACTIONS_COMMANDS()
    IPFS_SVC_COMMANDS()LOG_SVC_COMMANDS()

CONTRACT_START()

...

/*** REPLACE eosio : ***/
typedef eosio::multi_index<"accounts"_n, account> accounts_t;

/*** WITH    dapp  : ***/
typedef dapp::multi_index<"accounts"_n, account> accounts_t;

...

CONTRACT_END((youraction1)(youraction2)(youraction2))
```

Compile: 
```sh
$ eosio-cpp -abigen -o contract.wasm contract.cpp
```

Buy RAM:
```sh
$ cleos -u https://kylin.eoscanada.com system buyram ACCOUNT_NAME ACCOUNT_NAME "50.0000 EOS" -p ACCOUNT_NAME@active
```

Set contract:
```sh
$ cleos -u https://kylin.eoscanada.com set contract ACCOUNT_NAME ../contract -p ACCOUNT_NAME@active
```

Now that your contract is deployed to Kylin, use the [DAPP faucet UPDATE_THIS_LINK](https://www.google.com/) to get some DAPP tokens.

Once you've done that, go ahead and select a service package from the DSP of your choice.  DSPs who have registered their service packages may be found in the [package table](https://kylin.eosx.io/account/dappservices?mode=contract&sub=tables&table=package&lowerBound=&upperBound=&limit=100) under the dappservices account.

Select your package: 

```sh
$ cleos -u https://kylin.eoscanada.com push action dappservices selectpkg '["ACCOUNT_NAME","PROVIDER","SERVICE","PACKAGE_ID"]' -p ACCOUNT_NAME@active
```

Then stake your DAPP to the DSP that you selected the service package for:

```sh
$ cleos -u https://kylin.eoscanada.com push action dappservices stake '["ACCOUNT_NAME","PROVIDER","SERVICE","50.0000 DAPP"]' -p ACCOUNT_NAME@active
```

Finally you can now test your vRAM implementation by sending an action through your DSP's API endpoint.  The endpoint can be found in the [package table](https://kylin.eosx.io/account/dappservices?mode=contract&sub=tables&table=package&lowerBound=&upperBound=&limit=100) of the dappservices account on all chains.

You transaction will look something like this **UPDATE_THIS_LATER**:

```sh
executed transaction: 865a3779b3623eab94aa2e2672b36dfec9627c2983c379717f5225e43ac2b74a  104 bytes  67049 us
#  natdeveloper <= natdeveloper::upsert         {"user":"natdeveloper","first_name":"nat"}
>> {"version":"1.0","etype":"service_request","payer":"natdeveloper","service":"ipfsservice1","action":"commit","provider":"","data":"DHBVpVFtlbKZA25hdAA="}
```