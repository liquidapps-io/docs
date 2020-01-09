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

vRAM is a caching solution that enables DAPP Service providers (specialized EOS nodes) to load data to and from RAM <> vRAM on demand.  Data is evicted from RAM and stored in vRAM after the transaction has been run.  This works similar to the way data is passed to and from regular computer RAM and a hard drive.  As with EOS, RAM is used in a computer sometimes because it is a faster storage mechanism, but it is scarce in supply as well.  For more information on the technical details of the transaction lifecycle, please read the [vRAM Guide For Experts](https://medium.com/the-liquidapps-blog/vram-guide-for-experts-f809c8f82a27) article and/or the [whitepaper](https://liquidapps.io/DAPP%20Network%20and%20DAPP%20Token%20Whitepaper%20v2.0.pdf).

vRAM requires a certain amount of data to be stored in RAM permanently in order for the vRAM system to be trustless.  This data is represented as rows in the `ipfsentry` table of any `dapp::multi_index` enabled smart contract.  It is used to create the merkle root that is used by the smart contract to verify the data's integrity before being used.  Each row in the `ipfsentry` table is structured with an IPFS universal resource identifier (`vector<char>`) and a shard id (`uint64_t`).  The default maximum amount of shards (thus relating to the maximum amount of RAM required) is 1024.  Said differently, the total amount of RAM that a `dapp::multi_index` will need to use is `1024 * (vector<char> data + uint64_t id)`.

The DAPP Services Provider is responsible for removing this data after the transaction's lifecycle.  If the DSP does not perform this action, the `ipfsentry` table will continue to grow until the account's RAM supply has been exhausted or the DSP resumes its services.

## Hardware Requirements

## Prerequisites

* [eosio.cdt v1.6.3](https://github.com/EOSIO/eosio.cdt/releases/tag/v1.6.3)
* [eosio v1.7.4](https://github.com/EOSIO/eos/releases/tag/v1.7.4)
* If testing on Kylin: [eosio v1.8.8](https://github.com/EOSIO/eos/releases/tag/v1.8.8)
* [Kylin Account](kylin-account.md)

## Install

Clone into your project directory:
```bash
git clone  --single-branch --branch v1.4 --recursive https://github.com/liquidapps-io/dist
```

## Modify your contract

vRAM provides a drop in replacement for the `eosio::multi_index` table that is also interacted with in the same way as the traditional `eosio::multi_index` table making it very easy and familiar to use. Please note that secondary indexes are not currently implemented for `dapp::multi_index` tables. 

To access the vRAM table, add the following lines to your smart contract:

## Advanced features
To use advanced multi index features include `#define USE_ADVANCED_IPFS` at the top of the contract file while following the steps below. If you have already deployed a contract that does not use advanced features, do not add this line, as it is not backwards compatible.

### At header:
```cpp
#include "../dist/contracts/eos/dappservices/multi_index.hpp"

#define DAPPSERVICES_ACTIONS() \
    XSIGNAL_DAPPSERVICE_ACTION \
    IPFS_DAPPSERVICE_ACTIONS

#define DAPPSERVICE_ACTIONS_COMMANDS() \
    IPFS_SVC_COMMANDS()
  
#define CONTRACT_NAME() mycontract

CONTRACT_START()
   public:
```

### Replace eosio::multi_index
```cpp
/*** REPLACE ***/
typedef eosio::multi_index<"accounts"_n, account> accounts_t;

/*** WITH ***/
typedef dapp::multi_index<"accounts"_n, account> accounts_t;
      
/*** ADD (for client side query support): ***/
typedef eosio::multi_index<".accounts"_n, account> accounts_t_v_abi;
TABLE shardbucket {
    std::vector<char> shard_uri;
    uint64_t shard;
    uint64_t primary_key() const { return shard; }
};
typedef eosio::multi_index<"accounts"_n, shardbucket> accounts_t_abi;
```

### Add actions dispatcher
```cpp
CONTRACT_END((youraction1)(youraction2)(youraction2))
```

## Compile
```bash
eosio-cpp -abigen -o contract.wasm contract.cpp
```

## Deploy Contract
```bash
export DSP_ENDPOINT=https://kylin-dsp-1.liquidapps.io
export KYLIN_TEST_ACCOUNT=
export KYLIN_TEST_PUBLIC_KEY=
# Set contract code and abi
cleos -u $DSP_ENDPOINT set contract $KYLIN_TEST_ACCOUNT ../contract -p $KYLIN_TEST_ACCOUNT@active
# Set contract permissions
cleos -u $DSP_ENDPOINT set account permission $KYLIN_TEST_ACCOUNT active "{\"threshold\":1,\"keys\":[\"$KYLIN_TEST_PUBLIC_KEY\"],\"accounts\":[{\"permission\":{\"actor\":\"eosio.code\",\"permission\":\"active\"},\"weight\":1}]}" active -p $KYLIN_TEST_ACCOUNT@active
```

## Select and stake DAPP for DSP package

[DSP Package and staking](dsp-packages-and-staking.md)

## Test
Finally you can now test your vRAM implementation by sending an action through your DSP's API endpoint.  

The endpoint can be found in the [package table](https://kylin.eosx.io/account/dappservices?mode=contract&sub=tables&table=package&lowerBound=&upperBound=&limit=100) of the dappservices account on all chains.

```bash
cleos -u $DSP_ENDPOINT push action $KYLIN_TEST_ACCOUNT youraction1 "[\"param1\",\"param2\"]" -p $KYLIN_TEST_ACCOUNT@active
```

The result should look like:
```
executed transaction: 865a3779b3623eab94aa2e2672b36dfec9627c2983c379717f5225e43ac2b74a  104 bytes  67049 us
#  yourcontract <= yourcontract::youraction1         {"param1":"param1","param2":"param2"}
>> {"version":"1.0","etype":"service_request","payer":"yourcontract","service":"ipfsservice1","action":"commit","provider":"","data":"DH......"}
```

## Get table row
```bash
curl http://$DSP_ENDPOINT/v1/dsp/ipfsservice1/get_table_row -d '{"contract":"CONTRACT_ACCOUNT","scope":"SCOPE","table":"TABLE_NAME","key":"TABLE_PRIMARY_KEY"}' | python -m json.tool
```