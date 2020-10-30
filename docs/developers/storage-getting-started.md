LiquidStorage Getting Started
====================

```  
  _      _             _     _  _____ _                             
 | |    (_)           (_)   | |/ ____| |                            
 | |     _  __ _ _   _ _  __| | (___ | |_ ___  _ __ __ _  __ _  ___ 
 | |    | |/ _` | | | | |/ _` |\___ \| __/ _ \| '__/ _` |/ _` |/ _ \
 | |____| | (_| | |_| | | (_| |____) | || (_) | | | (_| | (_| |  __/
 |______|_|\__, |\__,_|_|\__,_|_____/ \__\___/|_|  \__,_|\__, |\___|
              | |                                         __/ |     
              |_|                                        |___/      
```

LiquidStorage allows the read/write of data to a DAPP Service Provider's IPFS node.

## Prerequisites

* [Zeus](zeus-getting-started.md) - Zeus installs eos and the eosio.cdt if not already installed
* [Kylin Account](kylin-account.md)

## Unbox LiquidStorage DAPP Service box
This box contains the LiquidStorage example smart contract, DAPP Service Provider node logic, unit tests, [dapp-client](dapp-client.md) source code and examples.
```bash
mkdir storage-dapp-service; cd storage-dapp-service
# npm install -g @liquidapps/zeus-cmd
zeus box create
zeus unbox storage-dapp-service
zeus test -c
```

## LiquidStorage Consumer Example Contract used in unit tests
`./zeus_boxes/contracts/eos/storageconsumer/storageconsumer.cpp`

The consumer contract is a great starting point for playing around with the LiquidStorage service.  This sample contract uses LiquidAccounts as an option, but the service may also be used with regular EOS accounts.

```cpp
/* DELAY REMOVAL OF USER DATA INTO VRAM */
/* ALLOWS FOR QUICKER ACCESS TO USER DATA WITHOUT THE NEED TO WARM DATA UP */
#define VACCOUNTS_DELAYED_CLEANUP 120

/* ADD NECESSARY LIQUIDACCOUNT / VRAM INCLUDES */
#include "../dappservices/ipfs.hpp"
#include "../dappservices/multi_index.hpp"
#include "../dappservices/vaccounts.hpp"
#include <eosio/singleton.hpp>

/* ADD LIQUIDACCOUNT / VRAM RELATED ACTIONS */
#define DAPPSERVICES_ACTIONS()                                                 \
  XSIGNAL_DAPPSERVICE_ACTION                                                   \
  IPFS_DAPPSERVICE_ACTIONS                                                     \
  VACCOUNTS_DAPPSERVICE_ACTIONS
#define DAPPSERVICE_ACTIONS_COMMANDS()                                         \
  IPFS_SVC_COMMANDS() VACCOUNTS_SVC_COMMANDS()

#define CONTRACT_NAME() storageconsumer

CONTRACT_START()

/* THE storagecfg TABLE STORES STORAGE PARAMS */
TABLE storagecfg {
  // all measurements in bytes
  uint64_t max_file_size_in_bytes = UINT64_MAX; // max file size in bytes that can be uploaded at a time, default 10mb
  uint64_t global_upload_limit_per_day = UINT64_MAX; // max upload limit in bytes per day for EOS account, default 1 GB
  uint64_t vaccount_upload_limit_per_day = UINT64_MAX; // max upload limit in bytes per day for LiquidAccounts, default 10 MB
};
typedef eosio::singleton<"storagecfg"_n, storagecfg> storagecfg_t;

/* SET PARAMS FOR storagecfg TABLE */
ACTION setstoragecfg(const uint64_t &max_file_size_in_bytes,
                     const uint64_t &global_upload_limit_per_day,
                     const uint64_t &vaccount_upload_limit_per_day) {
  require_auth(get_self());
  storagecfg_t storagecfg_table(get_self(), get_self().value);
  auto storagecfg = storagecfg_table.get_or_default();

  storagecfg.max_file_size_in_bytes = max_file_size_in_bytes;
  storagecfg.global_upload_limit_per_day = global_upload_limit_per_day;
  storagecfg.vaccount_upload_limit_per_day = vaccount_upload_limit_per_day;

  storagecfg_table.set(storagecfg, get_self());
}

/* THE FOLLOWING STRUCT DEFINES THE PARAMS THAT MUST BE PASSED FOR A LIQUIDACCOUNT TRX */
struct dummy_action_hello {
  name vaccount;
  uint64_t b;
  uint64_t c;

  EOSLIB_SERIALIZE(dummy_action_hello, (vaccount)(b)(c))
};

/* DATA IS PASSED AS PAYLOADS INSTEAD OF INDIVIDUAL PARAMS */
[[eosio::action]] void hello(dummy_action_hello payload) {
  /* require_vaccount is the equivalent of require_auth for EOS */
  require_vaccount(payload.vaccount);

  print("hello from ");
  print(payload.vaccount);
  print(" ");
  print(payload.b + payload.c);
  print("\n");
}

/* EACH ACTION MUST HAVE A STRUCT THAT DEFINES THE PAYLOAD SYNTAX TO BE PASSED */
VACCOUNTS_APPLY(((dummy_action_hello)(hello)))

CONTRACT_END((hello)(regaccount)(setstoragecfg)(xdcommit)(xvinit))
```

## Compile

See the [unit testing](unit-testing) section for details on adding unit tests.

```bash
zeus compile
# test without compiling
zeus test
# compile and test with
zeus test -c
```

## Deploy Contract
```bash
export DSP_ENDPOINT=https://kylin-dsp-2.liquidapps.io
export KYLIN_TEST_ACCOUNT=<ACCOUNT_NAME>
export KYLIN_TEST_PUBLIC_KEY=<ACTIVE_PUBLIC_KEY>
# Buy RAM:
cleos -u $DSP_ENDPOINT system buyram $KYLIN_TEST_ACCOUNT $KYLIN_TEST_ACCOUNT "200.0000 EOS" -p $KYLIN_TEST_ACCOUNT@active
# Set contract code and abi
cleos -u $DSP_ENDPOINT set contract $KYLIN_TEST_ACCOUNT storageconsumer -p $KYLIN_TEST_ACCOUNT@active

# Set contract permissions, add eosio.code
cleos -u $DSP_ENDPOINT set account permission $KYLIN_TEST_ACCOUNT active "{\"threshold\":1,\"keys\":[{\"weight\":1,\"key\":\"$KYLIN_TEST_PUBLIC_KEY\"}],\"accounts\":[{\"permission\":{\"actor\":\"$KYLIN_TEST_ACCOUNT\",\"permission\":\"eosio.code\"},\"weight\":1}]}" owner -p $KYLIN_TEST_ACCOUNT@active
```

## Select and stake DAPP for DSP package | [DSP Portal Link](https://dsphq.io/packages?service%5B0%5D=liquidstorag&network=kylin)
 * Use [the faucet](https://kylin-dapp-faucet.liquidapps.io/) to get some DAPP tokens on Kylin
 * Information on: [DSP Packages and staking DAPP/DAPPHDL (AirHODL token)](dsp-packages-and-staking.md)
```bash
export PROVIDER=heliosselene
export PACKAGE_ID=storagelarge

# select your package: 
export SERVICE=liquidstorag
cleos -u $DSP_ENDPOINT push action dappservices selectpkg "[\"$KYLIN_TEST_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"$PACKAGE_ID\"]" -p $KYLIN_TEST_ACCOUNT@active

# Stake your DAPP to the DSP that you selected the service package for:
cleos -u $DSP_ENDPOINT push action dappservices stake "[\"$KYLIN_TEST_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"10.0000 DAPP\"]" -p $KYLIN_TEST_ACCOUNT@active
```

## Set LiquidStorage Configuration
The `setstoragecfg` table allows setting limits to the LiquidStorage service for users.  This is an optional step.

```cpp
TABLE storagecfg {
  // all measurements in bytes
  uint64_t max_file_size_in_bytes = UINT64_MAX; // max file size in bytes that can be uploaded at a time, default 10mb
  uint64_t global_upload_limit_per_day = UINT64_MAX; // max upload limit in bytes per day for EOS account, default 1 GB
  uint64_t vaccount_upload_limit_per_day = UINT64_MAX; // max upload limit in bytes per day for LiquidAccounts, default 10 MB
};
```

Here we will set the maximum file size to 1MB and the global upload limits to 10MB each.  You can update the `YOUR_ACCOUNT_HERE` to your consumer account.

```bash
cleos -u https://kylin.eos.dfuse.io push transaction '{"delay_sec":0,"max_cpu_usage_ms":0,"actions":[{"account":"YOUR_ACCOUNT_HERE","name":"setstoragecfg","data":{"max_file_size_in_bytes":1000000,"global_upload_limit_per_day":100000000,"vaccount_upload_limit_per_day":100000000},"authorization":[{"actor":"YOUR_ACCOUNT_HERE","permission":"active"}]}]}'
```

## Test
This test will use a regular EOS account instead of a LiquidAccount, to use a LiquidAccount, visit the [LiquidAccount](vaccounts-getting-started) section for how to send a transaction.

First install the [`dapp-client`](dapp-client)

```bash
npm install -g @liquidapps/dapp-client
```

Create the following file:

```bash
touch test.js
# add contents below
vim test.js
# run file
node test.js
```

```js
const { createClient } = require('@liquidapps/dapp-client');
const fetch = require('isomorphic-fetch');
const endpoint = "https://kylin-dsp-2.liquidapps.io";
const getClient = () => createClient( { network:"kylin", httpEndpoint: endpoint, fetch });

(async () => {
    const service = await (await getClient()).service('storage', "YOUR_ACCOUNT_HERE");
    const data = Buffer.from("a great success", "utf8");
    const key = "YOUR_ACTIVE_PRIVATE_KEY_HERE";
    const permission = "active";
    const options = {
      // if true, DAG leaves will contain raw file data and not be wrapped in a protobuf
      rawLeaves: true
    };
    const response = await service.upload_public_file(
        data,
        key,
        permission,
        null,
        options
    );
    console.log(`response uri: ${response.uri}`);
})().catch((e) => { console.log(e); });
```

```bash
# node test.js 
response uri: ipfs://IPFS_URI_HERE
```

This will return the IPFS URI where the content can be fetched.

Add the URI to the end of the IPFS gateway: `https://ipfs.io/ipfs/IPFS_URI_HERE`, and you will see "a great success".

To fetch data, the following example can be used:

```js
const fetch = require("node-fetch");

(async () => {
    let res = await fetch('https://kylin-dsp-2.liquidapps.io/v1/dsp/liquidstorag/get_uri', {
      method: 'POST',
      mode: 'cors',
      body: JSON.stringify({ uri: "ipfs://zb2rhX28fttoDTUhpmHBgQa2PzjL1N3XUDaL9rZvx8dLZseji" })
    });
    res = await res.json();
    res = Buffer.from(res.data, 'base64').toString(),
    console.log(`result: ${res}`);
})().catch((e) => { console.log(e); });
// result: a great success
```

To see additional examples of the other `dapp-client` services, see the examples folder [here](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/storage-dapp-service/client/examples).

## Zeus commands

There are 2 Zeus commands available, upload, and unpin.  These can be used in the root of the storage box.  Upload will upload an archive (.tar), a directory, or a file.  The unpin command unpins data from the IPFS node.

```bash
zeus storage upload <ACCOUNT_NAME> package.json <ACCOUNT_PRIVATE_KEY>
zeus storage unpin <ACCOUNT_NAME> <IPFS_URI_RETURNED_ABOVE> <ACCOUNT_PRIVATE_KEY>
```