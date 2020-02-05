vRAM Getting Started
====================

```            
       _____            __  __ 
      |  __ \     /\   |  \/  |
__   _| |__) |   /  \  | \  / |
\ \ / /  _  /   / /\ \ | |\/| |
 \ V /| | \ \  / ____ \| |  | |
  \_/ |_|  \_\/_/    \_\_|  |_|
            
```

vRAM is a caching solution that enables DAPP Service providers (specialized EOS nodes) to load data to and from RAM <> vRAM on demand.  Data is evicted from RAM and stored in vRAM after the transaction has been run.  This works similar to the way data is passed to and from regular computer RAM and a hard drive.  As with EOS, RAM is used in a computer sometimes because it is a faster storage mechanism, but it is scarce in supply as well.  For more information on the technical details of the transaction lifecycle, please read the [vRAM Guide For Experts](https://medium.com/the-liquidapps-blog/vram-guide-for-experts-f809c8f82a27) article and/or the [whitepaper](https://liquidapps.io/DAPP%20Network%20and%20DAPP%20Token%20Whitepaper%20v2.0.pdf).

vRAM requires a certain amount of data to be stored in RAM permanently in order for the vRAM system to be trustless.  This data is stored in a regular `eosio::multi_index` table with the same name as the `dapp::multi_index` vRam table defined by the smart contract. Each row in the regular `eosio::multi_index` table represents the merkle root of a partition of the sharded data with the root hash being `vector<char> shard_uri` and the partition id being `uint64_t shard`. Note that this is equivalent to having a single merkle root with the second layer of the tree being written to RAM for faster access.  The default amount of shards (which is proportional to the maximum amount of permanent RAM required) is 1024 meaning that, the total amount of RAM that a `dapp::multi_index` table will need to permanently use is `1024 * (sizeof(vector<char> shard_uri) + sizeof(uint64_t id))`.

In order to access/modify vRam entries certain data may need to be loaded into RAM in order to prove (via the merkle root) that an entry exists in the table. This temporary data (the "cache") is stored in the `ipfsentry` table. The DAPP Services Provider is responsible for removing this data after the transaction's lifecycle.  If the DSP does not perform this action, the `ipfsentry` table will continue to grow until the account's RAM supply has been exhausted or the DSP resumes its services.

## Prerequisites

* [Zeus](zeus-getting-started.md) - Zeus installs eos and the eosio.cdt if not already installed
* [Kylin Account](kylin-account.md)

## Unbox sample template
This box supports all DAPP Services and unit tests and is built to integrate your own vRAM logic.
```bash
mkdir mydapp; cd mydapp
zeus unbox dapp --no-create-dir
zeus create contract mycontract
```

## Or use one of our template contracts
```bash
# unbox coldtoken contract and all dependencies
zeus unbox coldtoken
cd coldtoken
# unit test coldtoken contract locally
zeus test -c
```

## Advanced features
To use advanced multi index features include `#define USE_ADVANCED_IPFS` at the top of the contract file while following the steps below. If you have already deployed a contract that does not use advanced features, do not add this line, as it is not backwards compatible.

## Add your contract logic
in contract/eos/mycontract/mycontract.cpp
```cpp
#pragma once

#include "../dappservices/ipfs.hpp"
#include "../dappservices/multi_index.hpp"

#define DAPPSERVICES_ACTIONS() \
  XSIGNAL_DAPPSERVICE_ACTION \
  IPFS_DAPPSERVICE_ACTIONS

/*** IPFS: (xcommit)(xcleanup)(xwarmup) ***/
#define DAPPSERVICE_ACTIONS_COMMANDS() \
  IPFS_SVC_COMMANDS() 

/*** UPDATE CONTRACT NAME ***/
#define CONTRACT_NAME() mycontract

using std::string;

CONTRACT_START()
  public:

  /*** YOUR LOGIC ***/

  private:
    struct [[eosio::table]] vramaccounts {
      asset    balance;
      uint64_t primary_key()const { return balance.symbol.code().raw(); }
    };

    /*** VRAM MULTI_INDEX TABLE ***/
    typedef dapp::multi_index<"vaccounts"_n, vramaccounts> cold_accounts_t;

    /*** FOR CLIENT SIDE QUERY SUPPORT ***/
    typedef eosio::multi_index<".vaccounts"_n, vramaccounts> cold_accounts_t_v_abi;
    TABLE shardbucket {
      std::vector<char> shard_uri;
      uint64_t shard;
      uint64_t primary_key() const { return shard; }
    };
    typedef eosio::multi_index<"vaccounts"_n, shardbucket> cold_accounts_t_abi;

/*** ADD ACTIONS ***/
CONTRACT_END((your)(actions)(here))
```

## Compile

See the unit testing section for details on adding unit tests.

```bash
zeus compile
# compile and test with
zeus test -c
```

## Deploy Contract
```bash
export DSP_ENDPOINT=https://kylin-dsp-1.liquidapps.io
export KYLIN_TEST_ACCOUNT=<ACCOUNT_NAME>
export KYLIN_TEST_PUBLIC_KEY=<ACTIVE_PUBLIC_KEY>
# Buy RAM:
cleos -u $DSP_ENDPOINT system buyram $KYLIN_TEST_ACCOUNT $KYLIN_TEST_ACCOUNT "200.0000 EOS" -p $KYLIN_TEST_ACCOUNT@active
# Set contract code and abi
cleos -u $DSP_ENDPOINT set contract $KYLIN_TEST_ACCOUNT ../contract -p $KYLIN_TEST_ACCOUNT@active

# Set contract permissions
cleos -u $DSP_ENDPOINT set account permission $KYLIN_TEST_ACCOUNT active "{\"threshold\":1,\"keys\":[{\"weight\":1,\"key\":\"$KYLIN_TEST_PUBLIC_KEY\"}],\"accounts\":[{\"permission\":{\"actor\":\"$KYLIN_TEST_ACCOUNT\",\"permission\":\"eosio.code\"},\"weight\":1}]}" owner -p $KYLIN_TEST_ACCOUNT@active
```

## Select and stake DAPP for DSP package
 * Use [the faucet](https://kylin-dapp-faucet.liquidapps.io/) to get some DAPP tokens on Kylin
 * Information on: [DSP Packages and staking DAPP/DAPPHDL (AirHODL token)](dsp-packages-and-staking.md)
```bash
export PROVIDER=uuddlrlrbass
export PACKAGE_ID=package1

# select your package: 
export SERVICE=ipfsservice1
cleos -u $DSP_ENDPOINT push action dappservices selectpkg "[\"$KYLIN_TEST_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"$PACKAGE_ID\"]" -p $KYLIN_TEST_ACCOUNT@active

# Stake your DAPP to the DSP that you selected the service package for:
cleos -u $DSP_ENDPOINT push action dappservices stake "[\"$KYLIN_TEST_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"10.0000 DAPP\"]" -p $KYLIN_TEST_ACCOUNT@active
```

## Test
Finally you can now test your vRAM implementation by sending an action through your DSP's API endpoint

```bash
cleos -u $DSP_ENDPOINT push action $KYLIN_TEST_ACCOUNT youraction1 "[\"param1\",\"param2\"]" -p $KYLIN_TEST_ACCOUNT@active

# coldtoken (issue / transfer use vRAM):
cleos -u $DSP_ENDPOINT push action $KYLIN_TEST_ACCOUNT create "[\"$KYLIN_TEST_ACCOUNT\",\"1000000000 TEST\"]" -p $KYLIN_TEST_ACCOUNT
cleos -u $DSP_ENDPOINT push action $KYLIN_TEST_ACCOUNT issue "[\"$KYLIN_TEST_ACCOUNT\",\"1000 TEST\",\"yay vRAM\"]" -p $KYLIN_TEST_ACCOUNT
cleos -u $DSP_ENDPOINT push action $KYLIN_TEST_ACCOUNT transfer "[\"$KYLIN_TEST_ACCOUNT\",\"natdeveloper\",\"1000 TEST\",\"yay vRAM\"]" -p $KYLIN_TEST_ACCOUNT
```

The result should look like:
```
executed transaction: 865a3779b3623eab94aa2e2672b36dfec9627c2983c379717f5225e43ac2b74a  104 bytes  67049 us
#  yourcontract <= yourcontract::youraction1         {"param1":"param1","param2":"param2"}
>> {"version":"1.0","etype":"service_request","payer":"yourcontract","service":"ipfsservice1","action":"commit","provider":"","data":"DH......"}
```

## Get table row
```bash
# zeus:
zeus get-table-row "CONTRACT_ACCOUNT" "TABLE_NAME" "SCOPE" "TABLE_PRIMARY_KEY" "KEYTYPE" "KEYSIZE" --endpoint $DSP_ENDPOINT | python -m json.tool

# contract - account name
# table_name - name of dapp::multi_index table
# scope - table scope to search under
# table_primary_key - primary key of row requesting | options: "string", "number"
# key type (optional) - the type of key being passed in the request ("name", "number", "hex"). If table_primary_key is a js number, key type defaults to "number", otherwise "name". In case of large keys, table_primary_key should be encoded as a hex string, and key type should be "hex".
# key size (optional) - size of key in bits: 64 (uint64_t), 128 (uint128_t), 256 (uint256_t, eosio::checksum256)

# curl: 
curl http://$DSP_ENDPOINT/v1/dsp/ipfsservice1/get_table_row -d '{"contract":"CONTRACT_ACCOUNT","scope":"SCOPE","table":"TABLE_NAME","key":"TABLE_PRIMARY_KEY"}' | python -m json.tool

# coldtoken:
zeus get-table-row $KYLIN_TEST_ACCOUNT "accounts" $KYLIN_TEST_ACCOUNT "TEST" --endpoint $DSP_ENDPOINT | python -m json.tool
curl http://$DSP_ENDPOINT/v1/dsp/ipfsservice1/get_table_row -d '{"contract":"CONTRACT_ACCOUNT","scope":"CONTRACT_ACCOUNT","table":"accounts","key":"TEST"}' | python -m json.tool
```

## Get table - `get-table.js`
Reads all vRAM tables of a smart contract and stores them with the naming syntax: `${contract_name}-${table_name}-table.json`.  The script is located in the `utils/ipfs-service/get-table.js` of an unboxed zeus box.

Mandatory env variables:
```bash
# account name vRAM table exists on
export CONTRACT_NAME= 
# run script
node utils/ipfs-service/get-table
```

Optional env variables (if using non-local nodeos / IPFS instance):
```bash
# defaults to all vRam tables in the abi, can be used to target a specific table
export TABLE_NAME=
# defaults to localhost:8888, can be used to specify external nodeos instance
export NODEOS_ENDPOINT=
# defaults to localhost, can be used to specify external IPFS instance
export IPFS_HOST=
# defaults to 5001
export IPFS_PORT=
# defaults to http
export IPFS_PROTOCOL=
# defaults to 1024
export SHARD_LIMIT=
# defaults to false
# produces a ${contractName}-${tableName}-roots.json file which is the table's current entries
# also produces an ipfs-data.json which can be used to recreate the current state of the IPFS table
export VERBOSE=
```

Steps to produce `/ipfs-dapp-service/test1-test-table.json` file below:

```bash
npm i -g @liquidapps/zeus-cmd
zeus unbox ipfs-dapp-service
cd ipfs-dapp-service
zeus test -c
export CONTRACT_NAME=test1
node utils/ipfs-service/get-table
```

Expected output `/ipfs-dapp-service/test1-test-table.json`:

```json
[
  {
    "scope": "test1",
    "key": "2b02000000000000",
    "data": {
      "id": "555",
      "sometestnumber": "0"
    }
  },
  {
    "scope": "test1",
    "key": "0200000000000000",
    "data": {
      "id": "2",
      "sometestnumber": "0"
    }
  }
  ...
]
```

If `VERBOSE=true`, you will also get `test1-test-roots.json` and `ipfs-data.json`:

`test1-test-roots.json` - equivalent of `cleos get table test1 test1 test`
```json
[
  {
    "shard_uri": "01551220d0c889cbd658f2683c78a09a8161ad406dd828dadab383fdcc0659aa6dfed8dc",
    "shard": 3
  },
  {
    "shard_uri": "01551220435f234b3af595737af50ac0b4e44053f0b31d31d94e1ffe917fd3dfbc6a9d88",
    "shard": 156
  },
  ...
]
```

`ipfs-data.json` - produces all data necessary to recreate current state of the table, can be used for populating a DSP's IPFS cluster
```json
{
  "015512204cbbd8ca5215b8d161aec181a74b694f4e24b001d5b081dc0030ed797a8973e0": "01000000000000000000000000000000",
  "01551220b422e3b9180b32ba0ec0d538c7af1cf7ccf764bfb89f4cd5bc282175391e02bb": "77cc0000000000007f00000000000000",
  ...
}
```

## Get ordered keys - `get-ordered-keys.js`
Prints ordered vRAM table keys in ascending order account/table/scope.  This can be used to iterate over the entire table client side.  The script is located in the `utils/ipfs-service/get-ordered-keys.js` of an unboxed zeus box.

Mandatory env variables:
```bash
export CONTRACT_NAME=
export SCOPE=
export TABLE_NAME=
node utils/ipfs-service/get-ordered-keys
```

Optional env variables (if using non-local nodeos / IPFS instance):
```bash
# defaults to localhost:8888, can be used to specify external nodeos instance
export NODEOS_ENDPOINT=
# defaults to localhost, can be used to specify external IPFS instance
export IPFS_HOST=
# defaults to 5001
export IPFS_PORT=
# defaults to http
export IPFS_PROTOCOL=
# defaults to 1024
export SHARD_LIMIT=
# defaults to 10000
export IPFS_TIMEOUT=
```

Steps to produce console logged output below:

```bash
npm i -g @liquidapps/zeus-cmd
zeus unbox ipfs-dapp-service
cd ipfs-dapp-service
zeus test -c
export CONTRACT_NAME=test1
export SCOPE=test1
export TABLE_NAME=test
node utils/ipfs-service/get-ordered-keys
```

Expected output:

```bash
[ '0', '1', '2', '20', '555', '12345', '52343' ]
```

Querying table rows with Zeus or the [`dapp-client`](../developers/dapp-client#get-vram-row-get-vram-row-from-dsp-s-endpoint)'s `get_vram_row` call:

```bash
# zeus get-table-row <contract> <table> <scope> <key> <keytype> <keysize>
zeus get-table-row test1 test test1 52343 number 64
# output:
{"row":{"id":"0","sometestnumber":"0"}}
```

## Save load and reset dapp::multi_index data

To enable these features, you must include the advanced multi index with: `#define USE_ADVANCED_IPFS` at the top of the contract file. If you have already deployed a contract that does not use advanced features, do not add this line, as it is not backwards compatible.

With that, you now have the ability to save the list of shards currently being used to represent the table's current state.  With the saved snapshot, a developer can upload it to another contract, or version the snapshot and save it to be loaded to the contract later.  This adds much needed flexibility to maintaining database state in a vRAM table.

### Save dapp::multi_index data

Using zeus, a backup file can be created with the following command:

```bash
zeus backup-table <CONTRACT> <TABLE>

# optional flags:

--endpoint # endpoint of node
# default: localhost:13115
--output # output file name
# default: vram-backup-${CONTRACT}-${TABLE}-0-1578405972.json

# example
zeus backup-table lqdportfolio users --endpoint=http://kylin-dsp-2.liquidapps.io/
```

Example: vram-backup-lqdportfolio-users-0-1578405972.json

```json
{
  "contract": "lqdportfolio",
  "table": "users",
  "timestamp": "2020-01-07T14:06:12.339Z",
  "revision": 0,
  "manifest": {
    "next_available_key": 0,
    "shards": 1024,
    "buckets_per_shard": 64,
    "shardbuckets": [
      {
        "key": 2,
        "value": "015512202a1de9ce245a8d14b23512badc076aee71aad3aba30900e9c938243ce25b467d"
      },
      {
        "key": 44,
        "value": "015512205b43f739a9786fbe2c384c504af15d06fe1b5a61b72710f51932c6b62592d800"
      },
      ...
    ]
  }
}
```

### Load manifest

Once a manifest is saved, it can be loaded with the following smart contract action.

```cpp
[[eosio::action]] void testman(dapp::manifest man) {
  testindex_t testset(_self,_self.value);
  // void load_manifest(manifest manifest, string description)
  // description is description item in the backup table
  testset.load_manifest(man,"Test");
}
```

With a unit test:

```javascript
let manifest = {
  next_available_key: 556,
  shards: 1024,
  buckets_per_shard: 64,
  shardbuckets
}
await testcontract.testman({
  man: manifest
}, {
  authorization: `${code}@active`,
  broadcast: true,
  sign: true
});
```

## Clear table

By calling the clear command, a table's version is incremented via the `revision` param and the `next_available_key` is reset

```cpp
TABLE vconfig {
    checksum256 next_available_key = empty256;
    uint32_t shards = 0;
    uint32_t buckets_per_shard = 0;
    uint32_t revision = 0;
};
void clear() {
  vconfig_sgt vconfigsgt(_code,name(TableName).value);
  auto config = vconfigsgt.get_or_default();
  config.revision++;    
  config.next_available_key = empty256; //reset the next available key
  vconfigsgt.set(config,_code);
}
[[eosio::action]] void testclear() {
  testindex_t testset(_self,_self.value);
  testset.clear();
}
```