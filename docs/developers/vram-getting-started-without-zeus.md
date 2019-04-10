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

## Hardware Requirements

## Prerequisites

* [eosio.cdt v1.6.1](https://github.com/EOSIO/eosio.cdt/releases/tag/v1.6.1)
* [eosio v1.7.1](https://github.com/EOSIO/eos/releases/tag/v1.7.1)
* [Kylin Account](kylin-account.md)

## Install

Clone into your `/contracts` directory:
```bash
git clone --recursive https://github.com/liquidapps-io/****UPDATE_THIS****
```


## Modify your contract

vRAM provides a drop in replacement for the multi_index table that is also interacted with in the same way as the traditional multi_index table making it very easy and familiar to use.  

To access the vRAM table, add the following lines to your smart contract:

```cpp
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

## Compile
```bash
eosio-cpp -abigen -o contract.wasm contract.cpp
```

## Deploy Contract
```bash
# Buy RAM:
cleos -u $EOS_ENDPOINT system buyram $KYLIN_TEST_ACCOUNT $KYLIN_TEST_ACCOUNT "50.0000 EOS" -p $KYLIN_TEST_ACCOUNT@active
# Set contract code and abi
cleos -u $EOS_ENDPOINT set contract $KYLIN_TEST_ACCOUNT ../contract -p $KYLIN_TEST_ACCOUNT@active

# Set contract permissions
cleos -u $EOS_ENDPOINT set account permission $KYLIN_TEST_ACCOUNT active "{\"threshold\":1,\"keys\":[\"$KYLIN_TEST_PUBLIC_KEY\"],\"accounts\":[{\"permission\":{\"actor\":\"eosio.code\",\"permission\":\"active\"},\"weight\":1}]}" active -p $KYLIN_TEST_ACCOUNT@active
```

## Select and stake DAPP for DSP package

[DSP Package and staking](dsp-packages-and-staking.md)

## Test
Finally you can now test your vRAM implementation by sending an action through your DSP's API endpoint.  

The endpoint can be found in the [package table](https://kylin.eosx.io/account/dappservices?mode=contract&sub=tables&table=package&lowerBound=&upperBound=&limit=100) of the dappservices account on all chains.

```bash
export EOS_ENDPOINT=https://dspendpoint
cleos -u $EOS_ENDPOINT push action $KYLIN_TEST_ACCOUNT youraction1 "[\"param1\",\"param2\"]" -p $KYLIN_TEST_ACCOUNT@active
```

The result should look like:
```
executed transaction: 865a3779b3623eab94aa2e2672b36dfec9627c2983c379717f5225e43ac2b74a  104 bytes  67049 us
#  yourcontract <= yourcontract::youraction1         {"param1":"param1","param2":"param2"}
>> {"version":"1.0","etype":"service_request","payer":"yourcontract","service":"ipfsservice1","action":"commit","provider":"","data":"DH......"}
```